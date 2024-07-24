# 관계형 데이터베이스

DB에는 다양한 형태가 있다. 관계형, 키-값형, 객체형 등이 그 주인공인데 이 문서(TIL)에서는 **관계형 데이터베이스(RDB; Relational DataBase)** 에 관해 설명한다.

## 관계형 데이터베이스란?
![RDBMS](https://velog.velcdn.com/images/choijaehyeokk/post/5d76cf78-1f24-44e8-bfef-565eff096a47/rdbms.png)

+ 관계형 데이터베이스는 **행(Row, Record)** 과 **열(Column, Field)** 로 구성된 **표** 형식으로 데이터를 저장하며 데이터의 종속성을 **관계(Relation)** 로 표현하는 방식이다.
+ 컬럼의 구조와 데이터의 관계는 테이블 **스키마(Schema)** 로 사전 정의된다.
+ 각 데이터의 **생성**,**수정**,**조회**,**삭제**를 위해 **SQL**을 사용한다

![Sheet](https://velog.velcdn.com/images%2Fchoijaehyeokk%2Fpost%2Fd0e4d03e-2b2f-4a15-9cfe-c0968d336b5e%2FUntitled.png)
+ 대표적인 RDBMS에는 **<span style="color:blue">My</span><span style="color:orange">SQL</span>**, **Postgre<span style="color:skyblue">SQL</span>**, **<span style="color:red">Oracle</span> Database** 등이 존재한다.

## 키(keys)
+ ### Primary Key(기본 키)
    + 테이블에서 가장 **기본적인 속성**을 의미한다.
    + 테이블에서 **특정한 레코드를 식별**하는데 사용되는 값이다.
    + 기본 키로 설정하기 위해선 2가지 제약이 필수적인데
        + ``NOT NULL`` : **NULL 값**을 가질 수 없다.
        + ``UNIQUE`` : 다른 레코드와 **중복될 수 없다**.
    ```SQL
    # 기본 키 생성 예

    CREATE TABLE Persons
    (
        UserId INT PRIMARY KEY, # UserId 칼럼을 기본 키로 설정
        Name VARCHAR(100) NOT NULL,
        Job VARCHAR(100),
    );
    ```
+ ### Foreign Key(외래 키)
    + **하나의 테이블을 다른 테이블과 연결**해주는 역할을 한다.
    + 관계를 맺은 테이블 중 참조되는 릴레이션의 기본키 필드 혹은 UNIQUE 제약조건이 설정된 필드와 대응되어 릴레이션 간의 참조관계를 표현하는 속성을 의미한다
    + 쉽게 말해 외래 키는 두 테이블을 연결해 주는 **징검다리**라고 할 수 있다
    ```SQL
    # 외래 키 설정 예

    CREATE TABLE Persons (
        UserId INT NOT NULL,
        Name VARCHAR(100) NOT NULL,
        ClassId INT NOT NULL,
        PRIMARY KEY (UserId),
        FOREIGN KEY (ClassId) REFERENCES classes(ClassID) # ClassId 칼럼을 외래 키로 설정하고 classes 테이블에서 ClassID 필드를 참조
    );
    ```
## 연관관계 설정
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FctM422%2FbtrOlLAF46v%2FWJwxSyjkBDddzEZc8dssM0%2Fimg.png" width="500" height="195"><br>
+ ### 1:1 (1 대 1)
    + 하나의 테이블이 **단 하나의 테이블과 연관관계**를 가지는 것이다.
    + 하나의 테이블에 데이터를 다 넣지 않는 이유는 다음과 같다.
        + 너무나 많은 칼럼이 있을 때
        + 보안상 민감한 정보가 있을 때
    ```SQL
    # 1 대 1 관계 설정 예제

    CREATE TABLE User (
        UserID INT PRIMARY KEY,
        UserName VARCHER(50)
    );
    ```
    ```SQL
    CREATE TABLE UserProfile (
        UserProfileID INT PRIMARY KEY,
        UserID INT,
        ProfileDescription VARCHAR(255),
        FOREIGN KEY (UserID) REFERENCES User(UserID)
    );
    ```
+ ### 1:n (1 대 다)
    + 한 쪽 테이블의 레코드가 관계를 맺은 테이블의 **여러 레코드**와 연결되는 것이다.
    + **외래키**를 사용하고 **부모-자식** 관계로도 표현한다.
    ```SQL
    # 1 대 다 관계 설정 예제

    CREATE TABLE Department (
        DepartmentID INT PRIMARY KEY,
        DepartmentName VARCHAR(50)
    );
    ```
    ```SQL
    CREATE TABLE Employee (
        EmployeeID INT PRIMARY KEY,
        EmployeeName VARCHAR(50),
        DepartmentID INT,
        FOREIGN KEY (DepartmentID) REFERENCES Department(DepartmentID)
    );
    ```
+ ### n:n (다 대 다)
    + 양 쪽 엔티티(Entity) 모두에서 **1:n 관계**를 가지는 것이다.
    + 두 테이블의 대표키를 **칼럼으로 갖는 연결 테이블**을 생성해서 관리한다.
    ```SQL
    # 다 대 다 관계 설정 예제

    CREATE TABLE Student (
        StudentID INT PRIMARY KEY,
        StudentName VARCHAR(50)
    );
    ```
    ```SQL
    CREATE TABLE Course (
        CourseID INT PRIMARY KEY,
        CourseName VARCHAR(50)
    );
    ```
    ```SQL
    CREATE TABLE Enrollment (
        StudentID INT,
        CourseID INT,
        EnrollmentDate DATE,
        PRIMARY KEY (StudentID, CourseID),
        FOREIGN KEY (StudentID) REFERENCES Student(StudentID),
        FOREIGN KEY (CourseID) REFERENCES Course(CourseID)
    );
    ```