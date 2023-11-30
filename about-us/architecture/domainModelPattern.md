# Domain model Pattern

도메인: 지식, 영향력 또는 활동 영역의 선택된 측면을 설명하는 추상화 체계 => 개발자 입장에서 소프트웨어로 해결하고자하는 문제 및 관심사
___

### Domain model:

Domain Model이란 해당 도메인에서 비즈니스적인 의미를 가지는 object

> An object model of the domain that incorporates both behavior and data. - Martin Fowler (P of EAA Catalog)
>

도메인 모델 예시

```java
public class Product {
    // ...
    private ProductSalesInfo productSalesInfo;
    private ProductMetaInfo prroductMetaInfo;
    private Content content;

    // ...
    /*
            판매 시작
            @param startSaleDt
     */
    public void startSale(LocalDateTime startSaleDt) { // 도메인로직
        validateProduct();
        productSalesInfo = ProductSalesInfo.startSale(startSaleDt);
    }
}
```

> A Service Layer defines an application's boundary [Cockburn PloP] and its set of available operations from the
> perspective of interfacing client layers. It encapsulates the application's business logic, controlling transactions
> and
> coor-dinating responses in the implementation of its operations.
>
>마틴 파울러 - Service Layer
>
마틴파울러는 Service Layer 란 어플리케이션의 비즈니스 로직 즉, 도메인을 보호하는 레이어라고 말한다. 즉, 이 정의를 명확히 지키기 위해서는 Presentation Layer 에 도메인을 노출해서는
안된다. 따라서 이러한 관점으로는 도메인은 서비스 레이어에서 DTO로 변환되어 컨트롤러로 전달되어야 한다.
[출처](https://hudi.blog/data-transfer-object/)

**[Anemic Domain Model - 도메인 모델 안티패턴](anemicDomainModel.md)**

___

### Domain model Pattern:

아키텍쳐상의 도메인 계층을 객체 지향 기법으로 **구현**하는 패턴

![](https://wikibook.co.kr/images/readit/20141002/table4-1.png)

> 도메인 모델과 관련된 코드는 모두 한 계층에 모으고 사용자 인터페이스 코드나 애플리케이션 코드, 인프라스트럭처 코드와 격리하라. 도메인 객체(표현이나 저장, 애플리케이션 작업을 관리하는 등의 책임에서 자유로운)는
> 도메인 모델을 표현하는 것에만 집중할 수 있다.
>
> https://wikibook.co.kr/article/layered-architecture/

=> 시스템이 제공할 도메인 규칙을 구현한다.

![content](https://wikibook.co.kr/images/readit/20141002/figure4-1.png)
객체는 자신이 속한 계층에 맞는 책임을 수행하며, 같은 계층에 존재하는 객체와 더 결합된다.
___

### Domain Driven Development

> https://zetawiki.com/wiki/%EB%8F%84%EB%A9%94%EC%9D%B8_%EC%A3%BC%EB%8F%84_%EC%84%A4%EA%B3%84_DDD
>
단점:

- Architecture 구현에서 생성되는 생각보다 많은 코드
- 각 도메인에 대한 높은 이해도가 필요

장점:

- 보편적인 언어 사용에 따른 빠른 커뮤니케이션 (데이터 중심 => 비즈니스 중심)
- 도메인간 관계가 복잡한 경우 큰 틀에서 정리가 가능
- 도메인의 분리에 따른 유지보수에 대한 편의성
- 새로운 기능 및 요구 사항에 대한 유연성

장점(코드측면):

- Encapsulation
- Loose coupling, High cohesion
- Domain Logic의 분리로 Business Logic에 집중
- 코드 가독성

<details><summary>Entity와 VO(ValueObject)</summary>
도메인 모델은 id 여부에 따라 Entity와 VO(ValueObject)로 나뉜다.

**Entity**

- id가 있어 각각의 개체를 고유하게 식별 할 수 있는 경우
- 엄밀히 불변은 아니고 시간이 지나면서 상태가 변경될 수 있는 대상임. (그러나 이와 별개로 앱단에서는 불변 객체로 처리하는 것이 좋다. 함수형.)
- e.g., Member

**VO ( value object )**

- id가 없음
- To avoid aliasing bugs I follow a simple but important rule:value objects should be immutable .
- 필드 값 상태가 같다면 같은 객체로 처리해도 되는 경우
    - 그래서 이름이 ‘value’ object임. (반대 되는 개념 : reference object)
    - 참조가 아니라 값으로 동등함을 비교하는게 더 자연스러운 대상들.
    - 따라서 equals, hashCode 구현 필수
- e.g., Money


- 특별히 비즈니스 적인 의미를 가지지 않아도, 데이터 뭉치를 하나로 묶어주는 클래스도 VO로 본다 - 리팩터링 6.8절

    - 지금은 아니더라도, 이렇게 만들어진 VO는 시간이 지나면서 로직과 책임을 가지게 되어 진정한 Domain Model이 되는 경우가 많다.
    - larger structures can often be programmed as value objects if they don’t have any conceptual identity or don’t
      need share references around a program.

> 출처: https://umbum.dev/1203/
>
</details>


___

### Aggregate

도메인 주도 설계(DDD)에서 Aggregate는 연관된 객체들의 그룹입니다. Aggregate는 일관성 경계를 정의하고 해당 경계 내에서 일관성이 유지되어야 하는 일련의 객체로 구성됩니다. Aggregate는 단일
루트 엔터티(root entity)로 구성되며, 이 루트 엔터티는 Aggregate 내부의 다른 객체들에 대한 유일한 진입점(entry point) 역할을 합니다.

### Aggregate의 특징:

1. **루트 엔터티 (Root Entity):**

- Aggregate의 핵심은 루트 엔터티입니다. 이 루트 엔터티는 Aggregate 내에서 일관성을 유지하고 Aggregate 외부에서 Aggregate에 접근하는 유일한 방법입니다.

2. **일관성 경계 (Consistency Boundary):**

- Aggregate는 일관성 경계를 가지며, 일관성은 해당 Aggregate 내에서만 유지되어야 합니다. 즉, Aggregate 내의 객체들은 서로의 상태를 변경할 수 있지만, 외부에서 직접적으로 상태를 변경하는
  것은 허용되지 않습니다.

3. **식별자 (Identifier):**

- Aggregate는 각각 고유한 식별자를 가지고 있습니다. 이 식별자는 루트 엔터티의 주요 특징으로, 다른 Aggregate와의 구분을 위해 사용됩니다.

4. **캡슐화 (Encapsulation):**

- Aggregate 내부의 객체들은 외부에서 직접적으로 접근되지 않고, 루트 엔터티를 통해 간접적으로만 접근됩니다. 이것은 객체 간의 캡슐화를 유지하고 일관성을 보장하는 데 도움이 됩니다.

5. **생명 주기 (Lifecycle):**

- Aggregate는 자체적인 생명 주기를 가지며, 루트 엔터티의 생명 주기에 따라 내부 객체들도 관리됩니다. 루트 엔터티가 생성되거나 삭제될 때, 해당 Aggregate 내의 모든 객체들도 함께 생성 또는
  삭제됩니다.

### 예시:

가령, 주문(Order)이라는 Aggregate를 살펴보겠습니다. 주문은 주문 항목(OrderItem)과 배송 정보(Delivery)로 이루어져 있습니다. 이때, 주문이 Aggregate의 루트 엔터티가 되며, 주문
항목과 배송 정보는 루트 엔터티에 속하는 내부 객체들로 구성됩니다. 이러한 구조에서 주문을 통해 주문 항목과 배송 정보에 접근하고 변경할 수 있습니다.

```java
public class Order {
    @Id
    private Long orderId;

    private List<OrderItem> orderItems;
    private Delivery delivery;

    // ... 다른 속성과 메서드들 ...
}

public class OrderItem {
    @Id
    private Long orderItemId;

    private String product;
    private int quantity;

    // ... 다른 속성과 메서드들 ...
}

public class Delivery {
    @Id
    private Long deliveryId;

    private String address;
    private DeliveryStatus status;

    // ... 다른 속성과 메서드들 ...
}
```

Aggregate는 DDD에서 복잡한 도메인 모델을 조직화하고 일관성을 유지하는 강력한 도구로 활용됩니다. Aggregate를 정의함으로써 객체 간의 관계와 책임을 명확히 정의할 수 있습니다.

---
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/4QHvTeeTsj0/0.jpg)](https://youtu.be/4QHvTeeTsj0?si=u_UMRCb_PAcR-qsx&t=431)

