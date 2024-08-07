# Kotlin 공부
+ **변수 선언 방법**
     ```kotlin
        val key = 1:Int
     ```
    + val은 **value**에서 유래한 말로 상수를 선언하는 것이다(Java에서 ``final``과 같은 역할)
    ```kotlin
       var key = 1: Int
     ```
    + var은 가변 변수로 한번 할당되면 변경 불가능한 ``val``과 다르게 변경이 가능하다
+ **자료형**
    |이름|크기|범위|Java|
    |---|---|---|---|
    |Byte|1|-128~127|Byte|
    |Short|2|-32768~32767|Short|
    |Int|4|-2³¹~2³¹-1|Int|
    |Long|8|-2⁶³~2⁶³-1|Long|
+ **비트 연산**
    + 오른쪽 시프트 **(Shift)** 연산(위 코드는 오른쪽으로 2만큼 시프트 연산)
     ```kotlin
        13 shl 2
    ```
    + 왼쪽 시프트 **(Shift)** 연산(위 코드는 왼쪽으로 2만큼 시프트 연산)
    ```kotlin
        13 shr 2
    ```
    + 부호 없는 오른쪽 시프트
    ```kotlin
        13 ushr 2
    ```
    + 비트 곱 **(AND)**
    ```kotlin
        13 and 19
    ```
    + 비트 합 **(OR)**
    ```kotlin
        13 or 19
    ```
    + 비트 배타합 **(XOR)**
    ```kotlin
        13 xor 19
    ```
    + 비트 반전 **(Inversion, NOT)**
    ```kotlin
        13.inv()
    ```