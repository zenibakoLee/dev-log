# Hibernate

> Hibernate는 JPA 인터페이스를 구현한 구현체이자, 자바용 ORM Framework이다. => 자바 객체를 통해 DB종류 상관 없이 데이터베이스를 자바 객체 단위에서 관리할 수 있도록 한다. 내부적으로는 JDBC API를 사용한다.

Java 개발자는 일반적 으로 SQL 문을 작성하는 것보다 Java 클래스를 작성하는 데 더 익숙합니다. 따라서 많은 (그린필드) 프로젝트가 Java 우선 접근 방식으로 작성됩니다. 즉, 해당 데이터베이스 테이블을 생성하기 전에 Java 클래스를 생성해야 한다는 의미이다.

이는 자연스럽게 객체 관계형 매핑 질문으로 이어진다. 새로 작성된 Java 클래스에서 (아직 생성되지 않은) 데이터베이스 테이블로 어떻게 매핑합니까? 그리고 해당 Java 클래스 에서 데이터베이스를 생성할 수도 있나요 ? 적어도 처음에는?

Hibernate가 잘하는 것을 요약하려는 시도는 다음과 같다:

1. 초기 매핑과 별도로 많은 작업을 수행하지 않고도 데이터베이스 테이블과 Java 클래스 간에 (상대적으로) 쉽게 변환할 수 있습니다.
2. (기본) CRUD 작업에 대해 SQL 코드를 작성할 필요가 없습니다. 생각해 보세요: 사용자 생성, 사용자 삭제, 사용자 업데이트.
3. SQL 위에 몇 가지 쿼리 메커니즘(HQL, Criteria API)을 제공하여 데이터베이스를 쿼리합니다. 지금은 이것이 SQL의 "객체 지향" 버전이라고 가정하겠습니다. 비록 나중에 설명할 예제가 없으면 약간 의미가 없지만 말입니# Hibernate

> Hibernate는 JPA 인터페이스를 구현한 구현체이자, 자바용 ORM Framework이다. => 자바 객체를 통해 DB종류 상관 없이 데이터베이스를 자바 객체 단위에서 관리할 수 있도록 한다. 내부적으로는 JDBC API를 사용한다.

Java 개발자는 일반적 으로 SQL 문을 작성하는 것보다 Java 클래스를 작성하는 데 더 익숙합니다. 따라서 많은 (그린필드) 프로젝트가 Java 우선 접근 방식으로 작성됩니다. 즉, 해당 데이터베이스 테이블을 생성하기 전에 Java 클래스를 생성해야 한다는 의미이다.

이는 자연스럽게 객체 관계형 매핑 질문으로 이어진다. 새로 작성된 Java 클래스에서 (아직 생성되지 않은) 데이터베이스 테이블로 어떻게 매핑합니까? 그리고 해당 Java 클래스 에서 데이터베이스를 생성할 수도 있나요 ? 적어도 처음에는?

Hibernate가 잘하는 것을 요약하려는 시도는 다음과 같다:

1. 초기 매핑과 별도로 많은 작업을 수행하지 않고도 데이터베이스 테이블과 Java 클래스 간에 (상대적으로) 쉽게 변환할 수 있다.
2. (기본) CRUD 작업에 대해 SQL 코드를 작성할 필요가 없다. 생각해 보세요: 사용자 생성, 사용자 삭제, 사용자 업데이트.
3. SQL 위에 몇 가지 쿼리 메커니즘(HQL, Criteria API)을 제공하여 데이터베이스를 쿼리합니다. 지금은 이것이 SQL의 "객체 지향" 버전이라고 가정. 비록 나중에 설명할 예제가 없으면 약간 의미가 없지만.

```properties
implementation 'org.hibernate.orm:hibernate-core:6.1.7.Final'
implementation 'org.postgresql:postgresql:42.5.4'
```

src/main/resources/META-INF/persistence.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.1">
    <persistence-unit name="demo">
        <class>com.example.demo.models.Person</class>
        <properties>
            <property name="jakarta.persistence.jdbc.url"
                      value="jdbc:postgresql://localhost:5432/postgres"/>
            <property name="jakarta.persistence.jdbc.user"
                      value="postgres"/>
            <property name="jakarta.persistence.jdbc.password"
                      value="password"/>
        </properties>
    </persistence-unit>
</persistence>

```

```java
import javax.persistence.Entity;
import javax.persistence.Table;
import javax.persistence.GeneratedValue;
import javax.persistence.Column;
import javax.persistence.Id;

@Entity
@Table(name = "people")
public class Person {
    @Id
    @Column(name = "name")
    private String name;

    @Column(name = "age") // 이 경우, 변수명과 속성(컬럼) 이름이 같기 때문에 @Column 애너테이션은 생략 가능하다.
    private Integer age;

    @Column(name = "gender")
    private String gender;

// …(후략)…
```

다음은 간단한 요약입니다.

* @Entity : Hibernate가 이해해야 할 마커: 데이터베이스의 테이블과 매핑되는 객체.
* @Table : 클래스를 매핑할 데이터베이스 테이블을 Hibernate에게 알려줍니다.
* @Column : 필드를 매핑할 데이터베이스 열을 Hibernate에게 알려줍니다.
* @Id 및 @GeneratedValue : Hibernate에게 테이블의 기본 키가 무엇인지, 그리고 그것이 데이터베이스에 의해 자동 생성되었는지 알려줍니다.

```java
public class JpaTest {
    private EntityManagerFactory entityManagerFactory;

    private EntityManager entityManager;

    @BeforeEach
    void setUp() {
        entityManagerFactory = Persistence.createEntityManagerFactory("demo");
        entityManager = entityManagerFactory.createEntityManager();
    }

    @AfterEach
    void tearDown() {
        entityManager.close();
        entityManagerFactory.close();
    }

    @Test
    void query() {
        Person person = entityManager.find(Person.class, "견우");

        System.out.println("*".repeat(80));
        System.out.println(person);
        System.out.println("*".repeat(80));
    }
}
```

```java
// Entity 생성 및 확인
Person person=new Person("Mr.Big",35,"남");
        entityManager.persist(person);

        Person found=entityManager.find(Person.class,"Mr.Big");
        assertEquals(person,found);

// 다른 트랜잭션에 가서 또 확인!
        Person person=entityManager.find(Person.class,"Mr.Big");
        assertEquals("Mr.Big",person.name());

// Entity의 상태를 바꾸면 commit했을 때 자동으로 UPDATE 수행.
        Person person=entityManager.find(Person.class,"Mr.Big");
        person.changeAge(30);

// 다른 트랜잭션에 가서 또 확인!
        Person person=entityManager.find(Person.class,"Mr.Big");
        assertEquals(30,person.age());

// Entity 삭제 및 확인
        Person person=entityManager.find(Person.class,"Mr.Big");
        entityManager.remove(person);

        Person found=entityManager.find(Person.class,"Mr.Big");
        assertNull(found);

// 다른 트랜잭션에 가서 또 확인!
        Person person=entityManager.find(Person.class,"Mr.Big");
        assertNull(person);
```

***

EntityManager엔 여러 Entity를 얻을 수 있는 메서드가 없다.

이를 위해 JPQL과 createQuery를 사용할 수 있다. JPQL은 SQL과 다르게 관계(테이블)이 아니라 JPA 객체를 대상으로 하는 SQL과 유사한 쿼리(질의) 언어다.

```java
String jpql="SELECT person FROM Person person";

        List<Person> people=entityManager
        .createQuery(jpql,Person.class) // 타입을 지정하지 않으면 Query, 지정하면 TypedQuery.
        .getResultList();

        assertEquals(2,people.size());
```

***

### @Embeddable

OOP에선 의미가 드러나지 않는 Primitive Type 대신 Value Object를 적극 활용하는 걸 권하는데, JPA에선 Aggregate Mapping을 통해 이를 지원할 수 있다.

우리의 예제를 보면 name과 gender가 모두 String이라 구분하기 어렵고, 특히 gender는 타입이 정확하면 좋을 것 같다. Gender 클래스를 만들어 보자.

```java

@Embeddable
public class Gender {
    @Column(name = "gender")
    private String value;

    private Gender() {
    }

    private Gender(String value) {
        this.value = value;
    }

    public static Gender male() {
        return new Gender("남");
    }

    public static Gender female() {
        return new Gender("여");
    }

    @Override
    public String toString() {
        return value;
    }
}
```

```java
// Person
@Embedded
private Gender gender;
```

지금은 이름이 String 하나지만, 연락처 앱 등을 생각해 보면 Name이란 Value Object를 만들어서 first name, last name 등에 대한 처리를 **“위임”** 했을 때 Person 객체가 SRP를 따르고 단순해질 거란 걸 알 수 있다.

### 관계 매핑 (Relationship Mapping)

데이터 모델의 Entity 간의 관계를 객체 참조로 간단하게 표현할 수 있는 방법으로, 주로 Jakarta Persistence API(JPA)를 사용하여 구현됩니다. 이는 객체 지향 프로그래밍에서 데이터베이스의 테이블 간 관계를 객체 간 참조로 표현하는 것을 지원합니다.

일반적으로는 [Domain-Driven Design(DDD)의 Aggregate](../architecture/domainModelPattern.md)를 구현하기 위해\
CascadeType.ALL과 orphanRemoval=true를 함께 사용하는 것이 권장되고 있습니다.\
Aggregate는 특정 엔터티와 그와 연결된 다른 엔터티들의 집합으로, 일관성을 유지하기 위한 경계를 정의합니다.

cascade 속성은 부모 엔터티가 자식 엔터티에 대한 변경을 어떻게 전파할지를 지정합니다. 즉, 부모 엔터티의 상태 변경이 자식 엔터티에 영향을 주는 방식을 설정합니다. CascadeType.ALL은 모든 변경을 전파하라는 의미입니다.

orphanRemoval 속성은 부모 엔터티에서 더 이상 참조되지 않는 자식 엔터티를 자동으로 삭제할지 여부를 지정합니다. 이를테면, 부모 엔터티에서 자식 엔터티를 삭제하면 해당 자식 엔터티가 데이터베이스에서도 삭제되도록 할 수 있습니다.

그러나 객체의 경계와 불변성을 다룰 때는 모든 관계를 무차별적으로 횡단하는 것을 피해야 합니다. 또한, 조회(읽기) 성능이 중요한 경우에는 Entity Relationship Mapping(ERM)을 넘어서는 관계 매핑보다는 Relational 접근이 필요할 수 있습니다.

쉽게 말하면:

* 객체 간의 관계를 설정할 때는 필요한 관계만 설정하고, 필요하지 않은 관계는 피하는 것이 중요합니다. 이렇게 함으로써 객체 간의 결합을 최소화하고 유연성을 유지할 수 있습니다.
* 조회 성능이 중요한 경우에는 데이터베이스와의 상호 작용을 직접 다루는 것이 더 효과적일 수 있습니다. 객체 지향적인 매핑이 아니라, 데이터베이스의 특성을 고려한 접근이 필요할 때가 있습니다.

따라서 일정 규모 이상의 시스템에서는 Command와 Query를 분리하여 다루는 것( CQRS(Command Query Responsibility Segregation) 디자인 패턴)이 권장됩니다. 전자는 JPA Entity와 관계 매핑을 활용하며, 후자는 JdbcTemplate 등의 SQL 매퍼를 적극적으로 활용하는 것을 추천합니다. Kotlin을 사용한다면 Exposed와 같은 라이브러리를 활용할 수 있습니다.

마지막으로, 만약 JPA, RDB 대신 [Event Sourcing](../../about-us/architecture/eventSourcing.md)을 도입한다면 더 큰 자유를 얻을 수 있습니다.

여기서는 Person이 Item을 “소유”하고, Item을 개별적으로 접근하는 일은 절대 없도록 하자. DDD에선 이런 걸 Aggregate란 개념으로 다룬다(자세한 건 DDD Lite 때 다룰 예정).

```java
@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
@JoinColumn(name = "person_name")
@OrderBy("name")
private List<Item> items=new ArrayList<>();
```

```java

@Entity
@Table(name = "items")
public class Item {
    @Id
    @Column(name = "name")
    private String name;

    @Column(name = "usage")
    private String usage;

    @Column(name = "person_name")
    private String personName;

    public Item() {
    }

    public Item(String name, String usage, String personName) {
        this.name = name;
        this.usage = usage;
        this.personName = personName;
    }
}
```

그냥 실행하면 “Association 'com.example.demo.models.Person.items' targets the type 'com.example.demo.models.Item' which is not an '@Entity' type” 에러가 나오게 된다. src/main/resources/META-INF/persistence.xml 파일에 class를 추가하자.

```xml

<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.1">
    <persistence-unit name="demo">
        <class>com.example.demo.models.Person</class>
        <class>com.example.demo.models.Item</class>
```

Item은 Person이 소유하기 때문에, 소유와 관련된 건 Person에서 처리한다.

```java
public void addItem(String name,String usage){
        Item item=new Item(name,usage,this.name);

        items.add(item);
        }

public void removeItem(String name){
        Optional<Item> item=items.stream()
        .filter(i->i.name().equals(name))
        .findFirst();

        if(item.isEmpty()){
        return;
        }

        items.remove(item.get());
        }

public List<Item> items(){
        return new ArrayList<>(items);
        }
```

이렇게 다른 Entity를 소유 및 관리하는 Aggregate를 잘 구성하면, 적절한 개수의 DAO를 만들어서 쓸 수 있게 된다.

하지만 여전히 몇 가지 문제가 있다:

1. persistence.xml 파일에 Entity를 계속 추가해야 한다.
2. EntityManager를 얻는 과정이 불편하다.
3. Transaction을 관리하기 불편하다.

이들은 모두 Spring을 사용함으로써 해결할 수 있고, Spring Data JPA를 사용하면 EntityManager를 사용하는 부분까지 작성하지 않아도 된다.

***

![](https://miro.medium.com/v2/resize:fit:1200/1\*oxQTGhzkbFHJ34p4Wvd9BQ.jpeg) 다.

```properties
implementation 'org.hibernate.orm:hibernate-core:6.1.7.Final'
implementation 'org.postgresql:postgresql:42.5.4'
```

src/main/resources/META-INF/persistence.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.1">
    <persistence-unit name="demo">
        <class>com.example.demo.models.Person</class>
        <properties>
            <property name="jakarta.persistence.jdbc.url"
                      value="jdbc:postgresql://localhost:5432/postgres"/>
            <property name="jakarta.persistence.jdbc.user"
                      value="postgres"/>
            <property name="jakarta.persistence.jdbc.password"
                      value="password"/>
        </properties>
    </persistence-unit>
</persistence>

```

```java
import javax.persistence.Entity;
import javax.persistence.Table;
import javax.persistence.GeneratedValue;
import javax.persistence.Column;
import javax.persistence.Id;

@Entity
@Table(name = "people")
public class Person {
    @Id
    @Column(name = "name")
    private String name;

    @Column(name = "age") // 이 경우, 변수명과 속성(컬럼) 이름이 같기 때문에 @Column 애너테이션은 생략 가능하다.
    private Integer age;

    @Column(name = "gender")
    private String gender;

// …(후략)…
```

다음은 간단한 요약입니다.

* @Entity : Hibernate가 이해해야 할 마커: 데이터베이스의 테이블과 매핑되는 객체.
* @Table : 클래스를 매핑할 데이터베이스 테이블을 Hibernate에게 알려줍니다.
* @Column : 필드를 매핑할 데이터베이스 열을 Hibernate에게 알려줍니다.
* @Id 및 @GeneratedValue : Hibernate에게 테이블의 기본 키가 무엇인지, 그리고 그것이 데이터베이스에 의해 자동 생성되었는지 알려줍니다.

```java
public class JpaTest {
    private EntityManagerFactory entityManagerFactory;

    private EntityManager entityManager;

    @BeforeEach
    void setUp() {
        entityManagerFactory = Persistence.createEntityManagerFactory("demo");
        entityManager = entityManagerFactory.createEntityManager();
    }

    @AfterEach
    void tearDown() {
        entityManager.close();
        entityManagerFactory.close();
    }

    @Test
    void query() {
        Person person = entityManager.find(Person.class, "견우");

        System.out.println("*".repeat(80));
        System.out.println(person);
        System.out.println("*".repeat(80));
    }
}
```

```java
// Entity 생성 및 확인
Person person=new Person("Mr.Big",35,"남");
        entityManager.persist(person);

        Person found=entityManager.find(Person.class,"Mr.Big");
        assertEquals(person,found);

// 다른 트랜잭션에 가서 또 확인!
        Person person=entityManager.find(Person.class,"Mr.Big");
        assertEquals("Mr.Big",person.name());

// Entity의 상태를 바꾸면 commit했을 때 자동으로 UPDATE 수행.
        Person person=entityManager.find(Person.class,"Mr.Big");
        person.changeAge(30);

// 다른 트랜잭션에 가서 또 확인!
        Person person=entityManager.find(Person.class,"Mr.Big");
        assertEquals(30,person.age());

// Entity 삭제 및 확인
        Person person=entityManager.find(Person.class,"Mr.Big");
        entityManager.remove(person);

        Person found=entityManager.find(Person.class,"Mr.Big");
        assertNull(found);

// 다른 트랜잭션에 가서 또 확인!
        Person person=entityManager.find(Person.class,"Mr.Big");
        assertNull(person);
```

***

EntityManager엔 여러 Entity를 얻을 수 있는 메서드가 없다.

이를 위해 JPQL과 createQuery를 사용할 수 있다. JPQL은 SQL과 다르게 관계(테이블)이 아니라 JPA 객체를 대상으로 하는 SQL과 유사한 쿼리(질의) 언어다.

```java
String jpql="SELECT person FROM Person person";

        List<Person> people=entityManager
        .createQuery(jpql,Person.class) // 타입을 지정하지 않으면 Query, 지정하면 TypedQuery.
        .getResultList();

        assertEquals(2,people.size());
```

***

### @Embeddable

OOP에선 의미가 드러나지 않는 Primitive Type 대신 Value Object를 적극 활용하는 걸 권하는데, JPA에선 Aggregate Mapping을 통해 이를 지원할 수 있다.

우리의 예제를 보면 name과 gender가 모두 String이라 구분하기 어렵고, 특히 gender는 타입이 정확하면 좋을 것 같다. Gender 클래스를 만들어 보자.

```java

@Embeddable
public class Gender {
    @Column(name = "gender")
    private String value;

    private Gender() {
    }

    private Gender(String value) {
        this.value = value;
    }

    public static Gender male() {
        return new Gender("남");
    }

    public static Gender female() {
        return new Gender("여");
    }

    @Override
    public String toString() {
        return value;
    }
}
```

```java
// Person
@Embedded
private Gender gender;
```

지금은 이름이 String 하나지만, 연락처 앱 등을 생각해 보면 Name이란 Value Object를 만들어서 first name, last name 등에 대한 처리를 **“위임”** 했을 때 Person 객체가 SRP를 따르고 단순해질 거란 걸 알 수 있다.

### 관계 매핑 (Relationship Mapping)

데이터 모델의 Entity 간의 관계를 객체 참조로 간단하게 표현할 수 있는 방법으로, 주로 Jakarta Persistence API(JPA)를 사용하여 구현됩니다. 이는 객체 지향 프로그래밍에서 데이터베이스의 테이블 간 관계를 객체 간 참조로 표현하는 것을 지원합니다.

일반적으로는 [Domain-Driven Design(DDD)의 Aggregate](../architecture/domainModelPattern.md)를 구현하기 위해\
CascadeType.ALL과 orphanRemoval=true를 함께 사용하는 것이 권장되고 있습니다.\
Aggregate는 특정 엔터티와 그와 연결된 다른 엔터티들의 집합으로, 일관성을 유지하기 위한 경계를 정의합니다.

cascade 속성은 부모 엔터티가 자식 엔터티에 대한 변경을 어떻게 전파할지를 지정합니다. 즉, 부모 엔터티의 상태 변경이 자식 엔터티에 영향을 주는 방식을 설정합니다. CascadeType.ALL은 모든 변경을 전파하라는 의미입니다.

orphanRemoval 속성은 부모 엔터티에서 더 이상 참조되지 않는 자식 엔터티를 자동으로 삭제할지 여부를 지정합니다. 이를테면, 부모 엔터티에서 자식 엔터티를 삭제하면 해당 자식 엔터티가 데이터베이스에서도 삭제되도록 할 수 있습니다.

그러나 객체의 경계와 불변성을 다룰 때는 모든 관계를 무차별적으로 횡단하는 것을 피해야 합니다. 또한, 조회(읽기) 성능이 중요한 경우에는 Entity Relationship Mapping(ERM)을 넘어서는 관계 매핑보다는 Relational 접근이 필요할 수 있습니다.

쉽게 말하면:

* 객체 간의 관계를 설정할 때는 필요한 관계만 설정하고, 필요하지 않은 관계는 피하는 것이 중요합니다. 이렇게 함으로써 객체 간의 결합을 최소화하고 유연성을 유지할 수 있습니다.
* 조회 성능이 중요한 경우에는 데이터베이스와의 상호 작용을 직접 다루는 것이 더 효과적일 수 있습니다. 객체 지향적인 매핑이 아니라, 데이터베이스의 특성을 고려한 접근이 필요할 때가 있습니다.

따라서 일정 규모 이상의 시스템에서는 Command와 Query를 분리하여 다루는 것( CQRS(Command Query Responsibility Segregation) 디자인 패턴)이 권장됩니다. 전자는 JPA Entity와 관계 매핑을 활용하며, 후자는 JdbcTemplate 등의 SQL 매퍼를 적극적으로 활용하는 것을 추천합니다. Kotlin을 사용한다면 Exposed와 같은 라이브러리를 활용할 수 있습니다.

마지막으로, 만약 JPA, RDB 대신 [Event Sourcing](../../about-us/architecture/eventSourcing.md)을 도입한다면 더 큰 자유를 얻을 수 있습니다.

여기서는 Person이 Item을 “소유”하고, Item을 개별적으로 접근하는 일은 절대 없도록 하자. DDD에선 이런 걸 Aggregate란 개념으로 다룬다(자세한 건 DDD Lite 때 다룰 예정).

```java
@OneToMany(cascade = CascadeType.ALL, orphanRemoval = true)
@JoinColumn(name = "person_name")
@OrderBy("name")
private List<Item> items=new ArrayList<>();
```

```java

@Entity
@Table(name = "items")
public class Item {
    @Id
    @Column(name = "name")
    private String name;

    @Column(name = "usage")
    private String usage;

    @Column(name = "person_name")
    private String personName;

    public Item() {
    }

    public Item(String name, String usage, String personName) {
        this.name = name;
        this.usage = usage;
        this.personName = personName;
    }
}
```

그냥 실행하면 “Association 'com.example.demo.models.Person.items' targets the type 'com.example.demo.models.Item' which is not an '@Entity' type” 에러가 나오게 된다. src/main/resources/META-INF/persistence.xml 파일에 class를 추가하자.

```xml

<persistence xmlns="http://xmlns.jcp.org/xml/ns/persistence" version="2.1">
    <persistence-unit name="demo">
        <class>com.example.demo.models.Person</class>
        <class>com.example.demo.models.Item</class>
```

Item은 Person이 소유하기 때문에, 소유와 관련된 건 Person에서 처리한다.

```java
public void addItem(String name,String usage){
        Item item=new Item(name,usage,this.name);

        items.add(item);
        }

public void removeItem(String name){
        Optional<Item> item=items.stream()
        .filter(i->i.name().equals(name))
        .findFirst();

        if(item.isEmpty()){
        return;
        }

        items.remove(item.get());
        }

public List<Item> items(){
        return new ArrayList<>(items);
        }
```

이렇게 다른 Entity를 소유 및 관리하는 Aggregate를 잘 구성하면, 적절한 개수의 DAO를 만들어서 쓸 수 있게 된다.

하지만 여전히 몇 가지 문제가 있다:

1. persistence.xml 파일에 Entity를 계속 추가해야 한다.
2. EntityManager를 얻는 과정이 불편하다.
3. Transaction을 관리하기 불편하다.

이들은 모두 Spring을 사용함으로써 해결할 수 있고, Spring Data JPA를 사용하면 EntityManager를 사용하는 부분까지 작성하지 않아도 된다.

***

![](https://miro.medium.com/v2/resize:fit:1200/1\*oxQTGhzkbFHJ34p4Wvd9BQ.jpeg)
