# **Casting**(**Polymorphism**)

객체지향의 다형성을 설명하기 위해 이 문서(TIL)에선 Java를 예로 들어 **형 변환**과 **다형성**에 대해 설명한다.

## 형 변환(Casting)
+ **변수**나 **리터럴**의 타입을 다른 타입으로 **변환**하는 것을 **형 변환(Casting)** 이라고 한다.
+ **문자**나 **문자열**을 **정수,실수형**으로 변환할 때는 **``valueOf``** 를 사용할 수 있다.
+ 또한 **``parseDouble``**,**``parseFloat``**,**``parseInt``** 등의 방법도 있다.
+ **``decode``** 를 이용해 **10진수**,**8진수**,**16진수** 등 다양한 진법의 정수로 변환할 수도 있다.
    ```java
    public class TypeConversionExample {
        public static void main(String[] args) {
            // 문자열->정수형
            String strInt = "123";
            int number1 = Integer.parseInt(strInt);  // parseInt
            Integer number2 = Integer.valueOf(strInt);  // valueOf
            System.out.println(number1); // 출력: 123
            System.out.println(number2); // 출력: 123

            // 문자열->실수형
            String strDouble = "123.45";
            double d1 = Double.parseDouble(strDouble);  // parseDouble
            Double d2 = Double.valueOf(strDouble);  // valueOf
            System.out.println(d1);  // 출력: 123.45
            System.out.println(d2);      // 출력: 123.45

            // 정수형->문자열
            int intNumber = 123;
            String strFromInt = String.valueOf(intNumber);  // valueOf
            System.out.println(strFromInt);  // 출력: "123"

            // 실수형->문자열
            double doubleNumber = 123.45;
            String strFromDouble = String.valueOf(doubleNumber);  // valueOf
            System.out.println(strFromDouble);  // 출력: "123.45"

            // 8진수,16진수 문자열->정수형
            String octalStr = "123";  // 8진수
            String hexStr = "7B";     // 16진수
            int octalNumber = Integer.parseInt(octalStr, 8);  //parseInt
            int hexNumber = Integer.parseInt(hexStr, 16);  //parseInt
            System.out.println(octalNumber);  // 출력: 83 (10진법)
            System.out.println(hexNumber);      // 출력: 123 (10진법)

            // 정수형->8진수,16진수 문자열
            int number = 123;
            String octalStrFromInt = Integer.toOctalString(number);  // toOctalString
            String hexStrFromInt = Integer.toHexString(number);  // toHexString
            System.out.println(octalStrFromInt);  // 출력: "173"
            System.out.println(hexStrFromInt);      // 출력: "7b"

            // 문자->정수형
            char ch = '5';
            int charNumber = Character.getNumericValue(ch);  // getNumericValue
            System.out.println(charNumber);  // 출력: 5

            // 문자열->10진수, 8진수, 16진수 정수형
            String decimalStr = "123";  // 10진수 표기
            String octalStrDecode = "0123";  // 8진수 표기
            String hexStrDecode = "0x7B";  // 16진수 표기
            int decimalNumber = Integer.decode(decimalStr);        
            int octalNumberDecode = Integer.decode(octalStrDecode);  // decode
            int hexNumberDecode = Integer.decode(hexStrDecode);       
            System.out.println(decimalNumber);  // 출력: 123 (10진법)
            System.out.println(octalNumberDecode);  // 출력: 83 (10진번)
            System.out.println(hexNumberDecode);  // 출력: 123 (10진법)
        }
    }
    ```
+ 하나의 **문자**를 형 변환 할때는 단순하게 **``(int)``** 와 같은 정수형,실수형 타입을 붙이는 방법으로 간단히 형 변환 할 수 있으며 이때는 **아스키코드 값**을 맞추기 위해 **48**을 **빼주거나 더해주어야** 한다.
    ```java
    // 문자->정수형
    char ch = '5';
    int i = (int)(ch - '0');

    // 정수형->문자
    int i = 5;
    char ch = (char)(i + '0');
    ```
+ 문자가 아닌 실수형과 정수형 사이의 형변환은 **문자와 숫자의 변환과 같이 ``(타입)``** 의 형태로 명시만 하면 된다.
    ```java
    // 정수형->실수형
    int i = 1234;

    double d = (double)i;
    float f = (float)i;
    ```
+ **자동 형변환**이 될 때도 있는데 이는 형 변환을 명시하지 않아도 컴파일러가 자동으로 **변환** 해주는 것을 뜻한다.
    ```java
    float f = 1234;  // float f = (float)1234;와 같음

    int i = 3;  // int i = (int)3;과 같음
    double d = 1.0 + i;  // double d = 1.0 + (double)i; 와 같음
    ```
+ 자동 형변환에도 **규칙**이 있는데 아래 순서의 반대로 구현해야 한다면 **수동으로 형 변환**을 해야한다.
    ```
    byte(1byte)  ➔  short(2byte)  ➔  int(4byte)  ➔  long(8byte)  ➔  float(4byte)  ➔  double(8byte)
                ↗
              char(2byte)
    ```
+ Java에선 **객체 형 변환**도 가능한데 **업캐스팅(Upcasting)**과 **다운캐스팅(Downcasting)**으로 나뉜다.
+ **객체 형 변환**은 상속 관계에 있는 상위 클래스와 하위 클래스 간의 형변환인데 클래스는 **reference(참조형)** 타입이니 **참조형 캐스팅**이라고도 한다.
+ 상위 클래스는 하위 클래스의 멤버를 **모두 가지고 있지만** 상위 클래스는 하위 클래스의 멤버를 **가지고 있지 않다.**
+ 사용 할 수 있는 멤버의 갯수를 **조절**하는 것이 객체 형 변환의 맹점이라 볼 수 있다.
+ 업캐스팅(Upcasting)은 **자동으로 이뤄지며** 명시적인 형 변환 없이도 사용 할 수 있다.
+ 업캐스팅이 되면 하위 클래스에만 있고 상위 클래스에는 없는 메서드는 실행이 **불가**하다.
+ 업캐스팅 후 **오버라이드** 된 메서드가 있을 경우 **하위 클래스**의 메서드가 실행된다.
    ```java
    class Animal {
        public void makeSound() {
            System.out.println("Animal sound");
        }
    }

    class Dog extends Animal {
        @Override
        public void makeSound() {
            System.out.println("Bark");
        }

        public void fetch() {
            System.out.println("Fetching");
        }
    }

    public class Main {
        public static void main(String[] args) {
            Dog dog = new Dog();
            Animal animal = dog; // Upcasting
            animal.makeSound();  // 출력: Bark
            animal.fetch();   // 컴파일 오류: Animal 타입에는 fetch() 메서드가 없음
        }
    }
    ```
+ 다운캐스팅(Downcasting)은 **명시적인 변환을 요구**하며 타입이 맞지 않을 경우 **``ClassCastException``** 오류가 발생 할 수 있다.
+ 다운캐스팅은 단순히 업캐스팅의 **반대 개념이 아니라**, 업캐스팅된 하위 클래스를 복구하여 본래 기능을 **회복**하기 위한 것이다.
    ```java
    class Animal {
        public void makeSound() {
            System.out.println("동물 소리");
        }
    }

    class Dog extends Animal {
        @Override
        public void makeSound() {
            System.out.println("멍멍");
        }

        public void fetch() {
            System.out.println("물어오기");
        }
    }

    public class Main {
        public static void main(String[] args) {
            Animal animal = new Dog(); // 업캐스팅

            try {
                Dog dog = castToDog(animal); // 다운캐스팅(기능 회복)
                dog.makeSound();  // 출력: 멍멍
                dog.fetch();  // 출력: 물어오기
            } catch (ClassCastException e) {
                System.out.println("다운캐스팅 실패: " + e.getMessage());
            }

            try {
                Animal anotherAnimal = new Animal();  // Animal 객체 생성
                Dog dog = castToDog(anotherAnimal);  // 예외 발생(ClassCastException; anotherAnimal은 Dog가 아니라 Animal을 바인딩함)
                dog.makeSound();  // 실행되지 않음
                dog.fetch();  // 실행되지 않음
            } catch (ClassCastException e) {
                System.out.println("다운캐스팅 실패: " + e.getMessage());
            }
        }

        public static Dog castToDog(Animal animal) throws ClassCastException {
            if (animal instanceof Dog) {
                return (Dog) animal;  // 다운캐스팅
            } else {
                throw new ClassCastException("Animal 객체는 Dog의 인스턴스가 아닙니다");
            }
        }
    }
    ```
+ 형 변환이 일어날 때는 큰 타입에서 작은 타입으로 갈때 손실이 발생하거나(**실수형(3.123)->정수형(3)**) 위 예제에서 처럼 처럼 업캐스팅 되지 않은 상위 객체를 다운 캐스팅 하려고 시도할 때 **오류**가 발생 할 수 있기 때문에 **주의**해야 한다.
## 다형성(Polymorphism)
+ 여러가지 **형태**를 가질 수 있는 능력을 **다형성**이라고 한다.
    ```java
    class Tv {
        boolean power;
        int channel;
        
        void power()		{ ... }
        void channelUp()	{ ... }
        void channelDown()	{ ... }
    }


    class SmartTv extends Tv {
        String text;
        void caption() { ... }
    }
    ```
+ 다음과 같은 상속 관계의 클래스들이 있을 때 ``SmartTv a = SmartTv();``처럼 참조변수와 인스턴스 타입이 **일치**하는 경우가 일반적이지만 ``Tv a = new SmartTv();`` 처럼 **불일치** 할때도 상위 타입의 참조변수로 하위 타입 객체를 다루는 것이 가능한것이 **다형성**이다.
+ 그러나 ``v a = new SmartTv();``와 같이 선언되면 이는 **업캐스팅** 된것으로 SmartTv의 ``text``,``caption()`` 멤버는 **이용할 수 없다**.
+ 다형성을 구현하고 유지하면 여러 **장점**이 있다.
    + 코드의 **유연성**과 **확장성**을 강화할 수 있다.
    + 코드의 **재사용성**을 높일 수 있다.
    + 조건문이나 기타 복잡한 구조를 피할 수 있어 코드의 **간결성**과 **가독성**이 확보가 된다.
    + **인터페이스(Interface) 기반 설계**를 촉진하여 객체 간 **의존성**을 줄이고 **유연한 아키텍쳐를 구축**할 수 있다.
    + **객체 지향 프로그래밍**의 **SOLID**를 지키는 데 도움이 된다