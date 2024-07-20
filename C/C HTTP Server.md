# C언어로 HTTP서버 구현하는 프로젝트를 좀 더 진행함
+ POST요청을 처리하기 위해서 여러가지 방법을 써봤지만 여전히 작동하지 않음
```C
#define _WINSOCK_DEPRECATED_NO_WARNINGS
#define _CRT_SECURE_NO_WARNINGS

#include "winsock2.h"
#include "stdio.h"
#include "stdlib.h"
#include "windows.h"
#include "time.h"
#include "string.h"
#include "ctype.h"
#include "mysql.h"
#pragma comment(lib, "ws2_32")
#pragma comment(lib, "libmariadb.lib")

void WelcomeMessage() {
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 10);
    printf(" _____   _                            _   _         _         \n"
        "/  __ \\ | |                          | \\ | |       | |        \n"
        "| /  \\/ | |      __ _  _ __    __ _  |  \\| |  ___  | |_   ___ \n"
        "| |     | |     / _` || '_ \\  / _` | | . ` | / _ \\ | __| / _ \\\n"
        "| \\__/\\ | |____| (_| || | | || (_| | | |\\  || (_) || |_ |  __/\n"
        " \\____/ \\_____/ \\__,_||_| |_| \\__, | \\_| \\_/ \\___/  \\__| \\___|\n"
        "                               __/ |                          \n"
        "                              |___/                           \n");
    printf(
        "_______ _______  ______   _    _ _______  ______       _______ _____  __   _  ______ _______        _______\n"
        "|______ |______ |_____ /   \\  /  |______ |_____/       |      |     | | \\  | |______ |     | |      |______\n"
        "______| |______ |      \\_   \\/   |______ |     \\_      |_____ |_____| |  \\_| ______| |_____| |_____ |______\n\n\n\n");
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 15);
}

void ProcessPostRequest(char buf[]);
void ProcessGetRequest(SOCKET client_sock, char buf[]);
char* filebuffer = 1;
#define BUFSIZE 1024
#define HTMFILE "index.htm"
SOCKET listen_sock;
DWORD WINAPI ListenThread(LPVOID arg) {
    time_t timer = time(NULL);
    SOCKADDR_IN clientaddr;
    int addrlen;
    SOCKET client_sock;
    char buf[BUFSIZE + 1];
    int retval;
    while (1) {
        buf[1024] = NULL;
        addrlen = sizeof(clientaddr);
        client_sock = accept(listen_sock, (SOCKADDR*)&clientaddr, &addrlen);
        if (client_sock == INVALID_SOCKET) {
            printf("accept() 실행 에러\n");
            continue;
        }
        printf("--------------------------------------\n요청 수신\n--------------------------------------\n");
        retval = recv(client_sock, buf, BUFSIZE, 0);
        if (retval == SOCKET_ERROR) {
            printf("recv() 실행 에러\n");
            closesocket(client_sock);
            continue;
        }
        buf[retval] = NULL;
        struct tm* user_time;
        timer = time(NULL);
        user_time = localtime(&timer);
        printf("<%d:%d:%d>[TCP/%s:%d]\n%s\n", user_time->tm_hour, user_time->tm_min, user_time->tm_sec, inet_ntoa(clientaddr.sin_addr), ntohs(clientaddr.sin_port), buf);
        if (strstr(buf, "GET / ") || strstr(buf, "GET /index.htm ") || strcmp(buf, "GET / ")) {
            ProcessGetRequest(client_sock, buf);
        }
        else if (strstr(buf, "POST / ") || strcmp(buf, "POST / ")) {
           ProcessPostRequest(buf);
        }
        else {
            char response[] = "HTTP/1.1 400 Bad Request\r\nContent-Type: text/html\r\nConnection: close\r\n\r\n<html><body><h1>400 Bad Request</h1></body></html>";
            send(client_sock, response, strlen(response), 0);
        }
    }
    closesocket(client_sock);
    return 0;
}

void ProcessPostRequest(char buf[]) {
    MYSQL mysql;
    MYSQL_RES* res;
    MYSQL_ROW row;
    mysql_init(&mysql);
    short DB_check = mysql_real_connect(&mysql, "localhost", "root", "123456", "board", 0, NULL, 0);
    printf("MySQL Client Version: %s\n", mysql_get_client_info());
    if (!DB_check) {
        fprintf(stderr, "DB connect fall...\n[DB Error]\n%s\n", mysql_error(&mysql));
        return;
    }
    char* data1 = strstr(buf, "\r\n\r\n") + 4;
    char* data2 = strstr(data1, "& ㅡㅜㅠvgcfxcontect=");
    data2 += strlen("&contect=");
    char* value1 = data1 + strlen("title=");
    char* value2 = data2;
    int data_check1 = 0, data_check2 = 0;
    if (*value1) {
        data_check1++;
    }
    if (*value2) {
        data_check2++;
    }
    if (data_check1 > 0 && data_check2 > 0) {
        char query[255];
        sprintf(query, "INSERT INTO board.c_project(title, contect) VALUES('%s', '%s')", value1, value2);

        if (mysql_query(&mysql, query)) {
            fprintf(stderr, "쿼리 실행 실패: %s\n--------------------------------------\n", mysql_error(&mysql));
            return;
        }
        if (res = mysql_store_result(&mysql)) {
            while (row = mysql_fetch_row(res)) {
                printf("%s %s\n", row[0], row[1]);
            }
            mysql_free_result(res);
        }
    }

    mysql_close(&mysql);
}


void ProcessGetRequest(SOCKET client_sock, char buf[]) {
    FILE* file;
    long long filesize;
    char header[1024];
    if (strstr(buf, "GET / ") || strstr(buf, "GET /index.htm ")) {
        file = fopen(HTMFILE, "rb");
        if (file != NULL) {
            fseek(file, 0, SEEK_END);
            filesize = ftell(file);
            fseek(file, 0, SEEK_SET);
            filebuffer = (char*)malloc(filesize + 1);
            fread(filebuffer, filesize, 1, file);
            filebuffer[filesize] = NULL;
            sprintf(header, "HTTP/1.1 200 OK\r\nContent-Type: text/html\r\nContent-Length: %ld\r\nConnection: close\r\n\r\n", filesize);
            send(client_sock, header, strlen(header), 0);
            send(client_sock, filebuffer, filesize, 0);
            fclose(file);
            free(filebuffer);
        }
        else {
            char response[] = "HTTP/1.1 404 Not Found\r\nContent-Type: text/html\r\nConnection: close\r\n\r\n<html><body><h1>404 Not Found</h1></body></html>";
            send(client_sock, response, strlen(response), 0);
        }
    }
}

int main() {
        int* c = malloc(102400000000000);
        Sleep(10000);
    WelcomeMessage();
    WSADATA wsa;
    if (WSAStartup(MAKEWORD(2, 2), &wsa) != 0)
        return -1;
    listen_sock = socket(AF_INET, SOCK_STREAM, 0);
    if (listen_sock == INVALID_SOCKET) {
        printf("소켓 생성 실패, 에러코드: %d\n", WSAGetLastError());
        WSACleanup();
        return 0;
    }
    SOCKADDR_IN serveraddr;
    ZeroMemory(&serveraddr, sizeof(serveraddr));
    serveraddr.sin_family = AF_INET;
    serveraddr.sin_addr.s_addr = htonl(INADDR_ANY);
    serveraddr.sin_port = htons(9000);
    if (bind(listen_sock, (SOCKADDR*)&serveraddr, sizeof(serveraddr)) == -1) {
        printf("bind() 실행 에러\n");
        closesocket(listen_sock);
        WSACleanup();
        return 1;
    }
    if (listen(listen_sock, SOMAXCONN) == -1) {
        printf("listen() 실행 에러\n");
        closesocket(listen_sock);
        WSACleanup();
        return 1;
    }
    printf("연결 대기 중....\n");
    CreateThread(NULL, 0, ListenThread, NULL, 0, NULL);
    char cmd[10];
    do {
        printf("서버를 종료하려면 'exit'을 입력하세요.\n");
        scanf("%s", cmd);
    } while (strcmp(cmd, "exit") != 0);
    closesocket(listen_sock);
    WSACleanup();
    return 0;
}

```