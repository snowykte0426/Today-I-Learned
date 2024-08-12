# Bean LifeCycle,Bean Scope
Bean(빈)의 **Life Cycle(생명주기)** 와 **Scope(범위)** 에 대해 본 문서(TIL)에서 알아본다.
## Bean 생명주기(LifeCycle)
+ Bean이 **생성**되고 **소멸**되는 주기(Cycle)을 의미한다.
    ```
    스프링컨테이너 생성
      -> 스프링 빈 생성
        -> 의존관계 주입
          -> 초기화 콜백
            -> 사용
              -> 소멸 전 콜백
                -> 스프링 종료
    ```
+ 