# Spring Data Jpa

* [JPA](../java/jpa.md)
* https://spring.io/projects/spring-data-jpa

> Spring Data JPA를 사용하면 JPA 기반(Java Persistence API) 리포지토리를 쉽게 구현할 수 있습니다.
>
Spring에서 JPA를 사용할 때는 구현체들(Hibernate...)을 직접 다루진 않는다.

애플리케이션의 데이터 액세스 계층을 구현하는 것은 꽤 오랫동안 성가신 일이었다. 간단한 쿼리를 실행하고 pagination 및 auditing을 수행하려면 너무 많은 boilerplate 코드를 작성해야 한다.
Spring Data JPA는 실제로 필요한 양으로 노력을 줄여 데이터 액세스 계층의 구현을 크게 개선하는 것을 목표로 만들어졌다.

특징:

* Spring 및 JPA기반의 Repo 구축을 위한 정교한 지원
* Querydsl 쿼리 지원 및 이에 따른 안전한 JPA 쿼리 처리
* 도메인 클래스의 투명한 감사
* pagination 지원, 동적 쿼리 실행, 맞춤형 데이터 액세스 코드 통합 기능
* @Query가 명시된 쿼리는 부트스트랩 시간에 유효성 검사
* XML 기반 엔티티 매핑 지원
* @EnableJpaRepositories을 지원하여 JavaConfig기반 저장소 구성

---

### Hiberante vs Spring Data JPA

[Hibernate](hibernate.md)와 Spring Data JPA는 큰 차이가 없는데 왜 굳이 Spring Data JPA를 사용해야 할까?

1. 구현체 교체 용이성

   Hibernate외에 다른 구현체로 쉽게 교체하기 위함이다. Hibernate에 대한 의존성을 낮추고 만약 새롭게 좋은 구현체가 나온다면 Spring Data JPA는 해당 구현체로 쉽게 교체할 수 있다.

   실제 예시로는 자바 Redis 클라이언트가 Jedis에서 Lettuce로 대세가 넘어갈 때 Spring Data Redis를 사용하던 사람들은 쉽게 교체를 했다.


2. 저장소 교체 용이성

   위와 마찬가지로 의존성을 낮추고 관계형 DB 외에 다른 저장소로 쉽게 교체하기 위함이다. 서비스 초기에는 관계형 DB로 모든 기능을 처리했지만, 점점 트래픽이 많아져 관계형 DB로는 도저히 감당이 안될 때가 올
   수도 있다. 이때 MongoDB로 교체가 필요하다면 개발자는 Spring Data JPA에서 Spring Data MongoDB로 의존성만 교체하면 된다.

→ Spring Data의 하위 프로젝트들(Spring Data JPA, Spring Data Redis, Spring Data MongoDB 등등)은 기본적인 CRUD의 인터페이스(save(), findAll,
findOne() 등)가 같다. 그러다 보니 저장소가 교체되어도 기본적인 기능은 변경할 것이 없기 때문에 Spring팀에서는 Hibernate를 직접 쓰는 것보다 Spring Data 프로젝트를 권장한다.

---

Spring Initializr에서 Dependencies를 추가할 때 Spring Data JPA를 넣어주자.

> [Spring Initializr](https://start.spring.io/)
>

의존성 목록:

- Spring Boot DevTools
- Spring Web
- Spring Data JPA
- PostgreSQL Driver

`src/main/resources/application.yml` 파일을 작성.

```yml
spring:
  datasource:
  url: jdbc:postgresql://localhost:5432/postgres
  username: postgres
  password: password
```

아까 작성한 Person과 Item 클래스를 챙겨오면 준비는 끝이다.


---

### Repository

> [Spring Data JPA 문서](https://docs.spring.io/spring-data/jpa/docs/current/reference/html/)
>

> [조영호님의 DAO와 Repository의 차이에 대한 설명](http://aeternum.egloos.com/1160846)
>

Person을 관리하는 PersonRepository를 만든다.

DAO가 Data Access 관점에 무게를 두고 있다면, Repository는 객체를 관리하는 Collection에 가까운 느낌으로 사용하게 된다.
EntityManager가 Persistence Context를 사용하기 때문에 가능한 일이다. JPA의 Repository가 DDD의 Repository와 일치하는 건 아니지만, 가능하면 그런 느낌으로 도메인
객체를 구성하고 사용해 보자.

<details><summary>JPA의 Repository / DDD의 Repository</summary>

`JPA Repository`와 [DDD (Domain-Driven Design)](../architecture/domainModelPattern.md)의 `Repository`는 데이터 액세스를 다루는 두 가지
다른 개념이며, 아래는 이들 간의 주요 차이를 설명합니다.

### 차이점:

- **패턴의 관점:**
    - `JPA Repository`는 주로 기술적인 측면에서의 패턴으로, 데이터베이스 액세스를 간소화하는 데 중점을 둡니다.
    - `DDD의 Repository`는 도메인 주도 설계에서의 개념으로, 도메인 객체와의 상호 작용에 중점을 둡니다.

- **목적:**
    - `JPA Repository`는 데이터베이스 액세스를 편리하게 만들기 위해 사용됩니다.
    - `DDD의 Repository`는 도메인 레이어를 데이터베이스와 분리하여 도메인 객체에 집중할 수 있도록 하는 데 사용됩니다.

- **포괄성:**
    - `JPA Repository`는 주로 데이터베이스 액세스와 관련된 작업을 다룹니다.
    - `DDD의 Repository`는 도메인 모델에서의 도메인 객체와의 상호 작용을 다룹니다.

두 Repository의 개념은 서로 다른 관점에서의 데이터 액세스를 나타내며, 애플리케이션의 요구 사항과 아키텍처에 따라 혼용되기도 합니다.

</details>

```java
public interface PersonRepository extends CrudRepository<Person, String> {
    List<Person> findAll();

    Optional<Person> findByName(String name);

    Person save(Person person);

    void delete(Person person);
}
```

이렇게 인터페이스만 만들어 주면 끝이다. 전형적인 Collection인 List나 JPA의 EntityManager에 update가 없는 것처럼, PersonRepository에도 update가 필요하지 않다.
Query를 위한 find 메서드는 findByXXX 형태로 써주기만 해도 된다.
---

### Transactional 애너테이션

> [Layered Architecture](https://wikibook.co.kr/article/layered-architecture/)
>

트랜잭션 범위는 Spring AOP를 이용한 @Transactional 애너테이션으로 지정할 수 있다. 일반적으로는 Application Layer가 곧 트랜잭션 범위가 된다.

Application Layer에 CreateItemService를 추가한다면 다음과 같이 써줄 수 있다.

```java

@Service
@Transactional
public class CreateItemService {
    private PersonRepository personRepository;

    public CreateItemService(PersonRepository personRepository) {
        this.personRepository = personRepository;
    }

    void createItem(String personName, String name, String usage) {
        Person person = personRepository.findByName(personName)
                .orElseThrow(() -> new PersonNotFound(personName));

        person.addItem(name, usage);
    }
}
```

### Transactional 테스트

> [토비님의 @Transactional 테스트의 실용성에 대한 설명](https://www.inflearn.com/questions/792383)
>

---

- [[SpringCamp2013] Spring Data JPA 영상 - 김영한님](https://www.youtube.com/watch?v=OOO4H3BAetU) / [발표자료](https://www.slideshare.net/zipkyh/spring-datajpa)