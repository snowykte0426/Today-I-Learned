# DAO,DTO,VO,Builder Pattern
**DAO(Data Access Object)**,**DTO(Data Transfer Object)**,**VO(Value Object)** 와 **Builder Pattern**에 대해 본 문서(TIL) 에서 알아본다.
## DAO
+ Data Access Object라는 이름처럼 **"실제로 DB의 데이터에 접근하는 객체"** 라고 정의할 수 있겠다.
+ 비즈니스 로직과 DB를 연결하는 역할을 하며 실제로 DB에 접근하여 **CRUD** 기능을 수행한다.
    ```java
    public User getUserById(int id) throws SQLException {
        String query = "SELECT * FROM users WHERE id = ?";
        PreparedStatement statement = connection.prepareStatement(query);
        statement.setInt(1, id);  // 1번째 플레이스홀더('?')에 id 값 삽입
        ResultSet resultSet = statement.executeQuery();  // 쿼리 실행
        if (resultSet.next()) {  // 커서를 다음 행(첫째 행)으로 이동시켜 값 유무 판단
            return new User(resultSet.getInt("id"), result Set.getString("name"));
        }
        return null;  // 값이 없다면 null 반환
    }
    ```
    ### DAO vs Repository
    + DAO는 **DB와 상호작용**을 추상화하고 Repository는 **도메인 객체와 상호작용**을 추상화한다.
    + DAO는 **CRUD 작업**에 좀 더 치중하지만 Repositroy는 복잡한 **도메인 로직**과 **쿼리**를 포함할 수 있다.
    + **빈약한 도메인 모델(도메인 객체에 비즈니스 로직이 거의 없거나 존재하지 않는 도메인 모델)** 에서 DAO와 Repository는 구분이 미약해질 수 있다.
        ```java
        @Repository
        public interface UserRepository extends JpaRepository<User, Integer> {
        }
        ```
## DTO
+ **"계층 간 데이터 교환을 위한 객체"** 라고 요약할 수 있겠다.
+ **로직을 가지지 않고 get/set 메서드만 가져** DB에서 데이터를 가져와 각 계층(Service,Controller 등) 으로 **전달**하는 역할을 수행한다.
    ```java
    @Getter
    @Setter  // Lombok를 이용한 get/set 메서드 자동화
    public class GetUserRes {
        private long userId;
        private String userName;
        private String email;
        private String password;
    }
    ```
## VO
+ DTO는 setter를 가지고 있어 값의 변경이 가능하지만 VO는 getter만 가지고 있어 **오직 읽기만 가능(Read-Only)** 하다.
+ 모든 속성은 **``final``** 로 선언되어 불변성을 가지며 값을 변경하려면 새로운 인스턴스를 **생성**해야만 한다.
+ 자체 식별자가 **없다**. 즉, DB의 기본 키와 같은 식별자가 없다는 뜻이다.
+ **각 속성의 값이 같다면 동등한 것으로 간주**한다.
    ```java
    @Getter  
    @EqualsAndHashCode  // Lombok를 이용한 동등성 비교용 equals,hashcode 메서드 자동화
    @RequiredArgsConstructor  // Lombok를 이용한 생성자 자동 생성
    public final class Address {
        private final String street;
        private final String city;
        private final String zipCode;
    }
    ```
## 빌더 패턴(Builder Pattern)
+ 복잡한 객체의 **생성 과정**과 **표현 방법**을 **분리**하여 다양한 구성의 인스턴스를 만드는 패턴이다.
+ 비유를 들자면 수제 햄버거를 주문할때 속재료들을 마음대로 결정하는 것과 같이 **객체의 속재료를 유연하게 받아내는 것**이라고 할 수 있다.
    ```java
    // 빌더 클래스 구현 예시
    class StudentBuilder {
        private int id;
        private String name;
        private String grade;
        private String phoneNumber;

        public StudentBuilder id(int id) {
            this.id = id;
            return this;
        }

        public StudentBuilder name(String name) {
            this.name = name;
            return this;
        }

        public StudentBuilder grade(String grade) {
            this.grade = grade;
            return this;
        }

        public StudentBuilder phoneNumber(String phoneNumber) {
            this.phoneNumber = phoneNumber;
            return this;
        }
         public Student build() {
            return new Student(id, name, grade, phoneNumber); // Student 생성자 호출
        }
    }
    ```
+ **Lombok**의 어노테이션 중 **``@Builder``** 어노테이션을 사용하면 별도로 **빌더 클래스를 생성하지 않고** 간편히 빌더 패턴을 구현할 수 있다.
    ```java
    @Getter
    @Builder
    public class BoardDto {
        private Long id;
        private String title;
        private String content;
        private String writer;

        public static BoardDto toDto(Board board) {  
            return BoardDto.builder()  // 자동 생성된 빌더 클래스
                        .id(board.getId())
                        .title(board.getTitle())
                        .content(board.getContent())
                        .writer(board.getUser().getName())
                        .build();
        }
    }
    ```
+ 빌더 패턴의 **장점**엔 여러가지가 있다.
    + 객체를 생성할 때 필요한 매개변수를 명확히 할 수 있어 **코드의 가독성**을 높이는 데 효과적이다.
    + 객체 생성시 일부 매개변수만을 선택적으로 설정하여 객체 생성에 **유연성**을 **부여**할 수 있다.
    + [OCP 법칙](https://github.com/snowykte0426/Today-I-Learned/blob/main/Object-Oriented%20Programming/SOLID.md)에 위배될 수 있는 Setter 패턴 대신 사용하여 **클래스 변수**의 **불변성**을 확보할 수 있다.
+ 빌더 패턴도 **단점** 역시 존재한다.
    + 빌더 클래스와 관련 메서드를 구현하는 과정에서 **코드의 양이 늘어날 수 있다**(Lombok과 같은 라이브러리 사용으로 해결가능).
    + 매번 메서드를 호출하고 빌더를 거쳐 인스턴스화를 수행하기에 **오버헤드가 심할 수 있다**(대부분의 경우 무시 가능할 정도이지만 일부 시스템에서는 고려사항).
