# RESTful한 API 설계
- RESTful한 API는 아래 사항을 **모두 지켜야 한다**
    - **Client-Server**(클라이언트-서버 구조)
    - **Stateless**(상태없음)
    - **Cache**(캐시 처리 가능)
    - **Uniform Interface**(인터페이스 일관성)
    - **Layered System**(계층화)
    - **Code-on-Demand**(서버가 네트워크를 통해 클라이언트에 전달한 프로그램을 클라이언트가 실행 가능)
    <ol>
        <li><b>클라이언트/서버 구조(Client-Server)</b></li>
            <ul>
                <li><b>Client</b>는 유저와 관련된 처리를,<b>Server</b>는 <b>REST API</b>를 제공함 으로써 <b>각각의 역할이 확실히 구분</b>되고 <b>일괄적인 인터페이스</b>로 분리되어 작동할 수 있도록 한다</li>
                    <ul>
                        <li><b>Server</b> - <b>API</b>를 제공하고 <b>비즈니스 로직 처리</b> 및 <b>저장</b>를 책임진다</li>
                        <li><b>Client</b> - 사용자 인증이나 <b>context</b>(세션,로그인 정보 등) 등을 <b>관리</b>하고 <b>책임</b>진다</li>
                    </ul>
                <li>서로 간 <b>의존성</b>이 감소한다</li>
            </ul>
        <li><b>무상태성(Stateless)</b></li>
            <ul>
                <li>REST는 <b>HTTP</b>의 특성을 가지기 때문에 <b>무상태성</b> 역시 갖는다</li>
                <li>어떤 작업을 위해 상태정보를 <b>기억할 필요가 없고</b> 들어온 요청에 대한 처리만 하면 되기 때문에 <b>구현이 간단해진다</b></li>
            </ul>
        <li><b>캐시 처리 기능(Cacheable)</b></li>
            <ul>
                <li>앞서 언급되었듯 <b>HTTP</b>를 사용하기 때문에 기본 웹에서 사용하는 <b>기존의 인프라</b>를 그대로 사용 가능하다</li>
                <li>캐시 사용을 통해 <b>응답시간이 빨라지고 트랜잭션(Transcation)이 미발생하기에</b> 서버의 성능을 <b>향상</b> 시킬수 있다
            </ul>
        <li><b>자체 표현 구조(Self-Descriptiveness)</b></li>
            <ul>
                <li><b>JSON</b>을 이용한 메세지 포맷을 이용해 <b>직관적으로 이해가능</b>하고 API메세지 만으로 그 요청이 <b>무엇인지 판별</b> 가능하다</li>
            </ul>
        <li><b>계층화(Layered System)</b></li>
            <ul>
                <li><b>Client</b>와 <b>Server</b>가 분리되어 있기 때문에 프록시 서버,암호화 계층 등의 <b>중간매체</b>를 사용 가능해 <b>자유도가 높다</b></li>
            </ul>
        <li><b>유니폼 인터페이스(Uniform Interface)</b></li>
            <ul>
                <li><b>URI</b>로 지정한 리소스에 대한 조작을 가능케 하는 <b>아키텍쳐 스타일</b>을 의미한다</li>
                <li>URI로 지정한 리소스에 대한 조작을 <b>통일되고 한정적인</b> 인터페이스로 수행한다</li>
                <li>특정 언어나 기술에 <b>종속되지 않는</b> 기술이다</li>
            </ul>
    </ol>
+ **중심 규칙**
    <ol>
        <li>URI는 <b>정보의 자원</b>을 표현하여야 한다</b></li>
        <li><b>자원에 대한 행위</b>는 <b>HTTP Method</b>로 표현한다
    </ol>
+ **세부 규칙**
    <ol>
        <li>슬래시 구분자(<b> / </b>)는 <b>계층 관계</b>를 나타내는데 사용된다</li>
        <li>URI 마지막 문자로 슬래쉬 구분자를 <b>사용하지 않는다</b></li>
        <li>하이픈(<b> - </b>)은 <b>가독성</b>을 높이는데 사용한다</li>
        <li>밑줄(<b> _ </b>)은 URI에 <b>사용하지 않는다</b></li>
        <li>URI에서 대문자 사용은 <b>지양</b>한다</li>
        <li><b>파일확장자</b>는 URI에 포함하지 않도록 한다</li>
        <blockquote>
           <ul> 
                <li>대신에 <b>Accept Header</b>를 사용한다</li>
                <li><b>ex)</b> <code>GET</code> : <code>http://restapi.exam.com/orders/2/Accept: image/jpg</code></li>
            </ul>
        </blockquote>
        <li>리소스 간 <b>연관관계</b>가 있는 경우 <b><code>GET: /resource1/2/resource2</code></b>와 같이 나타낼 수 있으며 이는 <b><code>/리소스명/리소스ID/관계가 있는 다른 리소스명</code></b>의 형태로 나타내는 것이다</li>
    </ol>