# **Abstract Class**,**Interface**

코드의 **재사용성**과 **다형성** 구현에 도움을 주는 **추상 클래스(Abstract Class)** 와 **인터페이스(Interface)** 에 대해 본 문서(TIL)에서 알아본다.

## **추상 클래스(Abstract Class)**
+ 추상 클래스는 일반 클래스와 다르게 **추상적인** 데이터를 담고 있는 클래스로 하위 클래스로 하여금 **오버라이드**를 통한 완성을 강제하도록 한 클래스이다.
+ 추상 클래스에는 여러가지 특징이 있다.
    + **추상 메서드**와 일반적인 메서드 둘 다 **선언 가능**하다.
    + 추상 클래스를 상속받는 모든 하위 클래스들은 추상 메서드를 **반드시 재정의** 해야만 한다.
    + **``new``** 키워드 사용을 통한 **인스턴스화**가 불가하다.
    + 추상 클래스를 선언 할때는 **``abstract``** 키워드를 이용한다.
        ```java
        public abstract class Test_Class {  // 추상 클래스 선언

            public abstract void abstractMethod();  // 추상 메서드 선언

            protected void normalMethod() {
                System.out.println("일반 메서드");
            }
        }
        ```
    + 추상 클래스를 사용할 때에는 무조건 클래스 내부에 추상 메서드가 **하나 이상**은 존재 해야만 한다.
    + ``new``키워드를 사용하지 못한다고 추상 클래스의 인자를 제어하지 못한다는 말은 아닌것이 **``super``** 키워드를 통해 하위 클래스의 인스턴스가 실행될 때 상위 클래스의 생성자 실행을 **선행**하여 인자 제어가 가능하다.
        ```java
        public abstract class Test_Class1 {
        
            public int num;

            public Test_Class1(int num) {
                this.num = num;
            }

            public abstract void abstractMethod();
        }

        public class Test_Class2 extends Test_Class1 {
            
            public Test_Class2(int num) {
                super(num);  // super를 통해 Test_Class1 메서드 호출
            }

            @Override
            public void abstractMethod() {
                System.out.println("추상 메서드 구체화");
            }
        }
        ```
## **인터페이스(Interface)**
+ 추상 클래스는 **다중 상속**을 지원하지 않기에 인터페이스를 이용하여 **다중 상속**을 구현한다.
+ 기본적인 **틀**을 제공하면서 다른 클래스 간의 **중간매개의 역할**까지 담당하는 일종의 **추상 클래스**라 할 수도 있다.
    ```java
    public interface Animal {  // 인터페이스
        public void eat();
    }
    ```
    ```java
    public class Dog implements Animal {
        @Override
        public void eat() {
            System.out.println("개가 뭔갈 먹는다");
        }
    }

    public class Cat implements Animal {
        @Override
        public void eat() {
            System.out.println("고양이가 뭔갈 먹는다");
        }
    }
    ```
+ 아래 이미지에서 볼 수 있듯이 아우디,벤츠,기아 세 회사의 캠핑카 클래스들은 기본적으로 **Car**라는 카테고리에 속하지만 **CamPingCar**라는 특수성 역시 지니고 있다,이런 상황에서 캠핑카로서 가춰야 할 기본적인 기능의 틀을 제공하는 것이 **CamPingCar 인터페이스** 인 것이다.

    <img src="https://velog.velcdn.com/images/ung6860/post/5e2a025f-1476-42d3-81aa-da798b8ced51/image.png" width=600>

+ 인터페이스의 기본문법에서 몇가지 유의할 점이 있다.
    + Java 8 이후부터는 **상수**와 **추상 메서드**,**디폴트(default) 메서드**까지 보유할 수 있다.
    + 클래스와는 달리 모든 필드는 **``public static final``** 로 선언하며 이로 인해 모든 필드는 **상수**가 된다.
    + 추상 메서드의 **``abstract``** 키워드는 생략가능하며 생략하더라도 **추상 메서드**로 인식한다.
    + 디폴트 메서드는 **실행코드**까지 정의하며 구현 객체에서 해당 메서드를 호출해 **사용이 가능**하다.
    + 추상 클래스와 마찬가지로 직접 **``new``** 키워드를 통한 인스턴스를 생성할 수 없다, 또한 추상 메서드를 구현 객체에게 **강제구현(Override)** 할 것을 요한다.
+ 인터페이스로도 **다형성**을 구현 가능하다.
    ```java
    Animal animal = new Cat();  // 인터페이스 타입으로 객체 생성

    public void send(Animal animal) { ... }  // 인터페이스 타입으로 매개변수 전달

    public Animal returnInterface() {  // 반환 타입을 인터페이스 타입으로 설정
        Animal animal = new Cat();
        return animal;
    }
    ```
+ 앞서 언급했듯이 디폴트 메서드를 구현할때 실제 동작을 정의가능 하며 재정의가 필요한 경우 **재정의**도 가능하다.
    ```java
    public interface Animal {
        public default void introduce() {  // 디폴트 메서드 선언
            System.out.println("나는 동물이다");
        }
    }

    public class Cat implements Animal {
        public void catIntroduce() {
            Cat cat = new Cat();
            cat.introduce();  // 이미 구현된 디폴트 메서드 사용
        }
    }

    public class Dog implements Animal {
        @Override
        public void introduce() {  // 디폴트 메서드 오버라이드로 재정의
            System.out.println("나는 멍뭉이다");
        }
    }

    public class Example {
        public static void main(String[] args) {
            Cat cat = new Cat();
            cat.catIntroduce();  // "나는 동물이다" 출력

            Dog dog = new Dog();
            dog.introduce();  // "나는 멍뭉이다" 출력
        }
    }
    ```
+ 동일한 디폴트 메서드를 2개 가진 인터페이스를 구현하는 클래스는 바로 사용하면 컴파일러는 어떤 메서드를 구현하는 것인지 알 수 없어 **오류**가 발생하기 때문에 **오버라이드**를 사용해 오류를 방지할 수 있다.
    ```java
    public interface Animal1{
        public default void introduce(){  // 디폴트 메서드 1 선언
            System.out.println("나는 동물1이다");
        }
    }

    public interface Animal2{
        public default void introduce(){  // 디폴트 메서드 2 선언
            System.out.println("나는 동물2이다");
        }
    }

    public class Cat implements Animal1, Animal2{
        @Override
        public void introduce(){  // 오버라이딩
            Animal1.super.introduce();
            Animal2.super.introduce();
            System.out.println("나는 고양이다");
        }

        public static void main(String[] args){
            Cat cat = new Cat();
            cat.intoroduce();
        }
    }
    ```
+ 위 코드에선 오버라이드를 하여 3개의 메서드가 동시에 실행되도록 하였는데 **``super``** 키워드로 직접 하나의 메서드를 지목하는 방식으로도 해결 가능하다.
+ 만약 클래스를 상속받고 인터페이스를 구현한 객체가 동일한 메서드명을 가지고 있다면 **상속받은 클래스**가 더 우선순위가 높다고 판단하고 상속 받은 클래스의 메서드를 **호출**한다.
    ```java
    public class ParentClass {
        public void introduce() {
            System.out.println("상위 클래스 메서드");
        }
    }

    public interface Animal {
        public default void introduce() {
            System.out.println("인터페이스 메서드");
        }
    }

    public class Cat extends ParentClass implements Animal {
        public static void main(String[] args) {
            Cat cat = new Cat();
            cat.introduce();  // 출력: 부모 클래스 메서드
        }
    }
    ```
## **추상 클래스 vs 인터페이스**

|추상 클래스|인터페이스|
|---|---|
|상속 받아 **기능을 확장**하는데 목적|구현 객체의 **동일한 실행 기능을 보장**하는 것이 목적|
|일반 메서드 **정의가 가능**|기본적으로 불가능 하다 Java 8 이후로 **static,default 메서드 정의 가능**|
|**클래스와 동일**하게 변수 선언 및 사용 가능|**상수**만 사용 가능|
|**extends** 키워드로 사용|**implements** 키워드로 사용|
|다중 상속이 **불가능**| 다중 상속을 **지원**|