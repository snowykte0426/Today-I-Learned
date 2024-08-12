# Bean LifeCycle,Bean Scope
Bean(빈)의 **Life Cycle(생명주기)** 와 **Scope(범위)** 에 대해 본 문서(TIL)에서 알아본다.
## Bean 생명주기(LifeCycle)
+ Bean이 **생성**되고 **소멸**되는 주기(Cycle)을 의미한다.
    ```
    스프링 컨테이너 생성
      -> 스프링 빈 생성
        -> 의존관계 주입
          -> 초기화 콜백
            -> 사용
              -> 소멸 전 콜백
                -> 스프링 종료
    ```
![Bean_LifeCycle](https://velog.velcdn.com/images/destiny1616/post/b5c3a8f6-1301-4ca2-9b48-d9e09ac4f07d/image.jpeg)
+ **스프링 컨테이너 생성**
  + **ApplicationContext**라고도 하며 스프링 프레임워크가 핵심인 스프링 컨테이너가 생성되는 과정이다.
  + 컨테이너는 Bean을 **관리**하고 그들의 **생명주기**를 **제어**한다.
+ **스프링 빈 생성**
  + 컨테이너가 초기화 된 이후 설정 파일(XML,Java 설정 클래스 등)에 **정의**된 Bean이 **생성**된다.
+ **의존관계 주입**
  + 생성된 Bean에 대한 **의존성**을 **주입**해준다.
+ **초기화 콜백(CallBack)**
  + Bean이 초기화 작업을 수행할 수 있도록 초기화 콜백 메서드가 **호출**된다.
+ **소멸 전 콜백(CallBack)**
  + 스프링 컨테이너가 **종료**되거나 Bean이 **소멸**되기 전에 Bean의 ***정리 작업**을 위해서 콜백 메서드가 호출된다.
  + **리소스 해제**,**연결 종료** 등의 마무리 작업을 수행할 수 있다.
+ Bean의 생명주기 중에서 **``@PostConstruct``** 와 같은 어노테이션 등을 이용해 Bean의 **초기화**,**생성** 작업을 세밀히 **조정**할 수 있다.
+ **초기화 시점**
  + **``@PostConstruct``**
  + **InitializingBean** 인터페이스의 **``afterPropertiesSet()``** 메서드
  + **Bean 설정 파일**의 **``initMethod``** 속성
+ **소멸 시점**
  + **``@PreDestroy``**
  + **DisposableBean** 인터페이스의 **``destroy()``** 메서드
  + **Bean 설정 파일**의 **``destroyMethod``** 속성
## Bean 범위(Scope)
+ Spring Bean은 기본적으로 **싱글톤**으로 생성되기 떄문에 Scope란 Bean이 존재할 수 있는 **범위**를 뜻한다.
+ Spring은 다양한 Bean의 범위를 제공한다
  + **싱글톤(Singleton)**: **기본 스코프**로 Spring 컨테이너가 시작할 때 부터 종료될 때까지 유지되는 **가장 큰 범위**의 스코프이다.
  + **프로토타입(Prototype)**: Spring 컨테이너는 Bean의 **생성**과 **의존 관계 주입**까지만 관여하고 더 이상 **관리 하지 않는 매우 짧은 범위**의 스코프이다.
  + **리퀘스트(Request)**: 웹 요청이 **들어오고 나갈 때까지** 유지되도록 하는 스코프이다.
  + **세션(Session)**: 웹 세션이 **생성되고 종료될 때까지 유지되는 스코프이다.
  + **애플리케이션(Application)**: 웹의 **서블릿 컨텍스트(Servlet Context)** 와 같은 범위로 유지되는 스코프이다.
+ Bean의 스코프는 다음과 같이 지정할 수 있다.
  ```java
  @Scope("prototype")  //Prototype 스코프 지정
  @Component
  public class NetworkClient {
  }

  @Configuration
  class LifeCycleConfig {
      @Scope("prototype")  //Prototype 스코프 지정
      @Bean
      public NetworkClient networkClient() {
          final NetworkClient networkClient = new NetworkClient();
          return networkClient;
      }
  }
  ```
+ 프로토타입 스코프의 경우 생성 과정과 의존성 주입까지는 콜백 메서드가 작동하시만 이후 **종료 메서드**는 작동하지 않는다.
+ 싱글톤 스코프를 가진 Bean이 프로토타입 스코프를 가진 Bean을 주입받는 다면 프로토타입 Bean은 싱글톤 Bean에게 의존성이 주입되는 시점에만 프로토타입 Bean이 **조회**될 것이고 그렇게 된다면 이후 프로토타입 Bean은 같은 Bean이 계속 사용될 수 있기 때문에 **주의**하여야 한다.