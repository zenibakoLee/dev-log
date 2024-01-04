# CQRS

### CQS (Command Query Separation)

> [Command Query Separation - Martin Fowler](https://martinfowler.com/bliki/CommandQuerySeparation.html)
>

> [Command–query separation - Wiki](https://en.wikipedia.org/wiki/Command–query_separation)
>

일반적인 객체는 시간에 따라 변하는 “상태”를 갖는다. 메서드(연산)는 캡슐화된 상태를 변화시키는 유일한 수단이고, 시간에 따라 변하는 상태는 우리가 객체를, 더 정확히는 프로그램을 쓰는 이유다. 하지만 언제 어떻게
상태가 변하는지 예측하기 힘들다면 프로그램을 사용하기 힘들고, 유지보수하기도 어렵다.

따라서, 우리는 상태를 변경하지 않고 상태를 알 수 있는, side-effect가 없는 메서드를 따로 분리하면 좋다. 더 나아가, 상태를 변경하는 메서드는 상태를 리턴하지 않게 하면 확실히 분리할 수 있다.

> 상태를 변경하지 않고 상태를 얻는 메서드를 Query라고 하고, 상태를 변경하고 상태를 돌려주지 않는 메서드를 Command라고 한다.
>
이 둘을 잘 나누는 건 메서드 단위의 적극적인 SRP기도 하고, 순수 함수와
함수형 도구를 활용하기 위한 준비 과정이 될 수도 있다.

(적절하게 제약을 건다면) CRUD는 동시에 실행되지 않고 개별적으로 실행된다. CUD는 Command, R은 Query라고 볼 때 도메인 지식과 비즈니스 로직은 Command에 집중된다. Query에 상태를 바꾸지
않는 고수준의 순수 함수를 집중할 수도 있지만, 이런 경우는 SRP에 따라 분리되거나 객체 외부에서 함수형 기법을 활용하게 될 가능성이 크다.

---

### CQRS (Command Query Responsibility Segregation)

> [Greg Young의 문서](https://cqrs.files.wordpress.com/2010/11/cqrs_documents.pdf)
>

- 18페이지: 기존 방식
- 24페이지: CRQS, 데이터 모델 분리
- 51페이지: Event Sourcing 도입
- 54페이지: 고수준 뷰

> [CQRS - Marin Fowler](https://martinfowler.com/bliki/CQRS.html)
>

> [CQRS](https://learn.microsoft.com/ko-kr/azure/architecture/patterns/cqrs)
>

비즈니스 로직은 복잡하고 고려해야 할 게 많다. 적절한 도메인 모델, 객체 등 객체지향 기술을 적극 활용해서 복잡성을 통제해야 한다. 모든 게 다 중요하지만, 도메인 지식이 얼마나 잘 반영되었는지, 이를 어떻게 쉽게
검증할 수 있는지, 얼마나 유지보수하기 용이한지 등에 무게를 둬야 한다. 이 부분은 모두 애플리케이션의 상태를 변경하는 일이고, Command에 해당하게 된다.

반면 사용자에게 보여주는 부분은 대부분 “이미 끝난 일”에 해당할 때가 많다. 상태를 변경시키는 일은 없지만, 많은 사용자가 자주 사용하는 부분이기 때문에 성능 등에 무게를 둬야 한다. 이 부분은 Query에
해당하고, 사용자에게 보기 좋은 형태로 미리 준비해 놓는다면 크게 도움이 된다.

이렇게 Write Model과 Read Model을 구분하고, 애플리케이션의 상태가 바뀔 때마다(Command가 실행된 후 거의 바로) Read Model을 업데이트한다면 Command와 Query가 명확히 구분되고
트래픽 등이 비대칭적으로 발생하는 대부분의 서비스에서 도움이 된다.

Write Model은 SSOT(Single Source Of Truth)일 때 유용하지만, Read Model은 상황에 맞게 적절한 기술을 통해 여러 개가 존재해도 된다. Read Model과 관련된 몇 가지
개념을 잠깐 짚고 넘어가자.

- [단일 진실 공급원](https://ko.wikipedia.org/wiki/단일_진실_공급원) : 단일 진실 공급원(SSOT)이란 모든 비즈니스 데이터를 하나의 공간에 저장하는 것을 말합니다. 단일 진실 공급원은
  비즈니스 데이터를 하나의 공간에 저장하면 회사 내의 누구나 이러한 접근 가능 데이터를 바탕으로 중요한 비즈니스 의사 결정을 내릴 수 있을 것이라는 아이디어를 근간으로 합니다. 그래서 중요한 정보가 차단되는 사일로
  현상이 발생하지 않죠. 회사에서 단일 진실 공급원을 사용하면 모두가 동일한 정보에 접근할 수 있어 함께 의기투합해 업무를 원활하게 진행할 수 있습니다. 그리고 하나의 중심 공간에서 파일에 액세스하면 그 누구도
  어떤 파일 버전으로 작업해야 하는지 헛갈릴 일이 없죠.

---

### Materialized View

> [Materialized view](https://learn.microsoft.com/ko-kr/azure/architecture/patterns/materialized-view)
>

> [Materialized view - Wiki](https://ko.wikipedia.org/wiki/구체화_뷰)
>

몇몇 RDBMS는 Query(읽기) 성능을 향상시킬 목적으로 Materialized View를 제공한다. 일반 View와 다르게 물리적으로 저장되고, 주기적으로 동기화가 이뤄진다. 복잡한 인프라 구성 없이 CQRS를
도입하고 싶을 때 유용하다.

쿼리의 결과를 담고 있는 데이터베이스 오브젝트이다. 원거리에 위치한 데이터의 근거리 사본일 수도 있고, 테이블의 줄이나 열의 하부 집합 또는 Join 결과일 수도 있으며 테이블 데이터 집합에 기반한 요약일 수도
있다. 원거리 테이블에 기반한 데이터를 저장하는 구체화 뷰는 스냅샷으로도 부른다.

```sql
 CREATE
MATERIALIZED VIEW MV_MY_VIEW
REFRESH FAST START WITH SYSDATE
   NEXT SYSDATE + 1
     AS
SELECT *
FROM < table_name >;
```

---

### OLTP(온라인 트랜잭션 처리) & OLAP(온라인 분석 처리)

> [OLTP (Online Transaction Processing)](https://www.oracle.com/kr/database/what-is-oltp/)
>

> [OLTP (온라인 트랜잭션 처리)](https://ko.wikipedia.org/wiki/온라인_트랜잭션_처리)
>

> [OLAP (Online Analytical Processing)](https://ko.wikipedia.org/wiki/온라인_분석_처리)
>

> [OLAP / OLTP Difference - AWS](https://aws.amazon.com/ko/compare/the-difference-between-olap-and-oltp/)
>
→ RM의 창시자가 또 해냄 😮

온라인 분석 처리(OLAP)의 기본적인 목적은 집계된 데이터를 분석하는 것이고

온라인 트랜잭션 처리(OLTP)의 기본적인 목적은 데이터베이스 트랜잭션을 처리하는 것입니다.

OLAP 시스템은 보고서를 생성하고, 복잡한 데이터 분석을 수행하며, 추세를 식별하는 데 사용됩니다. 반대로 OLTP 시스템은 주문을 처리하고, 재고를 업데이트하며, 고객 계정을 관리하는 데 사용됩니다.

다른 중요한 차이점으로는 데이터 형식, 데이터 아키텍처, 성능 및 요구 사항이 있습니다. 조직에서 OLAP 또는 OLTP를 사용하는 사례도 살펴보겠습니다.

OLAP(온라인 분석 처리)와 OLTP(온라인 트랜잭션 처리)의 유사점은 다음과 같습니다:

1. **데이터 관리 시스템**: 둘 다 대용량의 데이터를 저장하고 처리하는 데이터베이스 관리 시스템입니다.

2. **IT 인프라 필요**: 효율적이고 안정적인 IT 인프라가 필요하며, 두 시스템 모두 원활한 운영을 위해 이를 필요로 합니다.

3. **데이터 쿼리 및 저장**: 기존 데이터를 쿼리하거나 새 데이터를 저장하는 데 사용됩니다.

4. **의사 결정 지원**: 두 서비스 모두 조직의 데이터 기반 의사 결정을 지원합니다.

그러나 OLAP와 OLTP는 목적, 데이터 형식, 데이터 아키텍처, 성능 및 요구 사항에서 큰 차이가 있습니다. OLAP는 주로 집계된 데이터를 분석하고 보고서를 생성하는 데 사용되며, 데이터 모델은 다차원 큐브
형식입니다. 반면 OLTP는 주로 트랜잭션 처리를 위해 사용되며, 데이터는 일차원적이고 관계형 데이터베이스의 테이블 형식으로 구성됩니다. OLAP는 읽기 작업에 중점을 두고 있으며, OLTP는 쓰기 작업에 중점을
둡니다.

---

### Event Sourcing

> [Event Sourcing](https://martinfowler.com/eaaDev/EventSourcing.html)
>

> [Event-Driven](https://martinfowler.com/articles/201701-event-driven.html)
>

> [이벤트 소싱 패턴](https://learn.microsoft.com/ko-kr/azure/architecture/patterns/event-sourcing)
>

“Capture all changes to an application state as a sequence of events.”

상태의 변화를 이벤트의 연속으로 다루는 방식. 이벤트 자체는 과거형으로 표현하고, 과거의 이력이 모여서 현재의 상태를 계산할 수 있다.

예를 들면 다음과 같은 이벤트가 쌓였다면…

1. 장바구니에 상품 #1을 3개 담았다.
2. 장바구니에 상품 #2를 1개 담았다.
3. 장바구니에서 상품 #1을 1개 뺐다.

현재 상태는 다음과 같다: 장바구니에 상품 #1이 2개, 상품 #2가 1개 담겨 있다.

현재 상태를 빠르게 조회하거나 데이터를 분석하려면 Event Sourcing만으론 너무 어렵다. 따라서 Event Sourcing은 대부분 CQRS와 함께 사용된다.

---
> [우아한형제들 사례 #1](https://youtu.be/BnS6343GTkY)
>

- Microservices 관점에서 설명
- 19분 4초 → EDA
- 28분 10초 → CQRS
- 36분 38초 → 거대한 CQRS

> [우아한형제들 사례 #2](https://youtu.be/38cmd_fYwQk)
>

- Query 관점에서 언급
- 8분 42초 → CQRS

[CQRS and Event Sourcing in Java(페이지 왜캐 무겁지 이거)](https://www.baeldung.com/cqrs-event-sourcing-java)

[스프링캠프 2017 [Day2 A2] : 이벤트 소싱 소개 (이론부)](https://youtu.be/TDhknOIYvw4?si=EEs9WBy9ATfA9NKn)

[스프링캠프 2017 [Day2 A3] : Implementing EventSourcing & CQRS (구현부)](https://www.youtube.com/watch?v=12EGxMB8SR8)

[Coople: Serverless CQRS and Event Sourcing - AWS](https://www.youtube.com/watch?v=WtCfHP6rUAY)

[Event sourcing pattern - AWS](https://docs.aws.amazon.com/prescriptive-guidance/latest/modernization-data-persistence/service-per-team.html)