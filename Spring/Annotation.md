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
    + 어노테이션의 **유지 정책**을 **설정**한다.
        ```java
        @Retention(RetentionPolicy.RUNTIME)
        public @interface CustomAnnotation {   }
        ```