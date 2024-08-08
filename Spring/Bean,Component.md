# Bean,Component
Java와 Spring 프레임워크에서 중요하게 다뤄지는 **Bean(빈)** 과 Spring의 **Component(컴포넌트)** 에 관해 본 문서(TIL)에서 살펴본다.
## 빈(Bean)
### **Java의 경우**
+ **특정 형태의 클래스**들을 가르키는 말이다.
+ **[DTO 또는 VO](https://github.com/snowykte0426/Today-I-Learned/blob/main/Spring/DAO%2CDTO%2CVO%2CBuilder%20Pattern.md)** 의 형태가 Java Bean이라고 생각해도 된다.
+ 각 필드는 **``private``** 으로 구성되어 **getter**,**setter**로만 접근할 수 있고 인자가 없는 기본 생성자를 가진다.
+ **Serializable 인터페이스**를 구현하는 것을 Java에선 권장한다
    <h4><span class="rotated_star">&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp&nbsp★</span> <b>Serializable 인터페이스란?</b></h4>
    <blockquote>
        <ul>
            <li>객체가 <b>객체 직렬화(객체가 이진 데이터로 변환되는 것)</b> 하는 것이 가능하다는 것을 나타낸다.</li>
            <li>이를 이용하면 데이터를 <b>저장</b>하거나 <b>전송</b>할 때 유용하다.</li>
        </ul>
    </blockquote>

```java
public class User implements Serializable {
    private String name;
    private int age;

    public User() {  // 기본 생성자        
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
            this.age = age;
    }
}
```
### **Spring의 경우**
+ **Spring IoC 컨테이너**가 관리하는 **Java 객체**를 뜻한다.
+ 일반적인 Java 객체와 다른 점은 없으며 단순히 Spring IoC 컨테이너에서 관리되는 객체를 **Bean**이라고 부르는 것이다.
+ Spring Bean은 **등록** 해주어야 하는데 **XML에 직접 등록**하거나 **``@Bean``** 어노테이션을 사용하거나 **``@Component``**,**``@Controller``**,**``@Service``**,**``@Repository``** 어노테이션을 사용하는 방법이 있다.
+ **XML 등록방법**
    ```xml
    <!-- XML 네임스페이스 및 스키마 정의 -->
    <beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
                           http://www.springframework.org/schema/beans/spring-beans.xsd">

        <!-- Bean 정의 -->
        <bean id="userRepository" class="com.example.UserRepository"/>
        <!-- Bean 정의 -->
        <bean id="userService" class="com.example.UserService">
            <!-- 의존성 주입 -->
            <property name="userRepository" ref="userRepository"/>
        </bean>

    </beans>
    ```
+ **``@Bean``** 사용
    ```java
    @Configuration
    public class AppConfig {
        @Bean  // @Bean 사용
        public UserService userService() {
            return new UserService();
        }
    }
    ```
+ **``@Component``**,**``@Controller``**,**``@Service``**,**``@Repository``** 사용
    ```java
    @Service
    public class UserService {
        private final UserRepository userRepository;

        @Autowired
        public UserService(UserRepository userRepository) {
            this.userRepository = userRepository;
        }
        // 이하 생략
    }
    ```
## 컴포넌트(Component)
+ 영단어 상으로는 **구성요소**라는 뜻으로 **독립적인 단위 모듈**을 의미한다.
+ 개발자가 직접 작성한 클래스를 **Bean**으로 만드는 것이다.
+ **``@Component``** 어노테이션을 통해 **싱글톤** 클래스 빈을 생성할 수 있다(**[@Scope](https://github.com/snowykte0426/Today-I-Learned/blob/main/Spring/Annotation.md#spring-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98)** 어노테이션을 통해 싱글톤이 아닌 방식으로도 생성이 가능).
+ 한마디로 패키지 스캔을 할 때 이 어노테이션을 **"이 클래스를 정의했으니 빈으로 등록하라"** 라는 뜻으로 쓰인다.
### ``@Component``와 관련 어노테이션
+ **``@Component``**: 일반적인 스프링 관리 빈을 정의할 때 사용한다.
+ **``@Controller``**`: MVC 패턴에서 **컨트롤러 역할**을 하는 빈을 정의할 때 사용한다.
+ **``@Service``**: **서비스 레이어**를 나타내는 빈을 정의할 때 사용한다.
+ **``@Repository``**: **데이터 접근 레이어**를 나타내는 빈을 정의할 때 사용한다.