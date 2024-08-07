# Kotlin 공부
+ **when문**
     ```kotlin
        fun numberDescription(n: Int, max: Int = 100): String = when (n) {
            0 -> "Zero"
            1, 2, 3 -> "Small"
            in 4..9 -> "Medium"
            in 10..max -> "Large"
            !in Int.MIN_VALUE until 0 -> "Negative"
            else -> "Huge"
        }
    ```
    ``when``문은 ``switch``문과 비슷한 문법으로 ``if``문과 같이 식으로 사용할 수 있고 Java의 ``switch``문과 다르게 **폴 스루(fall-through)**의 의미가 없기 때문에 ``break``문을 사용하지 않아도 됨
+ **while문**
     ```kotlin
        import kotlin.random.*

        fun main() {
            val num = Random.nextInt(1, 101)
            var guess = 0

            while (guess != num) {
                guess = readLine()!!.toInt()
                if (guess < num) println("Too small")
                else if (guess > num) println("Too big)
            }
            println("Right: it's $num")
        }
    ```




    다른 언어의 ``while``문 처럼 조건식이 참인 동안 동작하는 반복문.루프(loop)의 본문을 실행하기 **전에** 조건식의 참 여부를 검사한다는 점이 특징임
+ **do-while문**
    ```kotlin
    fun main() {
        var sum = 0
        var num

        do {
            num = readLine()!!.toInt()
            sum += num
        } while (num != 0)

        println("Sum: $sum")
    }
    ```
    ``do``와 ``while``사이에 있는 코드를 실행하며 조건식이 참이 아니더라도 1회 실행함,조건식의 참 여부를 본문 실행 **후** 검사한다는 점이 특징임
+ **for문**
    ```kotlin
    fun main() {
        val a = IntArray(10) { it * it }
        var sum = 0

        for (x in a) {
            sum += x
        }

        printl("Sum: $sum")
    }
    ```
    다른 언어와 비슷하게 동작하며 여러 값이 들어있을 수 있는 값에 대한 루프를 수행 가능
+ **내포된 루프와 레이블**
    ```kotlin
    fun index0f(subarray: IntArray, array: IntArray): Int {
        outerLoop@ for (i in array.indices) {
            if (subarray[j] != array[i + j]) continnue@outerLoop
        }
        return i
    }
    ```
    ``contionue``문을 바깥쪽 루프에 적용 가능,역시 ``break``문도 같은 방식으로 사용가능함
+ **꼬리 재귀 함수**
    ```kotlin
    tailrec fun binIndex0f(
        x: Int,
        array: IntArray,
        from: Int = 0,
        to: Int = array.size
    ): Int {
        if (from == to) return -1
        val midIndex = (from + to -1) / 2
        val mid = array[midIndex]
        return when {
            mid < x -> binIndex0f(x, array, midIndex + 1, to)
            mid > x -> binIndex0f(x , array, from, midIndex)
        }
    }
    ```
    코드를 깔끔하게 보여주지만 비재귀 함수와 비교해보면 성능 차원에서 약간의 부가 비용이 발생할 수 있고 **Stack Overflow**가 발생할 가능성이 있음,그러나 코틀린에선 ``tailrec`` 키워드를 붙이면 재귀 함수를 비재귀 함수로 변환해줌