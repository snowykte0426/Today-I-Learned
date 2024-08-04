# **Build Tool**,**Directory Structure**
이 문서(TIL)에서는 **빌드 도구**, **Spring 디렉터리 구조**을 알아본다.
## 빌드 도구(Build Tool)
+ 소스 코드에서 애플리케이션 생성을 **자동화** 하는 도구(프로그램)을 **빌드 도구**라 칭한다.
+ 기존에 프로그래머가 직접 하던 빌드 작업을 프로그램이 대신 자동화 해준다고 생각하면 된다.
+ Java 뿐 아니라 C,C# 등 타 언어에도 빌드 도구가 존재한다.
+ **Make**,**CMake**,**Ninja**,**MSBuild**,**Ant**,**Maven**,**Gradle** 등 수많은 빌드 도구가 사용되고 개발 중이다.
    ### **Ant**

    <img src="https://upload.wikimedia.org/wikipedia/commons/2/2f/Apache-Ant-logo.svg" width=300/>

    + make와 비슷하나 Java로 만들어져 **Java 실행환경**이 필요하며 빌드를 위한 환경구성을 **XML**을 사용한다.
    + 이클립스(eclipse) IDE에 탑재되던 빌드 도구로 현재는 많이 사용되지 않는다.
    ### **Maven**

    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/5/52/Apache_Maven_logo.svg/180px-Apache_Maven_logo.svg.png" width=250/>

    + Java 프로젝트들을 위한 **빌드 자동화 도구**로 Java 뿐 아니라 **C#**,**Ruby**,**Scala** 등의 언어로 개발된 프로젝트 역시 빌드하고 관리할 수 있다.
    + 프로젝트들은 **오브젝트 모델(Project Object Model; POM)**을 사용하여 구성되며 **pom.xml** 파일에 저장된다.
    + **생명주기(Life Cycle)** 개념이 도입되어 빌드 순서등을 정의할 수 있다.
    ### **Gradle**

    <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/6/6b/Gradle_logo.svg/180px-Gradle_logo.svg.png" width=240/>

    + **Java**,**Kotlin**,**Groovy**로 개발된 빌드 도구로 **안드로이드 스튜디오(Android Studio)**의 공식 빌드 시스템이자 **Java**,**C/C++**,**Python**등 다양한 언어를 지원한다.
    + **Build.gradle**에 스크립트를 작성하며 프로젝트의 규모가 커질수록 복잡해지는 XML 기반 스크립트에 비해 **간편하다는 장점**이 있다.

    ### **Maven vs Gradle**
    | **항목**                  | **Gradle**                                                                 | **Maven**                                                         |
    |---------------------------|-----------------------------------------------------------------------------|-------------------------------------------------------------------|
    | **모델**                  | 작업 의존성 그래프 기반                                                     | 고정적이고 선형적인 단계 기반                                     |
    | **다중 모듈 빌드**        | 병렬 실행 가능, 태스크 업데이트 여부를 체크하여 증분 빌드 허용             | 병렬 실행 가능, 고정적인 빌드 단계                               |
    | **설정 공유**             | 설정 주입 방식 제공                                                        | 상속을 통해 설정 공유                                            |
    | **캐시**                  | Concurrent에 안전한 캐시, checksum 기반의 캐시 사용, repository와 동기화 가능 | 기본적인 캐시 지원                                               |
    | **커스터마이징**          | 간편한 커스터마이징                                                         | 제한된 커스터마이징                                               |
    | **증분 빌드**             | 이미 업데이트된 태스크는 실행하지 않으므로 빌드 시간 단축                   | 증분 빌드 미지원                                                 |
    | **유연성**                | 매우 유연하며 다양한 플러그인 및 확장 가능                                   | 상대적으로 덜 유연하고 제한적인 플러그인 지원                    |
    | **성능**                  | 빠른 빌드 시간, 특히 큰 프로젝트에서 유리                                    | 상대적으로 느림                                                  |

## 디렉터리 구조(Directory Structure)
+ 먼저 Spring 패키지 구조에 관해 정리하기 전 용어정리를 하고자 한다.

### Controller

```java
@RestController
@RequestMapping("/api/users")
public class UserController {

    @Autowired
    private UserService userService;

    @GetMapping("/{id}")
    public ResponseEntity<UserDTO> getUserById(@PathVariable Long id) {
        UserDTO user = userService.getUserById(id);
        return ResponseEntity.ok(user);
    }
}
```
+ **사용자 요청(Request)** 을 처리하고 **응답(Response)** 을 생성한다.
+ 적절한 서비스 계층의 메서드를 **호출**하고 결과를 **클라이언트에 반환**한다.
### Service

```java
@Service
public class UserService {

    @Autowired
    private UserRepository userRepository;

    public UserDTO getUserById(Long id) {
        User user = userRepository.findById(id).orElseThrow(() -> new UserNotFoundException(id));
        return convertToDTO(user);
    }

    private UserDTO convertToDTO(User user) {
        UserDTO userDTO = new UserDTO();
        userDTO.setId(user.getId());
        userDTO.setName(user.getName());
        userDTO.setEmail(user.getEmail());
        return userDTO;
    }
}
```
+ **비즈니스 로직**을 처리하는 계층으로 컨트롤러와 DAO 사이에서 동작하며 비즈니스 로직을 **캡슐화**하고 **트랜잭션(Transaction)** 을 관리한다.
### DTO
```java
public class UserDTO {
    private Long id;
    private String name;
    private String email;
}
```
+ **Data Transfer Object**의 약자로 계층 간 데이터를 전달하는데 사용된다.
### DAO
```java
@Repository
public interface UserRepository extends JpaRepository<User, Long> { }
```
+ DB와 상호작용하여 **저장**,**수정**,**삭제**,**조회**하는 역할을 수행한다.
### Entity
```java
@Entity
public class User {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @Column(nullable = false)
    private String name;

    @Column(nullable = false, unique = true)
    private String email;

    public User() {
    }

    public User(String name, String email) {
        this.name = name;
        this.email = email;
    }

    public Long getId() {
        return id;
    }

    public void setId(Long id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getEmail() {
        return email;
    }

    public void setEmail(String email) {
        this.email = email;
    }
}
```
+ DB 테이블에 **대응**하는 하나의 클래스로 실제 **DB의 테이블**과 매칭된다.
### **계층형 구조(Layered Architecture)**
```
com.example.myapp/
│
├── controller/
│   ├── UserController.java
│   └── ProductController.java
│
├── service/
│   ├── UserService.java
│   └── ProductService.java
│
├── repository/
│   ├── UserRepository.java
│   └── ProductRepository.java
│
├── model/
│   ├── User.java
│   └── Product.java
│
└── dto/
    ├── UserDTO.java
    └── ProductDTO.java
```
+ 애플리케이션을 **기능적인 계층**으로 나누어 각 계층이 특정 역할을 담당하도록 조직한다.
+ 각 계층이 명확하게 분리되어 있어 코드의 모듈성을 높이고 이해하기 쉽게 만든다.
### **도메인형 구조(Domain-Driven Design)**
```
com.example.myapp/
│
├── user/
│   ├── controller/
│   │   └── UserController.java
│   ├── service/
│   │   └── UserService.java
│   ├── repository/
│   │   └── UserRepository.java
│   ├── model/
│   │   └── User.java
│   └── dto/
│       └── UserDTO.java
│
├── product/
│   ├── controller/
│   │   └── ProductController.java
│   ├── service/
│   │   └── ProductService.java
│   ├── repository/
│   │   └── ProductRepository.java
│   ├── model/
│   │   └── Product.java
│   └── dto/
│       └── ProductDTO.java
```
+ 애플리케이션을 기능적인 계층이 아닌 **도메인(업무 영역)** 에 따라 조직하는 구조이다.
+ 복잡한 프로젝트에서 유용하며 각 도메인 패키지는 **모든 클래스(DTO,Controller,Service,Repository 등)** 를 포함한다.
### **계층형 구조** vs **도메인형 구조**
| **특징**                | **도메인형 구조**                                                                         | **계층형 구조**                                                                         |
|------------------------|-----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------|
| **조직 기준**           | 도메인(업무 영역)별로 패키지 구성                                                        | 기능적 계층별로 패키지 구성                                                            |
| **구조**               | 도메인 패키지 내에 컨트롤러, 서비스, 리포지토리, 모델, DTO 등이 포함됨                     | 컨트롤러, 서비스, 리포지토리, 모델, DTO 등이 각각 별도의 계층 패키지에 위치              |
| **코드 모듈화**         | 도메인별로 모듈화되어 유지보수와 확장에 용이                                               | 기능별로 모듈화되어 공통 기능을 재사용하기 용이                                         |
| **변경 영향**           | 도메인 내부에서의 변경이 주로 일어나므로 영향 범위가 적음                                 | 기능 계층 간의 의존성이 복잡해질 수 있으며, 계층 간의 변경 시 영향 범위가 넓을 수 있음   |
| **중복 코드**           | 공통 기능이 여러 도메인에 중복될 가능성이 있음                                            | 공통 기능을 계층별로 관리할 수 있어 중복이 적음                                         |
| **이해도**              | 도메인에 관련된 모든 것이 한 곳에 모여 있어 도메인별로 이해하기 쉬움                       | 계층별로 기능이 분리되어 있어 전체적인 구조를 이해하기 쉬움                             |
| **확장성**              | 도메인별로 쉽게 확장 가능, 큰 규모의 복잡한 애플리케이션에 적합                            | 새로운 계층을 추가하거나 기존 계층을 확장하기 용이                                       |
| **적합한 규모**         | 큰 규모의 복잡한 애플리케이션에 적합                                                      | 작은 규모에서 중간 규모의 애플리케이션에 적합                                           |
| **유지보수성**          | 도메인별로 변경이 일어나므로 특정 도메인에 국한된 유지보수가 용이                          | 계층별로 공통 기능을 수정할 수 있어 유지보수성 좋음                                     |
| **패키지 예시**         | com.example.myapp.user<br>com.example.myapp.product<br>각 도메인 내 controller, service 등 | com.example.myapp.controller<br>com.example.myapp.service<br>com.example.myapp.repository 등 |

클래스 구조가 적은 소규모 프로젝트에서는 어떠한 패키지 구조를 취하든 개발자의 기호대로 선택하면 되지만 대규모 프로젝트로 갈수록 **도메인형 구조가 더 유리해지는 경향**이 있다.