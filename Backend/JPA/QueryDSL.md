# QueryDSL
## QueryDSL이란?
QueryDSL은 Java 기반의 ORM 프레임워크인 JPA(Java Persistence API)에서 복잡한 쿼리를 간편하고 타입 안전하게 작성할 수 있도록 도와주는 도구이다. 보통 JPA를 사용할 때 문자열 기반의 JPQL을 사용하게 되는데, 이 방식은 런타임 오류를 유발할 가능성이 있다. QueryDSL은 이를 해결하기 위해 컴파일 타임에 오류를 감지할 수 있는 환경을 제공한다. 또한, 코드의 가독성을 높이고 유지보수를 용이하게 한다.

---

## QueryDSL의 주요 특징
- **타입 안전성**: 쿼리를 작성하는 과정에서 데이터베이스 스키마와 매핑된 Q 클래스들을 활용하여 타입 검사를 할 수 있다.
- **코드 가독성**: 문자열 기반 쿼리가 아닌 메서드 체이닝 방식을 사용해 직관적으로 쿼리를 작성할 수 있다.
- **호환성**: JPA뿐만 아니라 SQL, MongoDB, Elasticsearch 등 다양한 데이터 소스와 연동할 수 있다.

---

## QueryDSL의 기본 사용법

### 1. Gradle이나 Maven에 의존성 추가하기
QueryDSL을 사용하려면 프로젝트에 필요한 의존성을 추가해야 한다. 예를 들어 Gradle을 사용하는 경우, 다음과 같이 build.gradle 파일에 설정한다.

```groovy
dependencies {
    implementation 'com.querydsl:querydsl-jpa:5.0.0'
    annotationProcessor 'com.querydsl:querydsl-apt:5.0.0:jpa'
}
```

### 2. Q 클래스 생성하기
QueryDSL은 데이터베이스 엔티티와 매핑된 Q 클래스를 자동으로 생성한다. 이를 위해 annotationProcessor 플러그인을 통해 엔티티 클래스를 처리해야 한다. Q 클래스는 보통 `target/generated-sources` 디렉토리에 생성된다.

### 3. 쿼리 작성하기
다음은 QueryDSL을 사용해 간단한 쿼리를 작성하는 예제이다.

```java
QMember member = QMember.member;

JPAQueryFactory queryFactory = new JPAQueryFactory(entityManager);
List<Member> members = queryFactory.selectFrom(member)
    .where(member.age.gt(18))
    .orderBy(member.name.asc())
    .fetch();
```

위 코드는 나이가 18세 이상인 멤버들을 이름순으로 정렬하여 가져오는 쿼리를 작성한 예이다.

---

## QueryDSL로 복잡한 쿼리 작성하기

QueryDSL은 복잡한 조건문이나 조인도 간단히 처리할 수 있다. 예를 들어, 다중 테이블 조인을 사용하는 경우 다음과 같이 작성할 수 있다.

```java
QMember member = QMember.member;
QTeam team = QTeam.team;

List<Tuple> result = queryFactory
    .select(member.name, team.name)
    .from(member)
    .join(member.team, team)
    .where(team.name.eq("Development"))
    .fetch();
```

위 코드는 멤버와 팀 테이블을 조인하고, 팀 이름이 "Development"인 멤버들의 이름과 팀 이름을 가져오는 쿼리이다.

---

## QueryDSL을 사용할 때 주의할 점

### 1. Q 클래스 관리
엔티티 변경 시 Q 클래스를 항상 재생성해야 한다. 이를 자동화하기 위해 빌드 스크립트에 적절한 설정을 추가하는 것이 좋다.

### 2. 성능 고려
QueryDSL로 작성된 쿼리는 JPA와 동일한 성능 특성을 가진다. 따라서 복잡한 쿼리일수록 데이터베이스 성능을 고려해야 한다.

### 3. 테스트 환경 설정
QueryDSL은 엔티티 매핑이 올바르지 않으면 정상적으로 동작하지 않는다. 테스트 환경에서 항상 정확한 스키마 매핑을 확인해야 한다.

---

## How to Optimize QueryDSL Usage?

### JPQL Fetch Join 활용
QueryDSL과 JPA를 함께 사용할 때, Fetch Join을 통해 N+1 문제를 방지할 수 있다.

#### 예시 코드
```java
QArticle article = QArticle.article;
QOpinion opinion = QOpinion.opinion;

List<Article> articles = queryFactory
    .selectFrom(article)
    .join(article.opinions, opinion).fetchJoin()
    .fetch();
```

#### Fetch Join 주의점
- 컬렉션 관계에서 Fetch Join을 사용할 때는 중복 데이터가 발생할 수 있으므로, `DISTINCT`를 사용하는 것이 좋다.

```java
List<Article> articles = queryFactory
    .selectDistinct(article)
    .from(article)
    .join(article.opinions, opinion).fetchJoin()
    .fetch();
```

---

## Batch Fetching

Batch Fetching은 지연 로딩으로 인해 발생하는 다중 쿼리를 한 번에 처리하는 기술이다.

#### 설정 방법
- `@BatchSize` 어노테이션을 엔티티에 추가하거나, 글로벌 설정으로 적용할 수 있다.

```yaml
spring:
    jpa:
        properties:
            default_batch_fetch_size: 100
```

```java
@OneToMany(mappedBy = "article")
@BatchSize(size = 10)
List<Opinion> opinions;
```

---

## 요약 및 결론
QueryDSL은 JPA를 사용하는 개발자에게 매우 유용한 도구이다. 이를 통해 타입 안전하고 가독성 높은 쿼리를 작성할 수 있으며, 특히 복잡한 쿼리를 효율적으로 처리할 수 있다. Fetch Join, Batch Fetching과 같은 기법을 적절히 활용하여 성능 문제를 사전에 방지하자.

