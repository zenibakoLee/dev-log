# SOLID principle

👉 엉클 밥이 2003년에 <클린 소프트웨어>에서 5가지 객체지향 설계 원칙을 정리했고, 마이클 페더스가 순서를 조금 바꿔서 SOLID라고 부르기 시작함.

---

### SRP (Single Responsibility Principle, 단일 책임 원칙)

> 1979년에 톰 드마르코와 메이릴 페이지 존스가 언급한 응집도(cohesion)를 다루는 설계 원칙. 엉클 밥은 이를 “한 클래스는 단 한 가지의 변경 이유만을 가져야 한다”고 이야기한다.
>

> 한 클래스는 하나의 책임만 가져야 한다.
>
> 변경이 있을 때 파급효과가 적으면 단일 책임 원칙을 잘 따른 것.

JdbcTemplate를 사용하는 AddProductToCartService를 생각해 보자. 우리는 언제 이 클래스를 수정해야 할까?

1. 장바구니에 상품을 담는 방식이 변경됐을 때
2. 장바구니 상태를 DB에 저장하는 방식이 변경됐을 때

우리는 CartRepository를 추출/분리함으로써 SQL문이 바뀌거나 심지어 DBMS가 바뀌는 상황이 벌어져도 AddProductToCartService를 수정하지 않아도 된다.

SRP를 따르면 우리는 테스트하기 쉽고, 재사용하기 편하며, 더 작고 이해하기 쉬운 클래스를 얻게 된다.

---

### OCP (Open-Closed Principle, 개방-폐쇄 원칙)

1988년에 버트런드 마이어가 제안한 설계 원칙.


> “소프트웨어 개체(클래스, 모듈, 함수 등)는 확장에 대해 열려 있어야 하고, 수정에 대해서는 닫혀 있어야 한다.”
>

1. Open: 모듈의 기능을 변경할 수 있어야 한다.
2. Close: 변경이 다른 곳으로 퍼져나가지 않아야 한다.

이 목표는 추상화를 통해 달성 가능하고, Java에서는 interface를 활용한다.

> 다형성을 활용해 보자. (역할과 구현의 분리)
>
> 인터페이스로 구현한 새로운 클래스를 하나 만들어 새로운 기능을 구현

`CartRepository`라는 인터페이스를 만들고, `JdbcCartRepository`란 클래스로 구현했다고 해보자. 어느날 DBMS를 MongoDB로 변경하기로 했다면 어떻게 될까?
우리는 `MongoCartRepository`라는 클래스를 새로 만들 수 있고, 기존에 `CartRepository`를 사용하던 곳은 아무 것도 수정하지 않아도 된다.

엉클 밥에 따르면 “OCP는 객체지향 설계의 심장이라 할 수 있다”. 그 본질은 다형성에 대한 이야기지만, 여기에 OCP란 이름을 붙여서 관리하는 게 중요하다.

---

### LSP (Liskov Substitution Principle, 리스코프 치환 원칙)

바버라 리스코프는 1968년에 미국에 있는 컴퓨터 학과에서 박사 학위를 취득한 최초의 여성. 2008년에 튜링상을 수상했다.

1998년에 바버라 리스코프는 타입의 관계에 대해 이렇게 이야기했다.

> “타입 S의 각 객체 o1과 타입 T의 각 객체 o2가 있을 때, T로 프로그램 P를 정의했음에도 불구하고, o2를 o1로 치환할 때 P의 행위가 변하지 않으면, S는 T의 서브타입이다.”
>

우리가 따를 수 있는 원칙으로 정리하면 다음과 같다.

> “서브타입(subtype)은 그것의 기반 타입(base type)으로 치환 가능해야 한다.”
>

> 프로그램의 객체는 프로그램의 정확성을 깨뜨리지 않으면서 하위타입의 인스턴스로 바꿀 수 있어야 한다.
>
> 예) 자동차 인터페이스의 엑셀은 앞으로 가는 기능. 뒤로 가는 엑셀이 존재하면 LSP 위반

유명한 사례로 `Rectangle(직사각형)` 클래스를 상속하는 `Square(정사각형)` 클래스가 있다.

```java
public class Rectangle {
    protected int width;
    protected int height;

    public void changeWidth(int width) {
        this.width = width;
    }

    public void changeHeight(int height) {
        this.height = height;
    }

    public int area() {
        return width * height;
    }
}

public class Square extends Rectangle {
    @Override
    public void changeWidth(int width) {
        this.width = width;
        this.height = width;
    }

    @Override
    public void changeHeight(int height) {
        this.width = height;
        this.height = height;
    }
}
```

이렇게 되면 우리는 `Rectangle`에 대해 오해하게 된다.

```java
@Test
void area(){
        Rectangle rectangle=new Rectangle();
        rectangle.changeWidth(5);
        rectangle.changeHeight(4);
        assertThat(rectangle.area()).toBe(5*4);
        }
```

이런 평범한 코드에 `Square`를 넣어보자.

```java
@Test
void area(){
        Rectangle rectangle=new Square();
        rectangle.changeWidth(5);
        rectangle.changeHeight(4);
        assertThat(rectangle.area()).toBe(5*4);
        }
```

이 테스트는 실패한다. 이 경우엔 `Square`를 생성하는 곳이 가까워서 문제의 원인을 추측하기 쉽지만, 프로그램이 거대해지고 객체를 생성하는 곳과 사용하는 곳이 멀어지면(Spring은 이게 가능하도록 적극
돕는다!) 원인을 파악하기 힘든 버그와 싸우는 일이 잦아질 것이다.

LSP를 지키고 있는지 파악하는 방법 중 하나는 상속이 “행위” 측면에서 Is-A 관계인지 파악하는 것이다. 정사각형은 직사각형인가? 우리의 수학적인 직관은 그렇다고 이야기하겠지만, 객체의 행위 측면에서 보면 둘은
엄격히 분리된다. 우리가 올바른 Is-A 관계를 파악하기 위해 “X는 Y인가요?”라고 물을 때 기계적으로 답을 낼 수 없다.

---

### ISP (Interface Segregation Principle, 인터페이스 분리 원칙)

> ISP는 응집력이 없는 커다란 인터페이스를 여러 개의 작은 인터페이스로 나눌 것을 제안한다.
>
> = 특정 클라이언트를 위한 인터페이스 여러 개가 범용 인터페이스 하나보다 낫다.

상품 검색 기능이 늘어나면서 `ProductRepository`가 엄청나게 커지는 경우를 생각해 보자.

하나의 구현이 여러 인터페이스를 구현할 수 있고, Spring Data JPA라면 하나의 인터페이스가 여러 인터페이스를 상속하는 형태로 작성할 수 있다.

```java
public interface SearchProductRepository {
    List<Product> findAllByName(String name);
}

public interface CommandProductRepository {
    Product save(Product product);
}

// 원래는 이렇게 분리된 인터페이스를 구현하는 클래스가 있어야 함.
public class ProductRepository
        implements SearchProductRepository, CommandProductRepository {
}

// Spring Data JPA를 쓰니까 이렇게 쓸 수 있음.
public interface ProductRepository extends
        SearchProductRepository, CommandProductRepository,
        CrudRepository<Product, ProductId> {
}
```

이제 `SearchProductService`에선 save 등을 신경쓰지 않아도 된다. 또한, IntelliJ IDEA 등의 도구는 검색과 관련된 메서드만 자동완성으로 제공할 것이다.

```java

@Service
@Transactional(readOnly = true)
public class SearchProductService {
    private final SearchProductRepository searchProductRepository;

    public SearchProductService(SearchProductRepository searchProductRepository) {
        this.searchProductRepository = searchProductRepository;
    }

    public List<Product> searchByName(String name) {
        return searchProductRepository.findAllByName(name);
    }
}
```

마찬가지로, 상품을 조작하는 곳에선 어떻게 검색을 하는지 전혀 신경쓰지 않아도 된다. 다른 검색 기능이 추가된다고 해도 `CommandProductRepository`가 바뀌지 않으므로 영향을 받지 않는다.

```java

@Service
@Transactional
public class CreateProductService {
    private final CommandProductRepository commandProductRepository;

    public CreateProductService(CommandProductRepository commandProductRepository) {
        this.commandProductRepository = commandProductRepository;
    }

    public void createProduct(String name) {
        Product product = Product.create(name);
        commandProductRepository.save(product);
    }
}
```

만약 우리가 `ProductService`라는 클래스에 Product와 관련된 기능을 모았다면, 인터페이스만이라도 분리해서 각 기능이 명시적으로 보이게 만들 수도 있다.

```java
public interface CreateProductUseCase {
    void createProduct(String name);
}

public interface SearchProductUseCase {
    List<Product> searchByName(String name);
}

public class ProductService
        implements CreateProductUseCase, SearchProductUseCase {
    // …(중략)…
}
```

---

### DIP (Dependency Inversion Principle, 의존관계 역전 원칙)

> 엉클 밥은 DIP에 대해 이렇게 설명한다:
>

1. 상위 수준의 모듈은 하위 수준의 모듈에 의존해서는 안 된다. 둘 모두 추상화에 의존해야 한다.
2. 추상화는 구체적인 사항에 의존해서는 안 된다. 구체적인 사항은 추상화에 의존해야 한다.

> "구체화가 아닌 추상화에 의존해야한다." 를 구현하는 방법
>

여기서 상위 수준의 모듈은 비즈니스 로직을 다루는 부분을 의미하고, 하위 수준의 모듈은 상위 수준의 모듈에서 호출하는 기술적인 부분을 의미한다.

`AddProductToCartService`가 `JdbcCartRepository`에 의존한다면, 비즈니스 로직에 가까운 Application Layer의 객체가 기술적인 문제를 다루는 Infrastructure
Layer의 객체에 의존하는 꼴이 된다. 이렇게 되면 기술적인 문제가 바뀔 때 비즈니스 로직에 영향을 주게 된다. 따라서 의존성 방향을 [Application Layer → Infrastructure Layer]에서
역전된 [Infrastructure Layer → Application Layer]로 만드는 설계가 필요하다.

이를 위해 `CartRepository`라는 인터페이스를 도입해 Application Layer에 배치할 수 있다(정확히는 Domain Layer에 배치). `JdbcCartRepository`
가 `CartRepository`를 상속 또는 구현할 경우, `JdbcCartRepository`는 `CartRepository`에 의존하는 형태가 되고, 의존성의 방향이 역전된다.

인터페이스에 의존하면서 구체적인 인스턴스를 활용하기 위해서는 DI가 필요하고, Spring을 사용하면 아주 쉽게 DI와 DIP를 활용할 수 있게 된다(DI와 DIP의 I가 서로 다르다는 점에 주의!).
