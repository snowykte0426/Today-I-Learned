# C언어 프로젝트 수행평가 준비
+ 체육대회로 인해 30여분 밖에 공부하지 못함
+ game_start.c
    ```c
    #include "superclass.h"
    #pragma comment (lib,"libmariadb.lib")  
    #pragma comment(lib, "user32.lib")

    void ASCII_Art_print(void) {
        puts("\n\n\n");
        printf(" _____     _____    ___  ___   \n");
        printf("|  __ \\   /  ___|   |  \\/  |   \n");
        printf("| |  \\/   \\ `--.    | .  . |   \n");
        printf("| | __     `--. \\   | |\\/| |   \n");
        printf("| |_\\ \\ _ /\\__/ / _ | |  | | _ \n");
        printf(" \\____/(_)\\____/ (_)|_|  |_/(_)\n");
        printf("                                \n");  
    }

    void Program_config(void) {
        CursorView(0);
        system("COLOR 0F");
        system("mode con cols=250 lines=50 | title Gwangju Sword Master");
    }

    void gotoxy(int x, int y) {
        HANDLE consoleHandle = GetStdHandle(STD_OUTPUT_HANDLE);
        COORD pos = { NULL };
        pos.X = x;
        pos.Y = y;
        SetConsoleCursorPosition(consoleHandle, pos);
    }

    void loginmenuDraw(void) {
        int x = 55, y = 16;
        gotoxy(55 - 2, 16);
        printf("> 로그인");
        gotoxy(55, 17);
        printf("회원가입");
        gotoxy(55, 18);
        printf("종료");
        while (1) {
            int n = KeyControl();
            switch (n) {
                case UP: {
                    system("cls");
                    if (y > 16) {
                        gotoxy(x - 2, y);
                        printf(" ");
                        gotoxy(x - 2, --y);
                        printf(">");
                    }
                    break;
                }
                case DOWN: {
                    system("cls");
                    if (y < 18) {
                        gotoxy(x - 2, y);
                        printf(" ");
                        gotoxy(x - 2, ++y);
                        printf(">");
                    }
                    break;
                }
                case SUBMIT: {
                    system("cls");
                    return y - 16;
                }
            }
        }
    }

    int KeyControl(void) {
        int POS = 0;
        int prevPOS = -1;
        char temp = getch();
        if (temp == GetAsyncKeyState(VK_UP) & 0x8000 && POS != prevPOS) {
            return UP;
        }
        else if (temp == GetAsyncKeyState(VK_DOWN) & 0x8000 && POS != prevPOS) {
            return DOWN;
        }
        else if (temp == ' ') {
            return SUBMIT;
        }
    }

    void CursorView(char show) {
        HANDLE hConsole;
        CONSOLE_CURSOR_INFO ConsoleInfo = { 0, };
        hConsole = GetStdHandle(STD_OUTPUT_HANDLE);
        ConsoleInfo.dwSize = 1;
        ConsoleInfo.bVisible = show;
        SetConsoleCursorInfo(GetStdHandle(STD_OUTPUT_HANDLE), &ConsoleInfo);
    }
    ```