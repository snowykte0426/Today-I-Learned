# C언어 프로젝트 수행평가 준비
+ 당장은 텍스트RPG(Text RPG)로 개발 방향을 정했음
+ API.c
  ```c
   #include "main.h"

    void SetColor(int color) {
    SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), color);
    }

    void CursorView(char show) {
        HANDLE hConsole;
        CONSOLE_CURSOR_INFO ConsoleCursor;
    hConsole = GetStdHandle(STD_OUTPUT_HANDLE);
    ConsoleCursor.bVisible = show;
    ConsoleCursor.dwSize = 1;
    SetConsoleCursorInfo(hConsole, &ConsoleCursor);
    }   

    void gotoxy(int x, int y) {
    COORD pos = { x, y };
    SetConsoleCursorPosition(GetStdHandle(STD_OUTPUT_HANDLE), pos);
    }

    void box_print(void) {
    putsputs("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
    puts("■                                                                                                      ■");
    puts("■                                                                                                      ■");
    puts("■                                                                                                      ■");
    puts("■                                                                                                      ■");
    puts("■                                                                                                      ■");
    puts("■                                                                                                      ■");
    puts("■                                                                                                      ■");
    printf("■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■");
    }
   ```
+ main.c
    ```c
    #define _CRT_SECURE_NO_WARNINGS
    #include "main.h"

    void loginMenu(void);
    void login(void);
    void signup(void);
    void mainMenu(void);
    void gameMenu(void);

    int main(void) {
        CursorView(0);
        system("COLOR 0F");
        loginMenu();
        int POS = 0;
        while (true) {
            if (GetAsyncKeyState(VK_LEFT) & 0x8000) {
                cls;
                box_print();
                if (POS == 0) POS = 1;
                else POS -= 1;
            }
            else if (GetAsyncKeyState(VK_RIGHT) & 0x8000) {
                cls;
                box_print();
                if (POS == 1) POS = 0;
                else POS += 1;
            }
            else if (GetAsyncKeyState(VK_RETURN) & 0x8000) {
                cls;
                box_print();
                break;
            }
            switch (POS) {
            case 0:
                SetColor(11);
                gotoxy(35, 4); printf("> 로그인");
                SetColor(15);
                gotoxy(85, 4); printf("회원가입");
                break;
            case 1:
                gotoxy(35, 4); printf("로그인");
                SetColor(11);
                gotoxy(85, 4); printf("> 회원가입");
                SetColor(15);
                break;
            default: break;
            }   
            Sleep(150);
        }
        cls;
        if (POS == 0) login();
        else if (POS == 1) signup();
        gameMenu();
        return 0;
    }   

    void loginMenu(void) {
        system("mode con: cols=120 lines=9");
        system("title 로그인 및 회원가입");
        box_print();
        gotoxy(35, 4); printf("로그인");
        gotoxy(85, 4); printf("회원가입");
        return;
    }

    void login(void) {
        char username[50], password[50];
        cls;
        printf("로그인\n");
        printf("아이디: ");
        scanf("%s", username);
        printf("비밀번호: ");
        scanf("%s", password);
        printf("로그인 성공!\n");
        Sleep(2000);
    }

    void signup(void) {
        char username[50], password[50], confirmPassword[50];
        cls;
        printf("회원가입\n");
        printf("아이디: ");
        scanf("%s", username);
        printf("비밀번호: ");
        scanf("%s", password);
        printf("비밀번호 확인: ");
        scanf("%s", confirmPassword);
        if (strcmp(password, confirmPassword) == 0) {
            printf("회원가입 성공!\n");
    }
    else {
        printf("비밀번호가 일치하지 않습니다.\n");
    }
    Sleep(2000);
    }

    void gameMenu(void) {
        int POS_Menu = 0;
        CursorView(0);
        system("COLOR 0F");
        mainMenu();
        while (true) {
            if (GetAsyncKeyState(VK_LEFT) & 0x8000) {
                cls;
                box_print();
                if (POS_Menu == 0) POS_Menu = 3;
                else POS_Menu -= 1;
            }
            else if (GetAsyncKeyState(VK_RIGHT) & 0x8000) {
                cls;
                box_print();
                if (POS_Menu == 3) POS_Menu = 0;
                else POS_Menu += 1;
            }
            else if (GetAsyncKeyState(VK_RETURN) & 0x8000) {
                cls;
                box_print();
                break;
            }
            switch (POS_Menu) {
            case 0:
                SetColor(11);
                gotoxy(10, 4); printf("> 새로  하기");
                SetColor(15);
                gotoxy(35, 4); printf("이어  하기");
                gotoxy(60, 4); printf("랭킹  보기");
                gotoxy(85, 4); printf("게임  종료");
                break;
            case 1:
                gotoxy(10, 4); printf("새로  하기");
                SetColor(11);
                gotoxy(35, 4); printf("> 이어  하기");
                SetColor(15);
                gotoxy(60, 4); printf("랭킹  보기");
                gotoxy(85, 4); printf("게임  종료");
                break;
            case 2:
                gotoxy(10, 4); printf("새로  하기");
                gotoxy(35, 4); printf("이어  하기");
                SetColor(11);
                gotoxy(60, 4); printf("> 랭킹  보기");
                SetColor(15);
                gotoxy(85, 4); printf("게임  종료");
                break;
            case 3:
                gotoxy(10, 4); printf("새로  하기");
                gotoxy(35, 4); printf("이어  하기");
                gotoxy(60, 4); printf("랭킹  보기");
                SetColor(11);
                gotoxy(85, 4); printf("> 게임  종료");
                SetColor(15);
                break;
            default: break;
                }
            Sleep(150);
            }
        cls;
        if (POS_Menu == 0) puts("새로 하기를 눌렀습니다.");
        else if (POS_Menu == 1) puts("이어 하기를 눌렀습니다.");
        else if (POS_Menu == 2) puts("랭킹 보기를 눌렀습니다.");
        pause;
    }   

    void mainMenu(void) {
        system("mode con: cols=106 lines=9");
        system("title 광주소드마스터고등학교(Gwang Sword Master Highschool");
        box_print();
        gotoxy(10, 4); printf("새로  하기");
        gotoxy(35, 4); printf("이어  하기");
        gotoxy(60, 4); printf("랭킹  보기");
        gotoxy(85, 4); printf("게임  종료");
        return;
    }
    ```
+ main.h
    ```c
    #pragma once

    #include "stdio.h"
    #include "windows.h"
    #include "stralign.h"
    #include "stdlib.h"
    #include "conio.h"
    #include "stdbool.h"

    #define pause system("pause > nul")
    #define cls system("cls")

    void SetColor(int color);
    void CursorView(char show);
    void gotoxy(int x, int y);
    void box_print(void);
    void login(void);
    void signup(void);
    void gameMenu(void);
    void loginMenu(void);
    void mainMenu(void);    
    ```
### C언어 구조체,연결리스트 공부
+ 구조체배열,포인터 구조체 등을 공부함
```c
#include "stdio.h"

struct student {
    int number;
    char name[20];
    double grade;
};

int main(void) {
    struct student s = {2007, "홍길동", 4.3};
    struct student *p;
    p = &s;
    printf("학번=%d 이름=%s 키=%f\n", s.number, s.name, s.grade);
    printf("학번=%d 이름=%s 키=%f\n", (*p).number, (*p).name, (*p).grade);
    printf("학번=%d 이름=%s 키=%f\n", p->number, p->name, p->grade);
}
```
### C언어 화면구현 공부