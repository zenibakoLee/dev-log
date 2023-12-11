# JPA

### Java(Jakarta) Persistence API

JPA란 자바 ORM 기술에 대한 API 표준 명세를 의미한다.

JPA는 ORM을 사용하기 위한 인터페이스를 모아둔 것이며, JPA를 사용하기 위해서는 JPA를 구현한 Hibernate, EclipseLink, DataNucleus같은 ORM 프레임워크를 사용해야 한다.

![](https://yozm.wishket.com/media/news/2160/3.png)

Jakarta: 오라클에서 이클립스 재단으로 이관된 JavaEE 프로젝트. Java 상표권은 제외라서 Jakarta EE로 명명. 패키지명은 javax.* 에서 Jakarta.*로.

Spring Framework 내에서는 Spring Data JPA라는 모듈을 통해 Jakarta Persistence API를 사용할 수 있습니다. Spring Data JPA는 Jakarta Persistence
API를 기반으로 하며, 스프링 애플리케이션에서 데이터베이스와의 상호 작용을 더 쉽게 구현할 수 있도록 도와줍니다.

스프링 부트 프로젝트를 생성할 때 spring-boot-starter-data-jpa라는 의존성을 추가하면 Jakarta Persistence API와 Spring Data JPA 관련 라이브러리들이 함께
포함됩니다. 이 스타터를 추가하면 데이터베이스와의 통합, JPA 엔터티 매핑, 레포지토리 작성 등을 쉽게 수행할 수 있습니다.

JPA 주요 컨셉
> A Cache is a copy of data, copy meaning pulled from but living outside the database.
>
>Flushing a Cache is the act of putting modified data back into the database.
>
>A PersistenceContext is essentially a Cache. It also tends to have it's own non-shared database connection.
>
>An EntityManager represents a PersistenceContext (and therefore a Cache)
>
>An EntityManagerFactory creates an EntityManager (and therefore a PersistenceContext/Cache)
>
>-https://tomee.apache.org/jpa-concepts.html
>

### 엔티티 매니저:

EntityManagerFactory를 통해 만들어지며 엔티티를 관리해 데이터베이스와 어플리케이션 사이에서 객체를 관리.
각 요청에 개별적인 엔티티매니저가 담당함. 스프링부트는 내부에서 팩토리를 하나만 생성해서 관리, @PersistenceContext 또는 @Autowired를 통해 주입

```java
@PersistenceContext
EntityManager em; // 프록시 엔티티 매니저. 필요할 떄 진짜 매니저 호출.
```

쉽게 말해, 앤티티매니저는 Spring Data JPA에서 관리하므로, 직접 생성하거나 관리할 필요가 없다.

### Persistence(영속성) Context

엔티티매니저는 엔티티를 Persistence context에 저장한다.
엔티티를 관리하는 가상의 공간. => 데이터베이스에서 효과적으로 데이터 로드 및 엔티티를 편하게 사용한다.

- 1차 캐시
    - 영속성 컨텍스트 내부에 1차 캐시 존재. @Id 어노테이션 필드, 엔티티의 키벨류 페어로 되어 있으며, 엔티티를 조회하면 1차 캐시에서 데이터를 조회하고 값이 있으면 반환, 없으면 DB조회 후 1차캐시
      저장 후 반환.
- 쓰기 지연(Transaction write-behind)
    - 트랜잭션 커밋 전까지 쿼리를 모아서 한번에 실행. => DB시스템 부담 감소
- 변경 감지
    - 트랜잭션 커밋시 1차 캐시에 저장되어 있는 엔티티 값과 현재 엔티티 값을 비교, 변경 존재시 변경사항을 데이터베이스에 자동 반영.
- 지연 로딩(lazy loading)
    - 필요할 때 쿼리를 날려 데이터를 조회.

해당 4가지 Persistence context의 특징은 DB접근을 최소화해 성능을 높인다.

---

### JPA Entity

JPA는 Entity라는 단위를 활용하는데, JPA에 익숙해지면 DB 세계(특히 ERM)의 Entity와 OOP 세계(특히 DDD)의 Entity를 절묘하게 결합하는데 활용할 수 있다.

DB 세계와 관련: “Entities represent persistent data **stored in a relational database** automatically using container-managed
persistence.”

OOP 세계와 관련: “An entity models a **business entity** or **multiple actions** within a single business process.”

전자에만 집중하면 단순히 SQL문을 자동으로 생성해주는 도구로 쓰게 되고, 이렇게 하면 간단한 CRUD 프로그램을 만들 때는 생산성 향상에 도움이 된다. 하지만 후자를 지속적으로 관리하지 않으면, 어느샌가 관계형
모델도, 객체지향도 만족시키지 못 하는 애매한 DB에 끌려가는 프로그램이 되기 십상이다.

JPA는 Relationship Mapping과 Aggregate Mapping 등을 지원하고, 이를 잘 활용함으로써 객체(Object)와 관계(Relational)를 적절히 조화시킬 수 있다.

### 엔티티의 상태

엔티티는 4가지 상태를 갖는다.

1. Detached(분리): Persistence Context가 관리하고 있지 않음
2. Managed(관리): Persistence Context에 의해 관리되고 있음
3. Transient(비영속): Persistence Context와 관계가 없음
4. Removed(삭제): 삭제된 상태.

```java
public class EntityManagerTest {
    @Autowired
    EntityManager em;

    public void example() {
        //Transient(비영속)
        Member member = new Member(1L, "홍길동");
        //Managed
        em.persist(member);
        //Detached
        em.detach(member);
        //Removed
        em.remove(member);
    }

}
```

---

### @OneToMany 어노테이션:

`@OneToMany` 어노테이션은 JPA(Java Persistence API)에서 관계형 데이터베이스의 일대다(1:N) 관계를 매핑할 때 사용됩니다. 이 어노테이션을 사용하면 엔터티 클래스 간의 일대다 관계를
표현할 수 있습니다.

#### 사용 예제:

```java

@Entity
public class ParentEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany
    private List<ChildEntity> children;

    // ... 다른 속성과 메서드들 ...
}

@Entity
public class ChildEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // ... 다른 속성과 메서드들 ...
}
```

위의 예제에서 `ParentEntity`와 `ChildEntity` 간에 일대다 관계를 설정하고 있습니다. `@OneToMany` 어노테이션을 사용하면 부모 엔터티(`ParentEntity`)에서 자식
엔터티(`ChildEntity`)의 목록을 관리할 수 있습니다.

### @JoinColumn 어노테이션:

`@JoinColumn` 어노테이션은 외래 키(Foreign Key)를 매핑할 때 사용됩니다. `@OneToMany`와 함께 사용될 때는 일대다 관계에서 다쪽(다(N) 쪽) 엔터티가 외래 키를 관리하는 방법을
지정합니다.

#### 사용 예제:

```java

@Entity
public class ParentEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    @OneToMany
    @JoinColumn(name = "parent_id") // 외래 키 설정
    private List<ChildEntity> children;

    // ... 다른 속성과 메서드들 ...
}

@Entity
public class ChildEntity {

    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;

    // ... 다른 속성과 메서드들 ...
}
```

위의 예제에서 `@JoinColumn` 어노테이션을 사용하여 `ParentEntity`의 `children` 목록에 대한 외래 키를 "parent_id"로 설정하고 있습니다. 이 외래 키는 데이터베이스 테이블에
매핑됩니다.

요약하면, `@OneToMany`는 일대다 관계를 표현하고, `@JoinColumn`은 외래 키를 매핑하는 데 사용됩니다. 이 두 어노테이션을 함께 사용하면 부모-자식 간의 관계를 명시적으로 정의할 수 있습니다.

---
출처

- JPA: https://yozm.wishket.com/magazine/detail/2160/
- Jakarta EE: https://www.samsungsds.com/kr/insights/java_jakarta.html