# Domain-driven design

2004년 Eric Evans가 사람들이 Blue Book이라고 부르는 책을 냄.

> [Blue Book](https://www.domainlanguage.com/ddd/blue-book/)
>
소프트웨어는 복잡하다. 이를 관리하지 못 하면 너무 빨리 (유지보수하기 힘든) 레거시 시스템으로 변하게 된다. 소프트웨어의 복잡성은 도메인의 복잡성에서 유래한다. 현실이 복잡하기 때문에, 현실을 위한 소프트웨어가
복잡한 것도 당연하다. 우리는 기술 영역과 도메인 영역을 분리하고(관심사의 분리), 도메인을 적절히 다루기 위한 모델을 구축함으로써 복잡성과 싸울 것이다. 이를 위한 사고방식, 도구 모음이 바로 도메인 주도 설계다.
Eric Evans의 책을 보면 다양한 Pattern을 소개하는 형태로 씌여있다.

DDD의 전제는 다음과 같다:

1. “대부분의” 소프트웨어는 도메인과 도메인 로직에 집중해야 한다.
2. 복잡한 도메인 설계는 모델을 기반으로 해야 한다.

DDD를 Microservices를 위한 수단 정도로 여기고 학습하는 경우가 있는데, 이런 접근보다는 “비즈니스를 어떻게 더 잘 이해하고, 이를 설계와 코드에 녹여낼 수 있을까? 어떻게 하면 비즈니스 도메인에 대한
이해가 심화될 때마다 이를 설계와 코드에 반영할 수 있을까?”라고 생각해야 한다. 이게 바로 진짜 기술 부채 문제를 다루는 방법이다.

---

## 기술 부채 (Technical Debt)

> 소프트웨어 출시 이후 학습한 내용을 다시 설계와 코드에 반영하지 않는 것
>

> [Ward Cunningham의 Debt Metaphor 설명 영상](https://www.youtube.com/watch?v=pqeJFYwnkjE)
>

→ [텍스트로 옮긴 것](http://wiki.c2.com/?WardExplainsDebtMetaphor)

많은 사람들이 기술 부채를 코드를 엉망으로 작성하는 것으로 오해하는데 그렇지 않다.

“A lot of bloggers at least have explained the debt metaphor and **confuse**d it, I think, with the idea that you could
**write code poorly** with the intention of doing a good job later and thinking that that was the primary source of
debt.”

소프트웨어를 출시하면, 우리는 뭔가를 배우게 된다(마찬가지로 Eric Evans는 지식 탐구, 지속적인 학습을 강조한다). 하지만 대부분은 이를 다시 설계와 코드에 반영하지 않는다. **=> 이는 갚을 생각 없이
돈을 빌리는 것과 같은 일이다.**

“I think that there were plenty of cases where people would rush software out the door and **learn things** but **never
put that learning back into the program**, and that by analogy was borrowing money thinking that you never had to pay it
back.”

배웠으면 갚아야 한다. 우리의 이해가 설계와 코드에 반영되지 않으면, 이해하기 어려운 복잡한 설계와 코드 앞에 좌절하고, 작업하는데 더 오래 걸릴 거다. 이게 기술 부채가 의미하는 거다.

“By the same token, if you develop a program for a long period of time by **only adding features** and **never
reorganizing it to reflect your understanding** of those features, then eventually that program simply does not contain
any understanding and all efforts to work on it **take longer and longer**.”

---

### 보편 언어 (Ubiquitous Language)

> 모두가 하나의 언어(어휘 모음)를 사용한다.
>
도메인 전문가, 비즈니스 분석가, 소프트웨어 설계자, 프로그래머 등은 모두 같은 언어를 사용한다. 도메인 전문가가 쓰는 어휘에 끌려가야 한다는 의미가 아니라, 비즈니스 도메인을 적절하게 다룰 수 있는 용어를 함께
만들어서 코드를 작성할 때도 사용해야 한다는 의미다. 즉, 설계하기 어렵거나 코딩하기 어려운 어휘 체계는 재검토가 필요하다. 이를 통해 우리는 더 똑똑해지고, 긍정적인 기술 부채를 쌓아나갈 것이다(물론, 갚아야
한다).

### 제한된 컨텍스트 (Bounded Context)

정말로 커다란 시스템을 만든다면, 하나의 어휘가 다양한 의미로 쓰일 위험이 있다. 예를 들어 Account는 어떤 경우에는 사용자의 계정을 의미하지만, 어떤 경우에는 은행의 통장을 의미한다. 그렇다고
UserAccount, BankAccount 등으로 나눠서 쓰기 시작하면 접두어 내지는 접미어가 넘쳐나는 상황에 직면하게 된다. 대화할 때 비용이 증가하게 되는 거다.

> “지난 번에 이야기한 Account 있잖아요?”
> “어떤 Account요?”
> “ㅇㅇ은행이랑 거래하는 기능 만들면서 추가한 Account요.”
> “아, 은행이랑 거래하는 맥락에서 이야기하려는 거죠?”
>

우리는 맥락을 좁힘으로써 하나의 어휘가 하나의 대상을 지시하는 이상적인 상황을 만들 수 있다. 여러 맥락을 뒤섞어서 이야기하거나 작업하는 경우는 매우 드믈 것이다. 우리가 영어와 한국어를 terrible하게 마구
mix해서 conversation하는 situation이 엄청나게 rare한 것처럼 말이다.

따라서 우리는 하나의 시스템을 잘 조직화된 Bounded Context로 나눔으로써 보편 언어를 잘 만들고 유지할 것이다.

### 하위 도메인 (Subdomain)

시스템을 나누는데 Bounded Context라는 단위를 사용한다면, 도메인을 나눌 땐 Subdomain이라는 단위를 사용한다. 전자는 소프트웨어를 조직화하는 방법이라면, 후자는 현실을 조직화하는 방법이다. 전자는
개발하면서 발전시킬 수 있지만, 후자는 개발하면서 맞춰나가야 한다. 좀 더 어렵게 (하지만 더 정확하게) 표현하면, Subdomain은 Problem Space고, Bounded Context는 Solution
Space다.

---

### Aggregate

도메인 주도 설계(DDD)에서 Aggregate는 연관된 도메인 모델, 객체들의 그룹입니다. Aggregate는 일관성 경계를 정의하고 해당 경계 내에서 일관성이 유지되어야 하는 일련의 객체로 구성됩니다.
Aggregate는 단일
루트 엔터티(root entity)로 구성되며, 이 루트 엔터티는 Aggregate 내부의 다른 객체들에 대한 유일한 진입점(entry point) 역할을 합니다.

Aggregate는 불변식(Invariant)을 유지하고 여러 도메인 객체를 사용하기 좋게 만들어 준다.

불변식은 우리가 지켜야 할 제약 조건, 규칙이라고 할 수 있다.

### 예시:

가령, 주문(Order)이라는 Aggregate를 살펴보겠습니다. 주문은(Order) 주문 항목(Lineitem)으로 이루어져 있습니다.

우리는 Order라는 Entity를 Order라는 Aggregate의 Root로 삼고,

하나의 객체에 동시에 접근할 수 없다는 제약을 통해(일반적으로 이 목표는 Transaction을 통해 달성한다) 불변식을 지킬 수 있게 된다.

이 경우 불변식만 지켜지는 게 아니라, Aggregate가 제공하는 적절한 인터페이스를 통해 더 나은 표현력도 가질 수 있게 된다.

Aggregate는 DDD에서 복잡한 도메인 모델을 조직화하고 일관성을 유지하는 강력한 도구로 활용됩니다. Aggregate를 정의함으로써 객체 간의 관계와 책임을 명확히 정의할 수 있습니다.

IDDD의 저자인 Vaughn Vernon은 4가지 경험 법칙을 제안한다:

1. 불변식을 통해 일관성 경계를 찾아서 모델링한다. Aggregate은 **트랜잭션적** 일관성 경계와 동의어다.
2. 작은 Aggregate으로 설계한다.
3. ID로 다른 Aggregate을 참조한다. JPA 등을 사용하면 관련된 객체를 모두 직접 참조할 수 있게 되는데, 그렇게 하지 않는다.
4. 경계 밖에선 결과적 일관성을 사용한다. 이를 위해 도메인 이벤트 등을 사용할 수 있다.

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

---

### 리파지터리 (Repository)

Repository는 Aggregate를 관리하는 Collection처럼 작동한다. 이는 두 가지 의미를 갖는데,

1. 오직 Aggregate만 Repository를 갖는다.
2. Repository는 영속화 방법 및 기술을 감춘다.

Spring Data JPA는 이 둘을 만족시키기 위한 기능을 갖추고 있다. Aggregate에 대해서만 Repository를 만들어도 안정적으로 작동할 수 있도록 CascadeType.ALL과
orphanRemoval=true 같은 기능을 제공하고 있고, Persistence Context를 통해 Collection처럼 사용하는 게 가능하며, 아예 인터페이스만 만들면 나머지는 크게 신경 쓰지 않아도 되는
기능까지 제공한다.

Repository는 사실상 전역으로 접근할 수 있어야 하는데, Spring을 사용하면 너무나 간단히 DI를 통해 접근 가능하게 된다.

따라서, 우리는 적절한 Aggregate를 발견하고, 적절히 책임을 나눌 수 있도록 Entity와 Value Object로 구성(또는 분해)하고, 이를 위한 Repository를 만듦으로써, 여러 기술 문제와 무관한
비즈니스 도메인에 집중할 수 있게 된다. 이렇게 비즈니스 도메인에 집중한 코드를 모아둔 곳을 Domain Layer라고 부르며, Repository와 Aggregate를 사용하는 코드가 모인 곳을
Application Layer(여기선 애플리케이션의 기능, Use Case가 직접적으로 드러나게 된다), Web 등 구체적인 기술로 사용자와 소통하는 코드가 모인 곳을 UI Layer라고 부른다. 이게 바로
Layered Architecture가 DDD와 함께 다뤄지는 이유다. Layered Architecture의 도움 없이는 도메인 모델을 Domain Layer에 격리하는 게 불가능하다.

> [Layered Architecture](layeredArchitecture.md)
>


---
[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/4QHvTeeTsj0/0.jpg)](https://youtu.be/4QHvTeeTsj0?si=u_UMRCb_PAcR-qsx&t=431)

