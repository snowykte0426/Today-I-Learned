# **Annotation**
Java와 Spring 프레임워크에서 사용되는 **어노테이션(Annotation)** 들에 대해 이 문서(TIL)에서 살펴본다.
## 어노테이션(Annotation)
+ **사전적 의미**로는 **'주석'** 이라는 뜻이지만 Java에서는 코드 사이에 **특별한 의미,기능**을 수행하도록 하는 기술이다.
### Java 표준 어노테이션
+ **``@Override``**
    + 메서드가 상위 클래스의 메서드를 **오버라이드(재정의)** 하고 있다는 것을 명시한다.
        ```java
        @Override
        public String toString() {
            return "String!!";
        }
        ```
+ **``@Deprecated``**
    + 해당 요소가 더 이상 **사용되지 않음**을 나타낸다.
        ```java
        @Deprecated
        public void setGender(boolean gender) {
            this.gender = gender;
        }
        ```
+ **``@SuppressWarnings``**
    + 특정 컴파일러 경고를 **무시**하도록 한다.
        ```java
        @SuppressWarnings("unchecked")
        public void myMethod() {
            List rawList = new ArrayList();
        }
        ```
+ **``@Retention``**
    + 어노테이션의 **유지 정책(컴파일 과정에서부터 언제까지 유지될지)** 을 **설정**한다.
        ```java
        @Retention(RetentionPolicy.RUNTIME)
        public @interface CustomAnnotation { }
        ```
+ **``@Target``**
    + 어노테이션이 **적용될 수 있는 요소**를 정의한다.
        ```java
        @Target(ElementType.METHOD)
        public @interface MyAnnotation{ }
        ```
+ **``@Documented``**
    + 해당 어노테이션이 **Javadoc**에 포함되어야 함을 나타낸다.
    ```java
    @Documented
    public @interface MyDocumentedAnnotation { }
    ```
+ **``@Inherited``**
    + 어노테이션이 자식 클래스에 **상속**됨을 나타낸다.
    ```java
    @Inherited
    @Retention(RetentionPolicy.RUNTIME)
    @Target(ElementType.TYPE)
    public @interface MyInheritedAnnotation { }
    ```
### Spring 어노테이션
+ **``@Entity``**
    + **JPA(Java Persistence API)** 에서 사용되는 어노테이션으로 클래스를 **DB 테이블**과 매핑한다.
    ```java
    @Entity
    public class User {
        @Id
        @GeneratedValue(strategy = GenerationType.AUTO)
        private Long id;

        private String name;
    }
    ```
+ **``@Autowired``**
    + Spring에서 자동으로 **의존성을 주입**할 때 사용한다.
    ```java
    @Service
    public class UserService {
        @Autowired
        private UserRepository userRepository;
    }
    ```
+ **``@Component``**
    + Spring 컨테이너가 관리하는 일반적인 **빈(Bean)**을 정의한다.
    ```java
    @Component
    public class PrintLn {
        public void doSomething() {
            System.out.println("PrintLn");
        }
    }
    ```
+ **``@Service``**
    + **비즈니스 로직**을 포함하는 클래스에 붙이며 **서비스 레이어의 빈**을 정의할 때 사용한다.
    ```java
    @Service
    public class MyService {
        private final MyComponent myComponent;

        @Autowired
        public MyService(MyComponent myComponent) {
            this.myComponent = myComponent;
        }

        public String process() {
            return myComponent.getMessage() + " - Processed by MyService";
        }
    }
    ```
+ **``@Repository``**
    + **DAO** 클래스를 정의할 때 사용되며, DB와 **상호작용**을 담당한다.
    ```java
    @Repository
    public class MyRepository {
        private final Map<Integer, String> dataStore = new HashMap<>();

        public void save(int id, String data) {
            dataStore.put(id, data);
        }

        public String findById(int id) {
            return dataStore.get(id);
        }
    }
    ```
+ **``@Controller``**
    + **Spring MVC**에서 컨트롤러 클래스를 정의할 때 사용하며, 요청을 처리하고 뷰(View)에 데이터를 반환한다.
    ```java
    @Controller
    public class MyController {
        @RequestMapping("/home")
        public String home() {
            return "home";
        }
    }
    ```
+ **``@RestController``**
    + **RESTful** 웹서비스의 컨트롤러를 정의할 때 사용하며 ``@Controller``과 ``@ResponseBody``를 결합한 형태이다.
    ```java
    @RestController
    public class MyRestController {
        @GetMapping("/api/greet")
        public String greet() {
            return "Hello, World!";
        }
    }
    ```
+ **``@Qualifier``**
    + 같은 타입의 여러 빈 중에서 **특정 빈**을 주입할 때 사용한다.
    ```java
    @Service
    public class MyService {
        @Autowired
        @Qualifier("specificBean")
        private MyRepository myRepository;
    }
    ```
+ **``@Value``**
    + 프로퍼티 값을 주입할 때 사용하며 주로 **'application.properties'** 또는  **'application.yml'**의 값을 주입할 때 사용한다.
    ```java
    @Component
    public class MyComponent {
        @Value("${my.property}")
        private String myProperty;
    }
    ```
+ **``@RequestMapping``**
    + **특정 URL 패턴**과 컨트롤러 메서드를 매핑한다.
    ```java
    @Controller
    public class MyController {
        @RequestMapping("/home")
        public String home() {
            return "home";
        }
    }
    ```
+ **``@GetMapping, @PostMapping, @PutMapping, @DeleteMapping, @PatchMapping``**
    + **HTTP 메서드(GET,POST,PUT,DELETE,PATCH)** 에 대응하는 요청을 매핑한다.
    ```java
    @RestController
    @RequestMapping("/api/items")
    public class MyRestController {
        private final List<String> items = new ArrayList<>();

        @GetMapping
        public List<String> getItems() {
            return items;
        }

        @PostMapping
        public String createItem(@RequestBody String item) {
            items.add(item);
            return "Item created: " + item;
        }

        @PutMapping("/{id}")
        public String updateItem(@PathVariable int id, @RequestBody String item) {
            if (id >= 0 && id < items.size()) {
                items.set(id, item);
                return "Item updated: " + item;
            } else {
                return "Item not found";
            }
        }

        @DeleteMapping("/{id}")
        public String deleteItem(@PathVariable int id) {
            if (id >= 0 && id < items.size()) {
                return "Item deleted: " + items.remove(id);
            } else {
                return "Item not found";
            }
        }

        @PatchMapping("/{id}")
        public String patchItem(@PathVariable int id, @RequestBody String item) {
            if (id >= 0 && id < items.size()) {
                items.set(id, item);
                return "Item patched: " + item;
            } else {
                return "Item not found";
            }
        }
    }
    ```
+ **``@PathVariable``**
    + **URL 경로**에서 **값을 추출**할 때 사용한다.
    ```java
    @RestController
    public class MyRestController {
        @GetMapping("/api/items/{id}")
        public Item getItemById(@PathVariable Long id) {
            return itemService.getItemById(id);
        }
    }
    ```
+ **``@RequestParam``**
    + **요청 매개변수의 값**을 **메서드 파라미터**로 바인딩할 때 사용한다.
    ```java
    @RestController
    public class MyRestController {
        @GetMapping("/api/items")
        public List<Item> getItemsByCategory(@RequestParam String category) {
            return itemService.getItemsByCategory(category);
        }
    }
    ```
+ **``@Scheduled``**
    + 메서드를 일정 간격으로 실행되도록 **스케쥴링**할 때 사용되며 **``@EnableScheduling``** 를 먼저 설정해주어야만 한다.
    ```java
    @Component
    public class MyScheduledTask {
        @Scheduled(fixedRate = 5000)
        public void performTask() {
            System.out.println("Task performed every 5 seconds");
        }
    }
    ```
+ **``@Transactional``**
    + 메서드나 클래스에 **트랜잭션 처리를 적용**할 때 사용하며, 어노테이션이 적용된 범위 내에서 수행된 DB 작업중 하나라도 실패하면 **롤백(RollBack)**하고 성공하면 **커밋(Commit)** 한다.
    ```java
    @Service
    public class UserService {
        private final UserRepository userRepository;

        @Transactional
        public User createUser(User user) {
            return userRepository.save(user);
        }

        @Transactional
        public User updateUser(Long id, User userDetails) {
            return userRepository.findById(id).map(user -> {
                user.setName(userDetails.getName());
                user.setEmail(userDetails.getEmail());
                return userRepository.save(user);
            }).orElseThrow(() -> new RuntimeException("User not found"));
        }

        @Transactional
        public void deleteUser(Long id) {
            userRepository.deleteById(id);
        }
    }
    ```
+ **``@Async``**
    + 메서드를 **비동기적**으로 실행하도록 하며 이를 위해서는 **``@EnableAsync``** 를 설정해야만 한다.
    ```java
    @Service
    public class MyService {
        @Async
        public void performAsyncTask() {
            try {
                Thread.sleep(2000); // Simulate a delay
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
            System.out.println("Asynchronous task completed");
        }
    }
    ```
+ **``@Configuration``**
    + **Spring 설정 클래스**를 정의할 때 쓰인다.
    ```java
    @Configuration
    public class AppConfig {

        @Bean
        public MyService myService() {
            return new MyService();
        }

        @Bean
        public MyRepository myRepository() {
            return new MyRepository();
        }
    }
    ```
+ **``@Bean``**
    + 메서드에서 반환된 객체를 Spring 컨테이너에 **빈**으로 등록할 때 사용된다.
        ```java
        @Bean
        public MyService myService() {
            return new MyService();
        }
        ```
+ **``@Scope``**
    + 빈이 적용되는 **범위**를 정의한다.
    ```java
    @Component
    @Scope("prototype")
    public class PrototypeBean {
        public void execute() {
            System.out.println("prototype bean!!!!!!!");
        }
    }
    ```
+ **``@Lazy``**
    + 빈을 **지연 초기화**할 때 사용된다.즉, 해당 빈이 실제로 필요해질 때까지 초기화를 지연한다.
    ```java
    @Component
    @Lazy
    public class LazyBean {
        public LazyBean() {
            System.out.println("LazyBean initialized");
        }
    }
    ```
+ **``@Primary``**
    + 같은 타입의 빈들 중 **기본 빈**을 지정할 때 사용한다.
    ```java
    interface MyInterface {
        void execute();
    }

    @Component
    @Primary
    public class PrimaryBean implements MyInterface {
        @Override
        public void execute() {
            System.out.println("Primary bean executing logic");
        }
    }

    @Component
    public class SecondaryBean implements MyInterface {
        @Override
        public void execute() {
            System.out.println("Secondary bean executing logic");
        }
    }
    ```
+ **``@Profile``**
    + 특정 환경에서**만** 활성화되는 빈을 정의한다.
    ```java
    @Component
    @Profile("dev")
    public class DevBean {
        public void execute() {
            System.out.println("Dev bean executing logic");
        }
    }
    ```
+ **``@RestControllerAdvice``**
    + REST 컨트롤러 전역에서 **예외 처리**를 수행할 때 사용된다.
    ```java
    @RestControllerAdvice
    public class GlobalExceptionHandler {

        @ExceptionHandler(RuntimeException.class)
        public ResponseEntity<String> handleRuntimeException(RuntimeException ex) {
            return ResponseEntity.status(500).body("An error occurred: " + ex.getMessage());
        }
    }
    ```
+ **``@CroosOrigin``**
    + **CORS(Cross-Origin Resource Sharing; 다른 출처의 리소스를 안전하게 사용하도록 하는 것)** 를 설정할 때 사용된다
    ```java
    @RestController
    public class MyRestController {

        @CrossOrigin(origins = "http://example.com")
        @GetMapping("/api/data")
        public String getData() {
            return "Data from server";
        }
    }
    ```
+ **``@EventListener``**
    + **Spring Event**를 처리할 때 사용한다.
    ```java
    @Component
    public class MyEventListener {

        @EventListener
        public void handleContextRefresh(ContextRefreshedEvent event) {
            System.out.println("Context refreshed: " + event);
        }
    }
    ```
+ **``@Conditional``**
    + **조건부**로 빈을 등록할 때 사용되며 조건은 **'Condition' 인터페이스를 구현하여 정의**한다.
    ```java
    public class MyCondition implements Condition {

        @Override
        public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
            return context.getEnvironment().containsProperty("my.property");
        }
    }

    @Component
    @Conditional(MyCondition.class)
    public class ConditionalBean {
        public void execute() {
            System.out.println("Conditional bean executing logic");
        }
    }
    ```
+ **``@Import``**
    + 다른 구성 클래스를 **가져올 때** 사용한다.
    ```java
    @Configuration
    @Import({AppConfig.class, OtherConfig.class})
    public class MainConfig {
    }
    ```
### Lombok 어노테이션
+ **``@Getter``**
    + 클래스의 필드에 대한 **getter 메서드**를 **자동으로 생성**한다.
        ```java
        @Getter
        public class User {
            private String name;
            private int age;
        }
        ```
+ **``@Setter``**
    + 클래스의 필드에 대한 **setter 메서드**를 **자동으로 생성**한다.
        ```java
        @Setter
        public class User {
            private String name;
            private int age;
        }
        ```
+ **``@ToString``**
    + 클래스의 **toString 메서드**를 자동으로 생성하며 모든 필드를 포함하거나 일부 필드만 포함시킬 수 있다.
    ```java
    @ToString
    public class User {
        private String username;
        private int age;
    }
    ```
+ **``@EqualsAndHashCode``**
    + **equals 메서드**와 **hashCode 메서드**를 자동으로 생성하며 기본적으로 모든 필드를 포함하지만 일부 필드만을 포함 시킬수도 있다.
    ```java
    @EqualsAndHashCode
    public class User {
        private String username;
        private int age;
    }
    ```
+ **``@NoArgsConstructor``**,**``@AllArgsConstructor``**,**``@RequiredArgsConstructor``**
    + ``@NoArgsConstructor``는 파라미터가 **없는** 기본 생성자를 **자동 생성**하고 ``@AllArgsConstructor``는 **모든 필드를 파라미터로 받는 생성자**를 생성,``@RequiredArgsConstructor``는 **``final``** 필드나 **``@Nonnull``** 이 선언된 필드를 파라미터로 받는 생성자를 생성한다.
    ```java
    @NoArgsConstructor
    @AllArgsConstructor
    @RequiredArgsConstructor
    public class User {
        private Long id;
        @NonNull private String username;
        @NonNull private int age;
    }
    ```
+ **``@Data``**
    + **``@Getter``**,**``@Setter``**,**``@ToString``**,**``@EqualsAndHashCode``**,**``@RequiredArgsConstructor``** 를 전부 포함하는 어노테이션이다.
    ```java
    @Data
    public class User {
        private Long id;
        private String username;
        private int age;
    }
    ```
+ **``@Builder``**
    + **빌터 패턴(Builder Pattern)** 을 사용하여 객체를 생성하도록 한다.
    ```java
    @Builder
    public class User {
        private Long id;
        private String username;
        private int age;
    }
    ```
+ **``@Value``**
    + **불변 객체**를 만들기 위해 사용되고 **``@Getter``**, **``@ToString``**, **``@EqualsAndHashCode``**, **``@AllArgsConstructor``**, **``@FieldDefaults(makeFinal=true, level=AccessLevel.PRIVATE)``** 를 모두 포함하며 모든 필드는 기본적으로 **``final``** 로 선언된다.
    ```java
    @Value
    public class User {
        private Long id;
        private String username;
        private int age;
    }
    ```
+ **``@NonNull``**
    + **필드**,**메서드 파라미터**,**메서드 반환값**에 **Null 검사**를 추가하고 **``@RequiredArgsConstructor``** 과 함께 사용하여 필드의 Null 가능성을 배제할 수 있다.
    ```java
    public class User {
        private Long id;
        @NonNull private String username;
        @NonNull private int age;

        public void setUsername(@NonNull String username) {
            this.username = username;
        }
    }
    ```
+ **``@Slf4j``**,**``@Log``**,**``@Log4j``**,**``@Log4j2``**
    + **로그 객체**를 자동으로 생성해 **로깅 기능**을 쉽게 사용할 수 있도록 한다.
    ```java
    @Slf4j
    public class User {
        public void doSomething() {
            log.info("Doing something...");
        }
    }
    ```