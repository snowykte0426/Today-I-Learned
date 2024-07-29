# Access Modifier,Static,Get,Set,Encapsulation
Java의 중요 문법인 **접근제어자(Access Modifier)**,**``static``**,**``get``**,**``set``** 과 객체지향의 중요 개념인 **캡슐화**에 대해 이 문서(TIL)에서 알아본다.
## 접근제어자(Access Modifier)
+ Java에서 클래스와 클래스의 멤버를 사용할 때 **접근 가능한 범위**를 지정해 주는 것이 접근제어자이다.

    |이름|설명|사용범위|
    |---|---|---|
    |**public**|패키지(pakage)와 상관없이 **모든 클래스**에서 접근이 가능|**클래스**,**클래스 멤버**|
    |**private**|**같은 클래스**안에 있는 멤버들만 접근 가능|**클래스 멤버**|
    |**protected**|**같은 패키지 안의 모든 클래스와 상속받은 자식 클래스**에서만 사용 가능|**클래스 멤버**|
    |**default**|**명시적으로 표기하지는 않고 같은 패키지 안의 클래스**에서만 사용 가능|**클래스**,**클래스 멤버**|
    |
    ```java
    public class Book {       // public 클래스

        public int a;          // public 멤버변수(필드)
        private int b;         // private 멤버변수(필드)
        protected int c;       // protected 멤버변수(필드)
        int d;                  // default 멤버변수(필드)
        
        
        public Book() {   }    // public 생성자
        
        
        public void showA() { ... }     // public 멤버함수(메소드)
        private void showB() { ... }    // private 멤버함수(메소드)
    }
    ```
## static
+ **static한 변수**
    + 한 클래스에서 **공통적인 값을 유지**해야 할 때 사용한다.
    + 클래스가 메모리에서 로딩될 때 **생성**되어 **프로그램이 종료**될 때 까지 유지된다.
    + **객체를 생성하지 않고도** ``클래스 이름.변수명``의 형태로 **호출이 가능**하다.
        ```java
        // static 변수 (클래스 변수)
        public class MyMathStaticBasic {
            public static final String DESCRIPTION = "static 변수";
        }
        
        // 인스턴스 변수
        public class MyMathBasic {
            public long a;
            public long b;
            public String description = "인스턴스 변수";
        }
        ```
        ```java
        @Test
        public void staticStr() {
            //static 변수는 인스턴스를 생성하지 않아도 사용할 수 있다.
            System.out.println("static 변수 출력 = " + MyMathStaticBasic.DESCRIPTION);
        
            //인스턴스 변수는 인스턴스를 생성해야 사용할 수 있다.
            MyMathBasic mathBasic = new MyMathBasic();
            System.out.println("인스턴스 변수 출력 = " + mathBasic.description);
        }
        ```
        ```bash
        static 변수 출력 = static 변수
        인스턴스 변수 출력 = 인스턴스 변수
        ```
+ **static한 메서드**
    + **인스턴스 변수**를 사용 불가하므로 인스턴스와 관계없는 메서드를 **static 메서드(클래스 메서드)** 라 칭한다.
    + static 변수와 마찬가지로 ``클래스 이름.메서드명``의 형태로 **호출 가능**하다.
## get,set
+ **get 메서드**는 **``private``** 로 선언된 필드에 대해 접근해서 값을 읽어 오는 메서드로 반드시 **``return`` 키워드를 요구**한다.
+ **set 메서드**는 역시 **``private``** 로 선언된 필드에 대해 접근해서 값을 **수정**하는 메서드로 get과는 다르게 **``return`` 키워드가 불필요하다**
```java
public class Person {
    // 필드 정의
    private String name;
    private int age;

    // name 필드의 getter 메서드
    public String getName() {
        return name;
    }

    // name 필드의 setter 메서드
    public void setName(String name) {
        this.name = name;
    }

    // age 필드의 getter 메서드
    public int getAge() {
        return age;
    }

    // age 필드의 setter 메서드
    public void setAge(int age) {
        this.age = age;
    }

    public static void main(String[] args) {
        // Person 객체 생성
        Person person = new Person();

        // setter 메서드를 사용하여 필드 값 설정
        person.setName("John");
        person.setAge(30);

        // getter 메서드를 사용하여 필드 값 출력
        System.out.println("Name: " + person.getName());
        System.out.println("Age: " + person.getAge());
    }
}
```
## 캡슐화(Encapsulation)
+ 서로 연관있는 **속성과 기능들을 하나의 캡슐(Class)** 로 만들어 데이터를 외부로부터 **보호**하는 것을 말한다.
+ Java에서 캡슐화를 하는 이유로는 대표적으로 2가지를 들수 있는데 **데이터 보호(Data Protection)** 와 **데이터 은닉(Data Hiding)** 이 있다.
    
    |||
    |---|---|
    |**데이터 보호(Data Protection)**|외부로부터 클래스에 정의된 **속성**과 **기능**들을 **보호**|
    |**데이터 은닉(Data Hiding)**|**내부의 동작***을 감추고 외부에는 **필요한 부분만 노출**|
    |

    <img src="https://i0.wp.com/blogcodestates.com/wp-content/uploads/2022/11/%EA%B0%9D%EC%B2%B4-%EC%A7%80%ED%96%A5-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D-%ED%8A%B9%EC%A7%95-%EC%BA%A1%EC%8A%90%ED%99%94.png?w=852&ssl=1" width=30%>

+ Java에선 **접근제어자**와  **get,set** 메서드를 통해 캡슐화를 구현가능 하다.
