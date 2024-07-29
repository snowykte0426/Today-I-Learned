# **Class**,**Method**,**Field**,**Object**,**Instance**

**객체 지향 언어**과 **JVM기반 언어**에서 핵심개념인 **클래스(Class)**,**메서드(Method)**,**필드(Field)**,**객체(Object)**,**인스턴스(Instance)**에 관해 이 문서(TIL)에서 설명한다.

## Class(클래스)
+ 프로그램에서 사용될 객체의 틀을 **정의**하며 **객체를 생성하기 위한 설계도** 역할을 한다.
+ 클래스는 여러 **멤버 변수**와 **메서드**를 포함하며 이를 이용해 객체의 **동작**을 정의한다.


|구성요소|역할|
|---|---|
|멤버 변수|클래스 내에서 사용되는 변수,클래스의 **속성**을 나타냄.**인스턴스 변수(instance variable)** 이라고도 함|
|메서드|클래스 내에서 사용되는 함수,클래스의 **동작**을 나타냄.**인스턴스 메서드(instance method)** 라고도 함|
|생성자|클래스로부터 객체를 생성할 때 **호출**되는 **특별한 메서드**.객체의 **초기화**를 수행|

## Method(메서드)
+ C와 같은 타 언어의 **함수**와 비슷한 개념으로 볼 수 있다.
+ 위 표에서 언급되었듯 클래스(정확히는 **객체**)의 **동작**을 나타낸다.
+ 메서드명을 정할 때는 동작을 나타내는 것이기 때문에 **동사**를 사용한다.
```java
public class Method {
    public static void sayHello() {     //sayHello 메서드
        System.out.println("Hello!");
    }

    public static void main(String[] args) {
        sayHello();     //sayHello 메서드 호출
    }
}
```

## Field(필드)
- 클래스에서 정의된 **변수**로 클래스(정확히는 **객체**)의 **상태**나 **속성**을 나타낸다.
- 필드는 **3가지**로 분류 가능한데
  - **인스턴스 변수(instance variable)**: **인스턴스(클래스를 통해 생성된 객체; instance)** 의 속성을 저장하기 위한 변수로 각 객체마다 **독립적**으로 존재하고 `new` 생성자를 통해 인스턴스가 **생성**될 때 만들어진다.
    ```java
    class Variable {
        int instanceVariable = 10;

        public static void main(String[] args) {
            Variable instance = new Variable();
            System.out.println(instance.instanceVariable);
        }
    }
    ```
  - **클래스 변수(class variable)**: **인스턴스 변수와는 다르게 공통된 저장공간을 점유**하며 모든 인스턴스들이 공통의 값을 가져야 하는 경우에 ``static`` 키워드로 선언한다.
    ```java
    class Variable {
        static int classVariable = 10;

            public static void main(String[] args) {
                System.out.println(Variable.classVariable);
            }
        }
    ```
   - **지역 변수(local variable)**: 멤버 변수와는 구분되며 **메서드 안에서 선언되고 사용**되고 메서드가 종료되는 동시에 **소멸**한다.
        ```java
        class Variable {
                public static void main(String[] args) {
                    int localVariable = 10;
                    System.out.println(localVariable);
                }
            }
        ```
## Object(객체)
+ 클래스에서 **정의**된 속성과 동작을 가지는 클래스의 **구현체**이다.
+ Java 공식 문서에선 **클래스의 인스턴스나 배열**이라고 정의하고 있다.
## Instance(인스턴스)
+ 클래스를 바탕으로 **실체화**되어 메모리에 할당된 실체를 말하는 것으로 객체와 유사한 개념이지만 **객체는 소프트웨어 세계에 구현할 대상**,**인스턴스는 설계도에 따라 소프트웨어 세계에 구현된 실체**라 할 수 있다.
+ 인스턴스는 객체에 포함된다고도 볼 수 있다.
```java
public class Animal {
    public static String habitat = "Forest";  // 클래스 변수

    private String color;  // 인스턴스 변수
    private String species;  // 인스턴스 변수

    public Animal(String color, String species) {
        this.color = color;
        this.species = species;
    }

    public void move() {
        System.out.println("The " + color + " " + species + " is moving.");
    }

    public void rest() {
        System.out.println("The " + color + " " + species + " is resting.");
    }

    public String getColor() {
        return color;
    }

    public String getSpecies() {
        return species;
    }

    public static void main(String[] args) {
        String title = "Animal";  //지역 변수 선언
        System.out.println(title);  // Animal 출력

        // 클래스 Animal의 객체 생성
        Animal myAnimal = new Animal("brown", "Deer");
        Animal yourAnimal = new Animal("grey", "Elephant");

        // 객체의 필드 접근
        System.out.println(myAnimal.getColor());  // brown 출력
        System.out.println(yourAnimal.getSpecies());  // Elephant 출력

        // 클래스 변수 접근
        System.out.println(Animal.habitat);  // Forest 출력

        // 객체의 메소드 호출
        myAnimal.move();  // The brown Deer is moving 출력
        yourAnimal.rest();  // The grey Elephant is resting 출력
    }
}
```