# N + 1 문제
### N + 1 문제란?
+ ORM(Object Relational Mapping) 기술에서 특정 객체를 대상으로 하여 수행한 쿼리가 **가지고 있던 연관관계**또한 조회되며 **N번**의 추가적인 쿼리가 발생하는 문제이다.
### 원인..?
+ 근본적으로는 **RDB**와 **객체 지향 언어**와의 패러다임 차이로 인해 발생하며 객체의 연관관계는 메모리 내에서 **Random Access**를 통하여 접근가능하지만, RDB에서는 **SELETE** 문을 이용하여 조회해야 하기 때문에 발생한다.
### 예시!
```kotlin
@Entity
import jakarta.persistence.*
@Entity
class Article : BaseEntity() {
    @OneToMany(mappedBy = "article", fetch = FetchType.LAZY, cascade = [CascadeType.REMOVE])
    var opinions: MutableList<Opinion> = mutableListOf()
}
```
위 코드에서 ``Article``은 <b>다수의 ``Opinion``</b>을 가지고 있다.``ArticleRepository``에서 여러 Article을 조회할 시 <b>``FatchType.LAZY``</b> 설정으로 인하여 프록시 객체가 생성되고 해당 컬렉션을 코드에서 조회하려는 순간 RDB에 **N개의 SQL 쿼리가 발생**하게 된다.
### 그래도 동작은 되잔아?
+ N + 1 문제가 발생하면 쿼리가 **배수적으로 동작**하며 RDB에 큰 부담이 가해지고 이는 직접적인 장애요인으로 작용할 수 있다.
+ 사용자 관점에서도 **수행 시간**이 지나치게 길어 보일수도 있다.
### How much?
+ 예를 들어 10개의 ``Article``을 조회할 경우
    + 첫번째 ``SELETE``로 모든 ``Article``을 조회함
    + 각 ``Article``에 연결된 ``Opinion``을 조회하기 위하여 **추가적인 ``SELETE``가 실행**됨

   결론적으로 **1 + 10 = 11**개의 쿼리가 실행된다.
### How to Fix N + 1 Problem?
#### JPQL Fetch Joun
+ 상위 Entity를 조회할 때 **``JOIN`` 쿼리를 통하여 한번에 조회**하는 방법이다.
```kotlin
@Query("SELECT DISTINCT a FROM Article a JOIN FETCH a.opinions")
fun findAllWithOpinions(): List<Article>
```