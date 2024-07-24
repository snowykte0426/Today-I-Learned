# RESTful한 API 설계

- RESTful한 API는 아래 사항을 **모두 지켜야 한다**
    - **Client-Server**(클라이언트-서버 구조)
    - **Stateless**(상태없음)
    - **Cacheable**(캐시 처리 가능)
    - **Uniform Interface**(인터페이스 일관성)
    - **Layered System**(계층화)
    - **Code-on-Demand**(서버가 네트워크를 통해 클라이언트에 전달한 프로그램을 클라이언트가 실행 가능)
    
1. **클라이언트/서버 구조(Client-Server)**

    <img src="https://images.velog.io/images/dnflekf2748/post/3ae851c0-f5a1-4b24-a9f0-00dd4d1efe2d/%EC%84%9C%EB%B2%84%ED%81%B4%EB%9D%BC%EC%9D%B4%EC%96%B8.png" width="440">

    - **Client**는 유저와 관련된 처리를, **Server**는 **REST API**를 제공함으로써 **각각의 역할이 확실히 구분**되고 **일괄적인 인터페이스**로 분리되어 작동할 수 있도록 한다
        - **Server** - **API**를 제공하고 **비즈니스 로직 처리** 및 **저장**을 책임진다
        - **Client** - 사용자 인증이나 **context**(세션, 로그인 정보 등) 등을 **관리**하고 **책임**진다
    - 서로 간 **의존성**이 감소한다

2. **무상태성(Stateless)**

    <img src="https://velog.velcdn.com/images/hameo/post/67a00e62-9f8d-44c4-8203-3989a3050d07/image.png" width="450">

    - REST는 **HTTP**의 특성을 가지기 때문에 **무상태성** 역시 갖는다
    - 어떤 작업을 위해 상태정보를 **기억할 필요가 없고** 들어온 요청에 대한 처리만 하면 되기 때문에 **구현이 간단해진다**

3. **캐시 처리 기능(Cacheable)**

    <img src="https://velog.velcdn.com/images/psk84/post/6cb89404-0020-42b3-a159-cd999b1d3ffe/image.png" width="425">

    - 앞서 언급되었듯 **HTTP**를 사용하기 때문에 기본 웹에서 사용하는 **기존의 인프라**를 그대로 사용 가능하다
    - 캐시 사용을 통해 **응답시간이 빨라지고 트랜잭션(Transaction)이 미발생하기에** 서버의 성능을 **향상** 시킬 수 있다

4. **자체 표현 구조(Self-Descriptiveness)**
    ```json
    {
        "age": 15,
        "name": "John",
        "classId": 2
    }
    ```

    - **JSON**을 이용한 메시지 포맷을 이용해 **직관적으로 이해 가능**하고 API 메시지만으로 그 요청이 **무엇인지 판별** 가능하다

5. **계층화(Layered System)**

    <img src="https://svbtleusercontent.com/u3vxovtCYz6R4TaScpv78L0xspap.png" width="350">

    - **Client**와 **Server**가 분리되어 있기 때문에 프록시 서버, 암호화 계층 등의 **중간 매체**를 사용 가능해 **자유도가 높다**

6. **유니폼 인터페이스(Uniform Interface)**

    <img src="https://encrypted-tbn0.gstatic.com/images?q=tbn:ANd9GcRslKLLay1QlCxP9-qwyuJ6I0nCNSdx-Zx6-w&s" width="400">

    - **URI**로 지정한 리소스에 대한 조작을 가능케 하는 **아키텍처 스타일**을 의미한다
    - URI로 지정한 리소스에 대한 조작을 **통일되고 한정적인** 인터페이스로 수행한다
    - 특정 언어나 기술에 **종속되지 않는** 기술이다

## 중심 규칙
1. URI는 **정보의 자원**을 표현하여야 한다
2. **자원에 대한 행위**는 **HTTP Method**로 표현한다

## 세부 규칙
1. 슬래시 구분자(<b> / </b>)는 **계층 관계**를 나타내는데 사용된다
2. URI 마지막 문자로 슬래시 구분자를 **사용하지 않는다**
3. 하이픈(<b> - </b>)은 **가독성**을 높이는데 사용한다
4. 밑줄(<b> _ </b>)은 URI에 **사용하지 않는다**
5. URI에서 대문자 사용은 **지양**한다
6. **파일 확장자**는 URI에 포함하지 않도록 한다
    - 대신에 **Accept Header**를 사용한다
    - **예시**: <code>GET</code> : <code>http://restapi.exam.com/orders/2</code>  
      <code>Accept: image/jpg</code>
7. 리소스 간 **연관 관계**가 있는 경우 <b><code>GET: /resource1/2/resource2</code></b>와 같이 나타낼 수 있으며 이는 <b><code>/리소스명/리소스ID/관계가 있는 다른 리소스명</code></b>의 형태로 나타내는 것이다
