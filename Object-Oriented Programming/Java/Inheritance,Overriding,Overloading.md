# **Inheritance**,**Overriding**,**Overloading**

**객체 지향 언어**와 **JVM 기반 언어**에서 핵심 개념이자 기술인 **상속(Inheritance)**,**오버라이딩(Overriding)**,**오버로딩(Overloading)** 에 관해 이 문서(TIL)에서 알아본다.

## 상속(Inheritance)
+ **상위 클래스**의 모든 것을 **하위 클래스**가 물려 받는 것을 의미한다.
+ **부모-자식 관계**로 정의하기도 하며 그때는 **부모 클래스**,**자식 클래스**로 구분한다.
+ 코드의 **재사용성**과 **간결성** 향상을 목표로 한다.
+ 상속을 통해 객체 지향의 특성인 **다형성**을 구현 가능하고 코드의 **확장성**을 확보하여 유연성을 늘릴 수 있다.
+ 상속을 할때는 **``extends``** 키워드를 이용해 상속을 구현한다.
    ```java
    class Animal {
        void eat() {
            System.out.println("냠냠");
        }
    }

    class Dog extends Animal {  //Animal 클래스를 상속 받음
        void bark() {
            System.out.println("멍멍");
        }
    }

    public class main {
        public static void main(String[] args) {
            Dog dog = new Dog();  //Dog 클래스를 사용하기 위해 객체 생성
            dog.eat();  //상위 클래스로부터 상속받은 eat 메서드 사용
            dog.bark();  //자기 자신의 bark 메서드 사용
        }
    }
    ```
## 오버라이딩(Overriding)
+ **상위 클래스**에서 상속받은 메서드를 **하위 클래스**에서 **재정의**하는 것을 의미한다.
+ 오버라이딩을 구현하기 위해선 여러 조건이 만족되어야 한다.
    + **메서드명**과 메서드의 매개변수 **타입**,**개수**,**순서**가 같아야 한다.
    + 메서드의 **반환 타입**이 같아야 한다,단 Java,C++과 같이 **공변 반환 타입(covariant return type)** 을 지원하는 언어에서는 상위 클래스의 반환 타입의 하위 타입일 경우 사용이 가능하다.
        ```java
        // 상위 클래스
        class Animal {
            Animal reproduce() {
                return new Animal();
            }
        }

        // 하위 클래스
        class Dog extends Animal {
            @Override
            Dog reproduce() { // 공변 반환 타입: Animal의 하위 타입인 Dog 반환
                return new Dog();
            }
        }
        ```
    + 접근제어자가 **같거**나 **더 넓은 범위**여야만 한다.
        ```java
        class Animal {
            public void eat() {
                System.out.println("냠냠");
            }
        }

        class Dog extends Animal {
            private void eat() {  //오버라이딩이 성립되지 않음
                System.out.println("냠냠냠냠");
            }
        }
        ```
    + 메서드가 **상위 클래스**에서 **``final``** 이나 **``static``** 으로 선언되지 않아야 한다,``final``은 더 이상 **수정될 수 없음**을 나타내는 키워드로 오버라이딩이 **불가**하고 ``static``은 오버라이딩과 코드 형태는 같아보여도 **숨김(하이딩; Hiding)** 이라고 정의한다.
+ Java에서는 **``@Overriding``** 어노테이션으로 컴파일러에게 오버라이딩을 명시함 으로써 명확히 할 수 있다.
    ```java
    // 상위 클래스
    class Animal {
        public void makeSound() {
            System.out.println("(동물의 울음소리)");
        }
    }

    // 하위 클래스
    class Dog extends Animal {
        @Override
        public void makeSound() { // 오버라이딩
            System.out.println("멍멍");
        }
    }
    ```
## 오버로딩(Overloading)
+ C언어에선 함수명은 고유하게 지정되지만 객체지향 프로그래밍을 지원하는 언어들에서는 **하나의 함수명으로 여러 기능을 구현 가능**하다.
+ 오버로딩을 구현하기 위해선 여러 조건이 만족되어야 한다.
    + **메서드명**이 같고 **매개변수**의 **개수**나 **타입**이 달라야만 한다.
    + **반환 타입**만 달라서는 오버로딩을 구현 할 수 **없다**.
    ```java
    class OverloadingMethods {
        public void print() {  //매개변수 X
            System.out.println("오버로딩1");
        }

        String print(Integer a) {  // Integer 타입 매개변수 a
            System.out.println("오버로딩2");
            return a.toString();
        }

        void print(String a) { // String 타입 매개면수 a
            System.out.println("오버로딩3");
            System.out.println(a);
        }

        String print(Integer a, Integer b) {  // Integer 타입 매개변수 a, b
            System.out.println("오버로딩4");
            return a.toString() + b.toString();
        }

    }
    ```
+ 오버라이딩과 **다르게** 접근제어자 설정에 **제약이 없다**.
+ 오직 오버로딩은 **매개변수**의 차이로만 구현가능하다.
## 오버라이딩 vs 오버로딩
| 구분 | Overriding | Overloading |
|---|---|---|
| 접근 제어자 | 부모 클래스의 메서드의 접근 제어자보다 더 넓은 범위의 접근 제어자를 자식 클래스의 메서드에서 설정할 수 있다. | 모든 접근 제어자를 사용할 수 있다. |
| 리턴형 | 동일해야 한다. (공변 반환 타입 허용) | 달라도 된다. |
| 메서드명 | 동일해야 한다. | 동일해야 한다. |
| 매개변수 | 동일해야 한다. | 달라야만 한다. |
| 적용 범위 | 상속 관계에서 적용된다. | 같은 클래스 내에서 적용된다. |
