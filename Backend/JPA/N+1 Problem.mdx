# N + 1 문제

## N + 1 문제란?
ORM(Object Relational Mapping) 기술에서 특정 객체를 대상으로 쿼리를 수행할 때, **연관관계를 가지는 엔티티들에 대해 추가적으로 N개의 쿼리가 발생**하는 문제를 의미한다.

---

## 원인
- **RDB**와 **객체 지향 언어**의 패러다임 차이로 인해 발생한다:
  - 객체 지향 언어에서는 메모리 내에서 연관된 객체를 **Random Access**로 접근할 수 있다.
  - 하지만 RDB에서는 연관된 데이터를 가져오기 위해 추가적인 **SELECT** 쿼리를 실행해야 한다.

---

## 예시 코드

```kotlin
@Entity
import jakarta.persistence.*
@Entity
class Article : BaseEntity() {
    @OneToMany(mappedBy = "article", fetch = FetchType.LAZY, cascade = [CascadeType.REMOVE])
    var opinions: MutableList<Opinion> = mutableListOf()
}
```

위 코드에서 `Article`은 다수의 `Opinion`을 가지고 있다.
`ArticleRepository`에서 여러 `Article`을 조회할 때 `FetchType.LAZY` 설정으로 인해 프록시 객체가 생성된다. 이 컬렉션을 조회하려는 순간, RDB에서 **N개의 추가 쿼리**가 발생한다.

---

## 그래도 동작은 되지 않을까?
N+1 문제가 발생하면 다음과 같은 부작용이 생긴다:
- **쿼리 배수 증가**: RDB에 큰 부하가 발생하여 성능 저하로 이어질 수 있다.
- **사용자 경험 저하**: 실행 시간이 지나치게 길어질 수 있다.

---

## How much?
예를 들어 10개의 `Article`을 조회하는 경우:
1. 첫 번째 `SELECT`로 모든 `Article`을 조회한다.
2. 각 `Article`에 연결된 `Opinion`을 조회하기 위해 **10개의 추가적인 `SELECT`**가 실행된다.

결과적으로 **1 + 10 = 11**개의 쿼리가 실행된다.

---

## How to Fix N + 1 Problem?

### JPQL Fetch Join
상위 엔티티를 조회할 때 **`JOIN` 쿼리를 통해 한 번에 조회**하는 방법이다.

```kotlin
@Query("SELECT DISTINCT a FROM Article a JOIN FETCH a.opinions")
fun findAllWithOpinions(): List<Article>
```

#### Fetch Join과 일반 Join의 차이
- `Fetch Join`은 **ORM에서 사용을 전제**로 DB 스키마를 엔티티로 변환하며, <b>영속성 컨텍스트(Persistence Context)</b>에 **영속화**를 수행한다.
- 한 번 `Fetch Join`으로 조회한 후, 다시 엔티티를 탐색하더라도 추가적인 쿼리는 실행되지 않는다.

#### Collection 연관관계에서 Fetch Join
- `Fetch Join`을 `Collection`에 대해 사용하면 **1:N 관계로 인해 상위 엔티티가 중복**될 수 있다.

| Article    | Opinion   |
|------------|-----------|
| Article_1  | Opinion_1 |
| Article_1  | Opinion_2 |
| Article_1  | Opinion_3 |
| Article_2  | Opinion_1 |

- 이런 중복 문제를 해결하기 위해 **`DISTINCT` 절**을 사용해야 한다.

#### JPQL과 SQL에서의 `DISTINCT` 차이
- SQL의 `DISTINCT`는 각 행의 모든 칼럼이 동일한지 확인하여 중복을 제거한다.
- JPQL의 `DISTINCT`는 엔티티 객체 단위에서 중복을 검사하므로, 상위 엔티티 기준으로 중복을 제거할 수 있다.

#### 여러 Collection에 대한 Fetch Join
- 여러 `Collection`에 대해 `Fetch Join`을 사용하면 **카르테시안 곱(Cartesian Product)**이 발생하여 데이터가 기하급수적으로 증가할 수 있다.
- 이를 방지하기 위해 한 번에 하나의 `Collection`만 `Fetch Join`해야 한다.

#### Fetch Join과 페이징 금지
- JPA는 페이징을 위해 쿼리에 `LIMIT`와 `OFFSET`을 추가한다. 하지만 `Fetch Join`은 연관된 데이터를 모두 가져오기 때문에 페이징 결과가 왜곡될 수 있다.
- 예를 들어, `Article` 10개를 페이징 처리하려고 해도 연관된 데이터는 제한 없이 모두 가져온다.
- Hibernate는 모든 데이터를 로드한 후 메모리에서 페이징을 처리하므로 대량 데이터에서는 **OutOfMemoryError**와 같은 문제가 발생할 수 있다.

<details>
<summary>Fetch Join과 페이징 문제 해결 방법</summary>

##### (1) 서브쿼리 활용
- 상위 엔티티를 먼저 페이징 처리하고, 연관된 데이터를 조인하는 방식이다.
```kotlin
@Query("""
    SELECT a
    FROM Article a
    WHERE a.id IN (
        SELECT a2.id
        FROM Article a2
        ORDER BY a2.createdAt DESC
    )
""")
fun findPagedArticles(): List<Article>
```

##### (2) DTO 매핑
- 엔티티 대신 필요한 데이터를 DTO로 매핑하여 조회한다.
```kotlin
@Query("""
    SELECT new com.example.dto.ArticleDto(a.id, a.title, o.content)
    FROM Article a
    JOIN a.opinions o
    WHERE a.createdAt > :date
""")
fun findArticleDtos(pageable: Pageable): Page<ArticleDto>
```

##### (3) Batch Fetching
- `@BatchSize` 또는 전역 설정으로 지연 로딩 데이터를 배치로 가져오는 방식이다.
```kotlin
@OneToMany(mappedBy = "article")
@BatchSize(size = 10)
var opinions: List<Opinion> = mutableListOf()
```

</details>

---

### Batch Size
지연 로딩 시 프록시 객체를 조회할 때 **`WHERE IN` 문으로 한 번에 조회**하도록 설정하는 방법이다.

#### 설정 방법
- `application.yml`에서 전역적으로 설정할 수 있다.

```yaml
spring:
    jpa:
        properties:
            default_batch_fetch_size: 100
```

- 또는 연관 관계마다 `@BatchSize`를 설정할 수 있다.

```kotlin
@OneToMany(mappedBy = "article")
@BatchSize(size = 10)
var opinions: List<Opinion> = mutableListOf()
```

#### 주의사항
- Batch Size가 너무 크면 데이터베이스 부하를 초래할 수 있으므로 일반적으로 10~100 사이의 값을 설정한다.

---

### Entity Graph
`@EntityGraph`는 지연 로딩을 즉시 로딩으로 바꾸는 방법이다.

#### 코드 예제

```kotlin
@Entity
class Article(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,

    val title: String,

    @OneToMany(mappedBy = "article", fetch = FetchType.LAZY, cascade = [CascadeType.ALL])
    val opinions: MutableList<Opinion> = mutableListOf()
)

@Entity
class Opinion(
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    val id: Long = 0,

    val content: String,

    @ManyToOne(fetch = FetchType.LAZY)
    @JoinColumn(name = "article_id")
    val article: Article
)

@Repository
interface ArticleRepository : JpaRepository<Article, Long> {
    @EntityGraph(attributePaths = ["opinions"])
    fun findAllWithOpinions(): List<Article>

    @EntityGraph(attributePaths = ["opinions"])
    fun findByIdWithOpinions(id: Long): Article?
}
```

---

## 요약 및 결론
N+1 문제는 JPA와 ORM 사용 시 성능 저하의 주요 원인이 될 수 있다. 이를 해결하기 위해 JPQL Fetch Join, Batch Size, Entity Graph와 같은 다양한 방법을 사용할 수 있다.

Fetch Join은 강력한 도구이지만, 여러 컬렉션에 대해 적용하거나 페이징과 함께 사용할 경우 문제가 발생할 수 있다. 따라서 상황에 맞는 최적화 방법을 선택하고, 데이터베이스와 JPA의 동작 방식을 깊이 이해하는 것이 중요하다.