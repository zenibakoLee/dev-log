# REST

REST(Representational State Transfer)는 웹 서비스 및 네트워크 응용 프로그램을 위한 아키텍처 스타일이며, 월드 와이드 웹(World Wide Web)의 아키텍처를 기반으로 합니다.
REST는 리소스를 표현하고 해당 리소스에 대한 상태를 전달하기 위한 간단하면서도 확장 가능한 방법을 제공합니다. 아래는 REST의 주요 특징과 개념을 설명한 것입니다:

1. **리소스(Resource)**: REST는 모든 것을 리소스로 간주합니다. 리소스는 웹에서 접근할 수 있는 모든 것을 나타냅니다. 이러한 리소스는 고유한 식별자(URI)를 가지며 상태 정보를 가질 수
   있습니다.

2. **상태 없음(Statelessness)**: REST 서비스는 클라이언트와 서버 간의 통신에서 상태 정보를 관리하지 않습니다. 각 요청은 충분한 정보를 포함하고 있어야 하며, 서버는 이러한 정보를 사용하여
   요청을 처리합니다. 이러한 특징은 서버의 확장성을 향상시키고 클라이언트와 서버 사이의 독립성을 유지합니다.

3. **HTTP 메서드**: RESTful 서비스는 HTTP 메서드(GET, POST, PUT, DELETE 등)를 사용하여 리소스에 대한 작업을 정의합니다. 예를 들어, HTTP GET 메서드는 리소스를
   검색하고, POST 메서드는 새 리소스를 생성하며, PUT 메서드는 리소스를 업데이트하고, DELETE 메서드는 리소스를 삭제합니다.

4. **URI(Uniform Resource Identifier)**: 모든 리소스는 고유한 URI를 가지며, 이 URI를 사용하여 리소스를 식별합니다. 예를
   들어, "https://example.com/products/123"는 ID가 123인 제품 리소스를 나타냅니다.

5. **표현(Representation)**: 리소스의 상태는 표현을 통해 클라이언트에게 전달됩니다. 이 표현은 다양한 형식으로 제공될 수 있으며, 주로 JSON 또는 XML 형식을 사용합니다.

6. **자기 서술성(Self-descriptive)**: RESTful 서비스는 메시지 자체에 충분한 정보를 포함하므로 클라이언트는 요청 및 응답 메시지를 이해할 수 있습니다. 이는 메시지 헤더 및 상태 코드를
   활용하여 구현됩니다.

7. **계층 구조(Layered System)**: RESTful 아키텍처는 계층 구조를 허용하며, 클라이언트와 서버 사이에 중간 계층(프록시 서버, 캐시 등)를 두어 시스템을 확장하고 보안을 강화할 수 있습니다.

REST는 웹 서비스, API 디자인, 마이크로서비스 아키텍처 및 분산 시스템에서 널리 사용되는 아키텍처 스타일로, 간결하며 확장 가능한 방법으로 리소스와 그 상태를 관리하고 공유합니다. RESTful 서비스는
HTTP 프로토콜을 기반으로 하여 쉽게 이해하고 사용할 수 있으며, 클라이언트와 서버 간의 통신을 단순화하고 표준화합니다.


<details><summary>필딩 제약 조건</summary>

[박사 학위 논문 "Architectural Styles and the Design of Network-based Software Architectures"](https://ics.uci.edu/~fielding/pubs/dissertation/rest_arch_style.htm)

로이 필딩(Roy Fielding)은 이 논문에서 아키텍처 스타일을 처음으로 제시하고 정의한 중요한 작품입니다.<br>
이 논문은 웹 아키텍처의 기본 원리를 설명하고 REST의 필요성 및 제약 조건을 논의합니다.<br>

필딩은 이 논문에서 RESTful 서비스의 확장성과 상호 운용성을 향상을 목적으로하여 설계할 때 준수해야 하는 필딩 제약 조건을 제안합니다. 이러한 제약 조건을 충족하면 서비스는 "RESTful"하다고 간주됩니다.
아래는 필딩 제약 조건을 요약한 것입니다:

1. **Client-Server Separation (클라이언트-서버 분리)**: 클라이언트와 서버는 독립적으로 발전하며, 각각의 역할을 명확히 해야 합니다. 이는 확장성과 각 구성 요소의 독립성을 증가시킵니다.

2. **Statelessness (상태 없음)**: 서비스는 클라이언트의 상태 정보를 유지하지 않으며, 각 요청은 모든 필요한 정보를 포함해야 합니다. 이로써 서버는 불변성을 유지하고 확장성을 개선합니다.

3. **Cacheability (캐싱 가능)**: 서버 응답은 캐싱될 수 있어야 하며, 이를 통해 중간 계층 캐시를 활용하여 성능을 개선할 수 있습니다.

4. **Layered System (계층 구조)**: 시스템은 다중 계층으로 나뉘며, 각 계층은 서비스의 기능을 확장하거나 보안을 제공합니다.

5. **Uniform Interface (일관된 인터페이스)**: 서비스는 일관된 인터페이스를 제공해야 하며, 이는 리소스를 식별하고 상호 작용하는 데 사용되는 표준 메서드 및 표현을 의미합니다.

6. **Code on Demand (선택적 코드 전송)**: 서비스는 클라이언트로 코드를 전송할 수 있어야 하지만 이 기능은 선택 사항입니다. 대부분의 RESTful 서비스는 이 제약 조건을 준수하지 않으며,
   클라이언트는 서버에서 전송된 코드를 실행하지 않습니다.

필딩 제약 조건은 RESTful 서비스를 구현하고 평가하는 데 사용되며, 웹 서비스 및 API 디자인에서 중요한 원칙 중 하나입니다. 이러한 제약 조건을 따르면 서비스는 확장 가능하고 유지 관리하기 쉽게 되며,
다양한 클라이언트와 서버 간의 상호 운용성을 제공합니다.
</details>

<details><summary>Uniform Interface</summary>

1. REST 아키텍처 스타일을 다른 네트워크 기반 스타일과 구별하는 중요한 특징은 구성 요소 간의 일관된(Uniform) 인터페이스에 중점을 둔다.
2. 일반성(generality) 원칙을 구성 요소 인터페이스에 적용함으로써 전체 시스템 아키텍처가 단순화되고 상호 작용의 가시성이 향상된다.
3. 구현은 제공하는 서비스로부터 독립적이며, 독립적인 진화를 장려한다.
4. 그러나 대가는 일관된 인터페이스는 효율성을 저하시키며, 정보가 특정 응용프로그램 요구 사항에 특화된 형식이 아닌 표준 형식으로 전송되기 때문이다.
5. REST 인터페이스는 대량의 하이퍼미디어 데이터 전송에 효율적으로 설계되었으며, 웹의 일반적인 경우를 최적화하지만 웹의 특별한 경우에는 최적이 아니다.<br>
6. **필딩 제약 조건**
    1. Four Interface Constraints

        - Identification of Resources → URI 등으로 리소스를 식별할 수 있다.

        - Manipulation of Resources through Representations → 표현으로 리소스를 조작한다.

        - Self-descriptive Messages → 메시지는 자기서술적이기 때문에 여러 레이어에서 처리/변환 가능하다.

            - JSON 같은 범용 포맷을 작게 사용하면 어떻게 해석해야 하는지 알 수 없기 때문에 자기서술적이기 어렵다. 뒤에서 다룰 MIME 타입으로 설명한다면, application/json이 아니라
              application/dns+json 같은 타입을 써야 한다.

            - REST API를 이야기할 때 까다로운 부분 중 하나.

        - Hypermedia as the Engine of Application State → 줄여서 HATEOAS라고 부른다. REST API를 이야기할 때 까다로운 부분 중 하나.

    2. 아키텍처 요소(5.2)에서 리소스와 표현을 구분
        - Resource → 추상. ⇒ 특정 시점의 스냅샷이 아니라, 모든 시간에 통용되는 엔티티 집합. 객체지향에서 말하는 Entity라고 생각하면 편하다. <객체지향의 사실과 오해>의 표현을 빌린다면,
          “앨리스”라는 리소스는 키가 커지던 작아지던 항상 “앨리스”다.

        - Representation → Data + Metadata + Meta-metadata… ⇒ 사실상 HTTP 메시지라고 보면 됨. 예를 들어, 리소스를 어떻게 조작할 것인가는 HTTP Method로
          표현하게 되고, 리소스를 무엇으로 조작할 것인가는 Content-Type과 Body로 표현하게 된다.

    3. URI 파트(6.2)에서 리소스에 대해 다시 강조

        - “The resource is not the storage object. The resource is not a mechanism that the server uses to handle the
          storage object.”

        - 리소스, 표현, 실제 데이터 등은 전부 구분된다.

    4. 아키텍처 데이터 뷰(5.3.3)에서 HATEOAS에 대해 언급.
        - 마지막 문단의 첫 문장: “The model application is therefore an **engine** that moves from one state to the next by
          examining and **choosing** from among the alternative **state transitions** in the current set of *
          *representations**.”

            - 이렇게 하려면 표현에 선택 가능한 상태 전환이 포함돼야 한다.

            - 이게 바로 하이퍼미디어 링크.

        - 대부분은 효율 문제로 표현에 링크를 넣지 않고, 클라이언트 개발자가 API 문서를 활용해 처리한다. 표현에서 상태 전환을 선택하는 게 아니라, API 문서를 참조해서 상태 전환을 강제하는 것.
        - [Richardson Maturity Model](https://martinfowler.com/articles/richardsonMaturityModel.html)
            - <RESTful Web API>의 공저자인 레오나르드 리처드슨은 Hypermedia Control(대표적인 게 바로 링크)을 강조.
            - 성숙한 REST라면 표현에 하이퍼미디어 컨트롤(링크)이 포함되어야 한다.

</details>
<details><summary>학습 자료</summary>
### Web

- https://www.youtube.com/watch?v=RP_f5dMoHFc
- [https://blog.npcode.com/2017/03/02/바쁜-개발자들을-위한-rest-논문-요약/](https://blog.npcode.com/2017/03/02/%eb%b0%94%ec%81%9c-%ea%b0%9c%eb%b0%9c%ec%9e%90%eb%93%a4%ec%9d%84-%ec%9c%84%ed%95%9c-rest-%eb%85%bc%eb%ac%b8-%ec%9a%94%ec%95%bd/)
- [https://blog.npcode.com/2017/04/03/rest의-representation이란-무엇인가/](https://blog.npcode.com/2017/04/03/rest%ec%9d%98-representation%ec%9d%b4%eb%9e%80-%eb%ac%b4%ec%97%87%ec%9d%b8%ea%b0%80/)
- [카카오 REST api 문서](https://developers.kakao.com/docs/latest/ko/kakaologin/rest-api)

### 서적

- [RESTful Web API](http://aladin.kr/p/zGUKk)
- [웹 API 디자인](http://aladin.kr/p/byC7Y)

</details>