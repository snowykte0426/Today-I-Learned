# C언어 프로젝트 수행평가 준비
+ 로그인 기능을 구현하려고 시도함
+ db.h
  ```c
   #pragma once

    #include "mysql.h"
    #include "mariadb/ma_io.h"

    unsigned short sign_up_db(char id[], char password[]);
    unsigned short sign_in_db(char id[], char password[]);
    void db_close(MYSQL* conn);
    ```
+ DB_Query.c
    ```c
    #define _CRT_SECURE_NO_WARNINGS
    #include "db.h"
    #include <mysql.h>
    #include <stdio.h>

    void db_close(MYSQL* conn) {
        mysql_close(conn);
    }

    unsigned short sign_up_db(char id[], char password[]) {
        MYSQL mysql;
        if (!mysql_real_connect(&mysql, "localhost", "root", "123456", "gwangju_sword_master", 0, NULL, 0)) {
            fprintf(stderr, "Server connection failed...: %s\n", mysql_error(&mysql));
            db_close(&mysql);
            return 101;
        }
        char query[255];
        sprintf(query, "INSERT INTO gwangju_sword_master(id, password) VALUES('%s', '%s')", id, password);
        if (mysql_query(&mysql, query)) {
            fprintf(stderr, "Database query failed...: %s\n", mysql_error(&mysql));
            db_close(&mysql);
            return 102;
        }
        db_close(&mysql);
        return 100;
    }

    unsigned short sign_in_db(char id[], char password[]) {
        MYSQL mysql;
        if (!mysql_real_connect(&mysql, "localhost", "root", "123456", "gwangju_sword_master", 0, NULL, 0)) {
            fprintf(stderr, "Server connection failed...: %s\n", mysql_error(&mysql));
            db_close(&mysql);
            return 101;
        }
        char query[255];
        sprintf(query, "SELECT id, password FROM gwangju_sword_master WHERE id = '%s' AND password = '%s'", id, password);
        if (mysql_query(&mysql, query)) {
            fprintf(stderr, "Database query failed...: %s\n", mysql_error(&mysql));
            db_close(&mysql);
            return 102;
        }
        MYSQL_RES* result = mysql_store_result(&mysql);
        if (mysql_num_rows(result) == 1) {
            mysql_free_result(result);
            db_close(&mysql);
            return 100;
        }
        else {
            mysql_free_result(result);
            db_close(&mysql);
            return 98;
        }
    }
    ```
