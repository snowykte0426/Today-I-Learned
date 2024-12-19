# Dispatcher-Servlet
Spring 구조에서 가장 핵심적인 컴포넌트 중 하나인 **Dispatcher-Servlet**에 대하여 알아본다
## 정의
**HTTP 프로토콜**로 들어오는 모든 요청을 가장 먼저 받아 적합한 컨트롤러에 위임하는 역할을 수행하는 <b>프론트 컨트롤러(Front Controller)</b>를 뜻한다.
## 설명
클라이언트에서 어떠한 요청이 오면 Tomcat과 같은 **서블릿 컨테이너(Servlet Container)**가 요청을 받게 된다.그리고 이 모든 요청을 프론트 컨트롤러인 Dispatcher-Servlet가 받으며 공통된 작업을 먼저 처리한 후에 요청을 처리해야 하는 컨트롤러를 찾아 **위임**하게 된다.
## 장점
Spring MVC에서 Dispatcher-Servlet이 등장하며 **web.xml**의 역할을 상당히 축소시켰다.과거에는 모든 서블릿을 URL 매핑을 위하여 web.xml에 등록해주어야 했지만 이제는 Dispatcher-Servlet이 자동으로 모든 요청을 **핸들링**해주고 **공통 작업**을 처리해주며 편리하게 개발할 수 있게 되었다
## 정적 자원(Static Resource) 처리
HTML,CSS,JavaScript 등의 정적 자원에 대한 요청까지 Dispatcher-Servlet이 가로채어 요청에 실패하는 문제가 발생하였는데 이를 해결하기 위한 2가지 방법이 존재한다.
### 애플리케이션(Application) 요청 분리
+ 클라이언트의 요청을 2가지로 분리하는 방법으로 예를 들어 ``/apps`` 경로로 요청하면 Dispatcher-Servlet이 담당하고 ``/resources`` 경로로 요청하면 담당하지 않는 방식이다.
+ 이러한 방식은 코드가 더러워지고 **모든 요청에 대하여 URL을 붙여줘야 함으로** 직관적인 설계가 될 수 없다**.
### 컨트롤러(Controller) 탐색 후 정적 자원 처리
+ Dispatcher-Servlet이 우선적으로 요청을 처리할 컨트롤러를 **탐색**한 후 없다면 **2차적으로 설정된 자원 경로를 탐색**하는 방식이다.
+ 영역을 완전히 분리하여 효율적으로 자원 관리를 함 뿐 아니라 추후 확장을 용이하게 할 수 있다.
## 동작 과정
![동작과정 이미지](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbcff5H%2FbtstbdRuSr9%2FpNKnGdMwftSWmiGLHA7yL0%2Fimg.png)
### 1.클라이언트의 요청을 Dispatcher-Servlet이 수령
<b>서블릿 컨텍스트(Servlet Context)</b>에서 <b>필터(Filter)</b>를 지나 <b>스프링 컨텍스트(Spring Context)</b>에서는 Dispatcher-Servlet이 가장 먼저 요청을 수령하게 된다.<b>[인터셉터(Interceptor)](https://amond-codingredord.tistory.com/12)</b>가 컨트롤러에 실제로 요청을 위임하지는 않기에 아래 이미지는 참고로만 보는 것이 좋다).
![처리과정 도식](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FoN96r%2Fbtrw7SYEpgr%2FlKLp5nqEZUJR32GoPc9bwk%2Fimg.png)
### 2.요청 정보를 통하여 요청을 위임할 컨트롤러 탐색
어느 컨트롤러가 받은 요청을 처리할 수 있는지 **확인**해야 하는데 이 역할을 하는 서블릿이 **HandlerMapping**이다.최근에는 보통 ``@Controller``등의 <b>[어노테이션(Annotation)](https://github.com/snowykte0426/Today-I-Learned/blob/main/Spring/Annotation.md)</b>을 쓰는 경우가 많지만 <b>``Controller`` 인터페이스(Interface)</b>를 이용하여 구현하는 등 여러 방법이 있기에 다양한 구현 방법에 따라 요청을 처리할 대상을 찾도록 한 것이 HandlerMapping이다.어노테이션을 통한 구현을 사용했을 때 <b>``RequestMappingHandlerMapping``</b>가 처리하며 ``@Controller``로 작성된 모든 컨트롤러를 찾고 파싱하여 **``HashMap<[요청정보], [처리대상]>``** 형식으로 관리한다.여기서 처리대상은 **``HandlerMethod`` 객체**로 컨트롤러,메서드(Method) 등을 가지고 있는데 이는 Spring이 <b>리플렉션(Reflection)</b>을 이용해 요청을 위임하기 때문이다.요청이 오면 **HTTP Method**,**URI** 등을 사용해 **요청 정보**를 만들고 HashMap에서 요청을 처리할 대상을 탐색한 후에 <b>``HandlerExecutionChain``</b>으로 감싸서 반환한다.이는 컨트롤러로 위임해주기 전에 처리해야 하는 인터셉터를 포함하기 위해서이다.
### 3.요청을 컨트롤러로 위임해줄 HandlerAdapter를 탐색 후 전달
Dispatcher-Servlet이 직접 컨트롤러로 요청을 위임하는것이 아닌 **HandlerAdapter**를 통하여 위임한다.이는 컨트롤러의 구현 방식이 굉장히 다양하기 때문으로 Spring은 다양한 컨트롤러 구현방식에 대응하기 위하여 <b>어댑터 패턴(Adapter Pattern)</b>을 적용하여 컨트롤러의 유형에 상관없이 요청을 위임할 수 있게 되었다.
### 4.HandlerAdapter가 요청을 컨트롤러로 위임
HandlerAdapter가 요청을 컨트롤러로 위임한 **전/후**로 공통 작업의 처리가 필요한데 인터셉터들을 포함해 요청 시에 <b>``@RequestBody``,``@RequestParam``</b> 등을 처리하기 위해 <b>``ArgumentResolver``</b>들과 응답시 ``ReponseEntity``의 Body를 JSON으로 직렬화 하는 등의 처리를 수행하는 <b>``ReturnValueHandler``</b> 등이 HandlerAdapter에서 처리된다.``ArgumentResolver``등을 통하여 파라미터가 준비가 된다면 컨트롤러로 요청을 위임한다.
### 5.비즈니스 로직 처리
컨트롤러는 서비스를 호출하고 비즈니스 로직을 통과하여 값을 처리한다.
### 6.컨트롤러의 반환값 반환
비즈니스 로직의 처리 완료 후 컨트롤러는 반환값을 반환한다.
### 7.HandlerAdapter의 반환값 처리
컨트롤러의 응답을 응답 처리기인 ``ReturnValueHandler``가 후처리를 한 후 Dispatcher-Servlet으로 **돌려준다**.컨트롤러가 ``ResponseEntity``를 반환한다면 <b>``HttpEntityMethodProcess``</b>가 <b>``MessageConverter``</b>를 이용하여 응답 객체를 **직렬화**하고 응답 상태(HTTP Status)를 설정한다.컨트롤러가 View 이름을 반환했다면 <b>``ViewResolver``</b>를 이용하여 View를 반환한다.
### 8.서버의 응답을 클라이언트로 반환
Dispatcher-Servlet를 통해 반환된 응답은 다시 필터들을 거쳐 클라이언트로 반환된다.응답이 데이터라면 **그대로 반환되지만** 응답이 화면이라면 View의 이름에 알맞은 View를 찾아 ``ViewResolver``가 적절한 화면을 반환한다.