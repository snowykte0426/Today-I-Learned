# RDBMS(관계형 데이터베이스)

DB에는 다양한 형태가 있다. 관계형, 키-값형, 객체형 등이 그 주인공인데 이 문서에서는 **관계형 데이터베이스(RDBMS; Relational DataBase Management System)**에 관해 설명한다.

## RDBMS란?
![RDBMS](https://velog.velcdn.com/images/choijaehyeokk/post/5d76cf78-1f24-44e8-bfef-565eff096a47/rdbms.png)

+ RDBMS는 **행(Row, Record)** 과 **열(Column, Field)** 로 구성된 **표** 형식으로 데이터를 저장하며 데이터의 종속성을 **관계(Relation)** 로 표현하는 방식이다.
+ 컬럼의 구조와 데이터의 관계는 테이블 **스키마(Schema)** 로 사전 정의된다.

![Sheet](https://velog.velcdn.com/images%2Fchoijaehyeokk%2Fpost%2Fd0e4d03e-2b2f-4a15-9cfe-c0968d336b5e%2FUntitled.png)
+ 대표적인 RDBMS에는 **<span style="color:blue">My</span><span style="color:orange">SQL</span>**, **Postgre<span style="color:skyblue">SQL</span>**, **<span style="color:red">Oracle</span> Database** 등이 존재한다.

## 연관관계 설정
<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FctM422%2FbtrOlLAF46v%2FWJwxSyjkBDddzEZc8dssM0%2Fimg.png" width="500" height="195"><br>
+ ### 1:1 (1 대 1)
    + 하나의 테이블이 **단 하나의 테이블과 연관관계**를 가지는 것이다.
    + 하나의 테이블에 데이터를 다 넣지 않는 이유는 다음과 같다:
        + 너무나 많은 칼럼이 있을 때
        + 보안상 민감한 정보가 있을 때

+ ### 1:n (1 대 다)
    + 한 쪽 테이블의 레코드가 관계를 맺은 테이블의 **여러 레코드**와 연결되는 것이다.
    + **외래키**를 사용하고 **부모-자식** 관계로도 표현한다.

+ ### n:n (다 대 다)
    + 양 쪽 엔티티(Entity) 모두에서 **1:n 관계**를 가지는 것이다.
    + 두 테이블의 대표키를 **칼럼으로 갖는 연결 테이블**을 생성해서 관리한다.