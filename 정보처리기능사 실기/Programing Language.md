# 정보처리기능사 실기 공부
+ **기본 용어**
    + 식별자: 프로그램의 구성요소를 **구별하기 위한 기준**
    + 바인딩: 변수와 변수의 **속성을 연결하는 과정**
        + 정적 바인딩: 프로그램 실행 **전**에 속성을 연결하는 방식
        + 동적 바인딩: 프로그램 실행 **중**에 속성을 연결하는 방식
    + 선언: 변수에 이름,자료형 등의 **속성을 부여**하는 작업
        + 묵시적 선언: **선언문 없이 기본 규칙에 따라** 속성을 부여하는 방식
        + 명시적 선언: **선언문을 이용하여** 속성을 부여하는 방식
    + 영역: 이름이 사용되는 **범위**
        + 정적 영역: 변수를 찾을 때 **구조**에 기반하는 방식
        + 동적 영역: 변수를 찾을 때 구조보다 **순서**에 기반하는 방식
    + 할당: 변수에 **메모리 공간**을 바인딩하는 작업
    + 데이터 타입(자료형): 변수가 가질 수 있는 **속성값**의 길이 및 성질을 의미
    + 연산자: 데이터 처리를 위해 연산을 표현하는 **기호**로 **+**,**-** 등과 같은 연산자를 포함
    + 명령문: 프로그램을 **구성**하는 **문장**
+ **서식 지정자**
    + **변수** 또는 **값**을 출력문을 통해 출력하기 위해 사용
    + ``printf``문은 **문자열**만을 출력하며, **이스케이프문**과 같이 문자열 선언을 탈출하여 변수를 **인식**시키는데 사용

        ||||
        |---|---|---|
        |**정수형**|%d|부호 있는 10진 정수|
        |**정수형**|%u|부호 없는 10진 정수|
        |**정수형**|%o|부호 없는 8진 정수|
        |**정수형**|%x|부호 없는 16진 정수|
        |**실수형**|%f|소수점 6자리까지의 실수|
        |**실수형**|%e|실수의 지수 표현 ``ex)0.000000e+00``|
        |**실수형**|%g|숫자 값의 크기에 따라 ``%e``나 ``%f``를 선택|
        |**문자형**|%c|단일 문자 출력|
        |**문자형**|%s|문자열 출력|
+ **정렬**
    + 선택정렬
        ```c
        #include <stdio.h>

        void main(void) {
            int ary[5] = { 47, 95, 64, 21, 66 };
            int temp;
            for (int i = 0; i < 5; i++) {
                for (int j = 0; j < i; j++) {
                    if (ary[i] < ary[j]) {
                        temp = ary[i];
                        ary[i] = ary[j];
                        ary[j] = temp;
                    }
                }
                for (int x = 0; x < 5; x++) {
                    printf("%d ", ary[x]);
                }
                printf("\n");
            }
        }
        ```
        ```
        47 95 64 21 66
        47 95 64 21 66
        47 64 95 21 66
        21 47 64 95 66
        21 47 64 66 95
        ```
    + 버블정렬
        ```c
        #include <stdio.h>
        
        void main(void) {
            int ary[5] = { 47, 95, 64, 21, 66 };
            int temp;
            for (int i = 0; i < 5; i++) {
                for (int j = 0; j < i; j++) {
                    if (ary[j] < ary[j + 1]) { 
                        temp = ary[j];
                        ary[j] = ary[j + 1];
                        ary[j+1] = temp;
                    }
                }
                for (int x = 0; x < 5; x++) {
                    printf("%d ", ary[x]);
                }
                printf("\n");
            }
        }
        ```
        ```
        47 95 64 21 66
        95 47 64 21 66
        95 64 47 21 66
        95 64 47 21 66
        95 64 47 66 21
        ```

    + 삽입정렬
        ```java
        public class Path {

            public static void main(String[] args) {
                int[] arr = { 4, 6, 9, 1, 2 };
                insertionSort(arr);
            }

            public static void insertSort(int[] arr) {
                int n = arr.length;
                for (int i = 1; i < n; i++) {
                    int key = arr[i];
                    int j = i - 1;
                    while (j >= 0 && arr[j] > key) {
                        arr[j + 1] = arr[j];
                        j--;
                    }
                    arr[j + 1] = key;
                    for (int element : arr) {
                        System.out.print(element + " ");
                    }
                    System.out.println();
                }
            }
        }
        ```
        ```
        4 6 9 1 2
        4 6 9 1 2
        1 4 6 9 2
        1 2 4 6 9
        ```
+ **프로그래밍 언어별 특성**
    + FORTRAN
        ``` FORTRAN
        program main
            write(*,*) "Hello World!"
        end program main
        ```
        + **1954년**에 초기 버전이 개발된 언어로 **시스템 의존적**이고 컴퓨터 시스템 관련 지식을 많이 요구
    + COBOL
        ```COBOL
        IDENTIFICATION DIVISION.
        PROGRAM-ID. HELLO-WORLD.
        PROCEDURE DIVISION.
        DISPLAY 'Hello, World'
        STOP RUN.
        ```
        + 1960년에 **미국 국방성**에 의해 개발
        + **순차적 방식**의 언어로 **웹 응용 프로그램**과 쉽게 통합이 가능
        + 자료 구조의 **선언부**와 **실행부**를 분리
    + PASCAL
        ```PASCAL
        program Hello;
        begin
        writeln ('Hello, world.');
        end.
        ```
        + 잘 짜인 구조로 인해 프로그래밍 언어로써 성공했지만 **분리 컴파일**과 문자열의 **적절한 처리** 등을 제공하지 **못함**
        + **사용자 정의 추상화** 기능은 제공하나 **정보 은닉 기능**이 없어 **현대적 프로그래밍 기법**을 적용하기에는 부적절
    + C
        ```C
        #include <stdio.h>

        void main(void) {
            printf("Hello, world!\n");
            return 0;
        }
        ```
        + 1972년에 개발된 언어로 **UNIX 운영체제** 구현에 사용
        + 문법의 간결성, 효율적 실행 등의 이유로 **가장 많이 사용되는 시스템 프로그래밍 언어**라는 칭호를 얻음
    + C++
        ```cpp
        import std;

        int main() {
            std::println("Hello, world!");
        }
        ```
        + **C를 발전시킨 언어**로 클래스,상속 등을 제공하는 **객체 지향 언어**
        + 대형 프로젝트 수행 시 **모듈별 분리**가 가능하여 **개발**과 **유지관리**에 적합
    + Java
        ```java
        public class HelloWorld {
            public static void main(String[] args) {
                System.out.println("Hello, World!");
            }
        }
        ```
        + C++에 비해 **단순하고 분산환경 및 보완성 지원**
        + Java는 **컴파일**을 반드시 거쳐야 하며 컴파일을 통해 생성된 **Class 파일**을 **가상 머신**으로 실행
    + JavaScript
        ```js
        console.log("Hello, world!");
        ```
        + **객체 지향 스크립트 언어**로 **웹 페이지 동작**을 구현
        + **확장성**이 좋고 코드를 완성하는데 걸리는 시간이 적으며 쉽게 배운다는 장점이 있지만 **보안**,**성능** 등이 타 언어에 비해 **부족**
    + Perl
        ```perl
        use strict;
        use warnings;
        
        print "Hello, World!\n"
        ```
        + **인터프러터 언어**로 **CGI용**으로 많이 사용
        + 변수를 **명시적으로 선언할 필요가 없음**
    + Python
        ```py
        print("Hello, world!\n")
        ```
        + 배우기 쉽고 **이식성**이 좋은 언어이며 **다양한 함수**도 많이 제공
        + **인터프러터 언어**이자 **객체 지향 스크립트 언어**임
    + C#
        ```cs
        Console.WriteLine("Hello, World");
        ```
        + 2000년에 **.NET 프레임워크** 환경에 맞추어 개발된 언어
        + **.NET 환경**과 **C# 컴파일러**를 요구
    + GoLang
        ```go
        package main

        import "fmt"

        func main() {
            fmt.Println("Hello, world!")
        }
        ```
        + **Go**라고도 부르며 **Google**에서 2009년에 제작한 언어
        + C와 **직접적인 연관**을 가지며 **내장 라이브러리**를 많이 지원
        + 하드웨어 사양이 부족해도 빠른 컴파일이 가능
    + Dart
        ```dart
        void main() {
            print("Hello, World");
        }
        ```
        + JavaScript와 Java의 영향을 받아 개발되었으며 **객체 지향적인 언어**
        + **백그라운드**에서 작동되며 JavaScript와 유사하나 **단순화** 됨
        + **HTML 페이지**를 별도의 라이브러리 없이 **수정** 가능
    + Ceylon
        ```ceylon
        shared void run() {
            print("Hello, World!");
        }
        ```
        + Java에 기반을 둔 언어로 **모듈성**을 주요 특징으로 가짐
        + **패키지**와 **모듈**로 가상 머신에서 컴파일 하며, **Ceylon Herd**라는 저장소에서 모듈을 **발행**