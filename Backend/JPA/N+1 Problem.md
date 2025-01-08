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
+ **Fetch Join과 일반 Join의 차이**
    + 근본적으론 ``Fetch Join``은 **ORM에서 사용을 전제**로 DB 스키마를  Entity로 변환해주고 <b>영속성 컨텍스트(Persistence Context)</b>에 **영속화**를 수행해준다는 차이점이 있다
    + 위와 같은 이유로 한번 ``Fetch Join``을 이용하여 조회한 후 다시 Entity Graph를 탐색하더라도 조회하는 쿼리는 수행되지 않는다.
+ **Collection 연관관계에서 Fetch Join**
    + ``Fetch Join``을 ``Collection``에 대해서 할 경우 <b>``1:n``</b> 관계이기 때문에 상위 칼럼은 중복되어 조회될 수 있다.

    |Article|Opinion|
    |---|---|
    |Article_1|Opinion_1|
    |Article_1|Opinion_2|
    |Article_1|Opinion_3|
    |Article_2|Opinion_1|
    + 이런 데이터 중복 문제를 해결하기 위하여 <b>**Distinct**</b> 절을 사용하여야 한다.

<h4><span>★</span> <b>JPQL과 SQL에서의 <code>Distinct</code>절 차이</b></h4>
<blockquote>
    <ul>
        <li>SQL에서의 <code>Distinct</code>은 RDB에서 수행되며 <code>JOIN</code>되여 발생한 데이터에서 각 행이 일치하는지만 확인하여 중복을 검사한다</li>
        <li>이 경우 <code>Article</code>은 겹칠 수 있지만 <code>Opinion</code>은 겹치지 않기 때문에 중복이 걸러지지 않을 수 있다</li>
        <li>JPQL의 경우 Entity 객체 상에서 검사를 수행하기에 중복 데이터를 제거할 수 있다</li>
    </ul>
</blockquote>