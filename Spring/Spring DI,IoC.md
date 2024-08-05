# **Spring DI,Ioc**
Spring 프레임워크의 차별점 중 하나인 **DI(Dependency Injection; 의존성 주입)** 와 **IoC(Inversion of Control; 제어 역전)** 에 대해 본 문서(TIL)에서 정리한다.
## 의존성 주입(Dependency Injection)
+ 객체를 **직접 생성하는게 아닌 외부에서 생성 후 주입**하는 방식을 뜻한다.
+ DI를 통해 모듈 간 **결합도 하락**과 **유연성 증가**를 노릴 수 있다.
+ 의존성 주입 방법에는 **4가지**가 있다.
    + **생성자 주입(Constructor Injection)**
        ```java
        public class A {
            private B b;
            
            public A(B b) {
                this.b = b;
            }
        }
        ```
        + Spring 에서 권장하는 방식이다.
        + 필수적으로 사용해야하는 의존성 없이 **인스턴스를 생성하지 못하도록 강제**할 수 있다.
        + **``final``**을 사용하여 객체의 불변성을 강화할 수 있으며 컴파일 단계에서 **순환 의존성**을 알아챌 수 있다.
        + 인자가 많아지면 코드가 길어지며 개발자로 하여금 [SPR 원칙](https://github.com/snowykte0426/Today-I-Learned/blob/main/Object-Oriented%20Programming/SOLID.md)을 지키는데 도움을 줄 수 있다.
    + **Setter 주입(Setter Injection)**
        ```java
        public class A {
            private B b;
            
            public void setB(B b) {
                this.b = b;
            }
        }
        ```
        + **선택적인 의존성**을 사용할 때 유용하며 **상황에 따라 의존성 주입**을 할 수 있다.
        + **의존성이 주입되지 않아도** 객체가 생성될 수 있으며 이에 따라 **``NullPointerException``** 이 발생할 수 있다.
    + **필드 주입(Field Injection)**
        ```java
        @Component
        public class SampleController {
            @Autowired
            private SampleService sampleService;
        }
        ```
        + 어노테이션을 활용하여 **직접** 의존성을 주입하는 방식이다.
        + 의존성을 주입하기 쉽기 때문에 위기감을 느끼기 어려워 [SPR 원칙](https://github.com/snowykte0426/Today-I-Learned/blob/main/Object-Oriented%20Programming/SOLID.md)을 지키기 어렵게 만든다.
        + 불변성이 없기 때문에 객체가 **변할 수도 있다**.
        + 의존성이 겉으로 들어나지 않기 때문에 **숨은 의존성**만 늘어난다.
        + 해당 방법은 **읽기 쉽고,사용하기 편하다는 장점 외에는 단점 뿐이기에 사용을 지양**한다.
    + **인터페이스 주입(Interface Injection)**
        ```java
        public interface BInjection {
            void inject(B b);
        }

        public A implements BInjection {
            private B b;
            
            @Override
            public void inject(B b) {
                this.b = b;
            }
        }
        ```
        + 의존성을 주입받기 위한 메서드를 **인터페이스**로 정의하고 이를 구현한 클래스에서 의존성을 주입받는 방식이다.
        + 의존성 주입에 대한 **명확한 계약 관계**를 정의가능하다.
        + 코드가 지나치게 복잡해질 수 있다.
+ 앞서 언급되었듯 2022년에 발표된 최신 Spring 프레임워크에선 **생성자 주입** 방법을 권장한다.
## 제어의 역전(Inversion of Control)
+ 프레임워크가 없는 전통적인 프로그래밍에서는 **객체의 생명 주기**를 프로그래머가 전부 관리하지만 프레임워크를 사용하면 이를 전부 위임할 수 있는데 이처럼 메서드나 객체의 제어를 외부에 **위임**하는 것을 **제어의 역전(IoC)** 라 한다.
+ IoC에선 객체 스스로가 사용할 객체를 결정하지도, 생성하지도 않는데 이름 그대로 **제어에 대한 권한이 외부 환경으로 역전**되는 것이다.
+ 어플리케이션의 제어 책임이 외부에 있으므로 프로그래머는 **핵심 비즈니스 로직 개발에 더 집중**할 수 있다는 장점이 있다.
+ Junit 프레임워크를 이용해 테스트를 할 때를 예로 들자면 각각의 **테스트 메서드들은 개발자가 작성하지만 해당 메서드의 제어권은 프레임워크에 있게 된다**.
    + 이때 **``@Test``** 어노테이션을 붙이기만 하면 Junit이 해당 **메서드를 자동으로 호출한다**,이런 작용이 바로 **IoC의 예시**이다.
+ 또 다른 예시로 **템플릿 메서드 패턴(Template Method Pattern)** 을 들수가 있다.
    + 추상 클래스에 흐름을 제어하는 메서드를 정의해두고 변경이 필요 없는 하위 메서드들은 **공통 메서드**로,변경이 필요하면 **추상 메서드**로 정의해 놓는다.
    + 해당 클래스를 상속받는 클래스에서 추상 메서드를 구현하여 사용하게 되는데 이렇게 된다면 **하위 클래스에서 메서드의 실 구현은 하긴 하지만 해당 메서드의 호출 제어권은 상위 추상 클래스**에 있게 된다.
+ IoC를 도입하면 다음과 같은 이점이 있을 수 있다.
    + 앞서 언급하였듯 개발자는 **비즈니스 로직 개발에 집중할 수 있다**.
    + **프로그램의 진행 흐름과 구체적인 구현을 분리할 수 있다**.
    + **구현체 사이의 변경이 용이**해진다.
    + **객체간 의존성 감소**에 효과적이다.
+ **DI는 IoC를 실현하기 위한 여러 디자인 패턴** 중 하나로 **IoC == DI**로 혼동 해서는 안된다.
## 참고자료
+ [[Spring DI/IoC] IoC? DI? 그게 뭔데?](https://velog.io/@ohzzi/Spring-DIIoC-IoC-DI-%EA%B7%B8%EA%B2%8C-%EB%AD%94%EB%8D%B0)<br>
+ [[Spring] DI, IoC 정리](https://velog.io/@gillog/Spring-DIDependency-Injection)
+ [[Spring] DI(Dependency Injection) 세 가지 방법](https://velog.io/@gillog/Spring-DIDependency-Injection-%EC%84%B8-%EA%B0%80%EC%A7%80-%EB%B0%A9%EB%B2%95)<br>
+ [제어의 역전 (Inversion Of Control, IoC)](https://hudi.blog/inversion-of-control/)