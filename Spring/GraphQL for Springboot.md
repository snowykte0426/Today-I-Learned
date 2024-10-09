# GraphQL과 Spring Boot로 간단한 백엔드 구축하기

## GraphQL 소개

GraphQL은 Facebook에서 만든 데이터 쿼리 언어로, 클라이언트가 필요한 데이터만 요청하고 받을 수 있게 해 줍니다. REST API의 단점을 해결하기 위해 만들어졌으며, REST에서 발생하는 불필요한 데이터 전송이나 여러 엔드포인트 요청 문제를 해결하는 데 효과적입니다. GraphQL을 사용하면 단일 엔드포인트로 다양한 요청을 처리할 수 있어 유연성이 높아집니다.

## Spring Boot에서 GraphQL 사용하기

Spring Boot와 GraphQL을 함께 사용하면 강력한 백엔드 서비스를 쉽게 만들 수 있습니다. 이 글에서는 Java 언어를 이용해 Spring Boot 환경에서 GraphQL을 구현하는 방법을 예제와 함께 설명합니다.

## 환경 설정

### Gradle 의존성 추가

먼저, GraphQL을 사용하기 위해 필요한 라이브러리를 `build.gradle` 파일에 추가해야 합니다. 주요 의존성으로는 `graphql-spring-boot-starter`와 `graphql-java-tools`가 있습니다.

```groovy
// Gradle 의존성 예시 (Kotlin DSL)
dependencies {
    implementation("com.graphql-java-kickstart:graphql-spring-boot-starter:11.1.0")
    implementation("com.graphql-java-kickstart:graphql-java-tools:11.1.0")
}
```

### 스키마 파일 만들기

GraphQL은 스키마 정의 언어(SDL)를 사용해 데이터를 정의합니다. `src/main/resources/graphql` 폴더에 `.graphqls` 파일을 만들어 스키마를 정의할 수 있습니다. 예를 들어, `book.graphqls`라는 파일에 다음과 같은 스키마를 작성합니다:

```graphql
type Query {
    bookById(id: ID!): Book
}

type Book {
    id: ID!
    title: String
    author: String
}
```

## GraphQL 서비스 구현

### 데이터 모델 만들기

먼저 `Book`이라는 데이터를 나타내는 Java 클래스를 만듭니다.

```java
public class Book {
    private String id;
    private String title;
    private String author;

    // 생성자, getter, setter
    public Book(String id, String title, String author) {
        this.id = id;
        this.title = title;
        this.author = author;
    }

    public String getId() { return id; }
    public String getTitle() { return title; }
    public String getAuthor() { return author; }
}
```

### 서비스 클래스 작성

데이터를 제공할 `BookService` 클래스를 작성합니다.

```java
import org.springframework.stereotype.Service;
import java.util.HashMap;
import java.util.Map;

@Service
public class BookService {
    private static Map<String, Book> bookRepo = new HashMap<>();

    static {
        bookRepo.put("1", new Book("1", "GraphQL Tutorial", "John Doe"));
        bookRepo.put("2", new Book("2", "Spring Boot with GraphQL", "Jane Smith"));
    }

    public Book getBookById(String id) {
        return bookRepo.get(id);
    }
}
```

### GraphQL Resolver 작성

GraphQL 쿼리를 처리하기 위한 `BookResolver` 클래스를 작성합니다.

```java
import com.coxautodev.graphql.tools.GraphQLQueryResolver;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

@Component
public class BookResolver implements GraphQLQueryResolver {
    @Autowired
    private BookService bookService;

    public Book bookById(String id) {
        return bookService.getBookById(id);
    }
}
```

## 테스트와 실행

이제 서버를 실행하고 GraphQL 엔드포인트로 접근할 수 있습니다. 기본적으로 `/graphql` 엔드포인트를 통해 GraphQL 쿼리를 테스트할 수 있습니다. 예를 들어, `bookById` 쿼리를 실행하여 특정 책의 정보를 얻을 수 있습니다:

```graphql
query {
    bookById(id: "1") {
        title
        author
    }
}
```

이 쿼리는 책의 제목과 저자를 반환합니다.

## 결론

Spring Boot와 GraphQL을 사용하면 REST보다 더 유연하고 직관적으로 데이터를 요청할 수 있습니다. 클라이언트는 필요한 데이터만 정확하게 요청할 수 있어 불필요한 데이터 전송을 줄일 수 있으며, 여러 요구 사항을 단일 엔드포인트에서 처리할 수 있는 장점이 있습니다.