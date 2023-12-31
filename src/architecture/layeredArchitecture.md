# Layered Architecture

레이어드 아키텍처(Layered Architecture)는 소프트웨어 개발에서 사용되는 설계 패턴으로, 응용 프로그램의 코드베이스를 명확하게 정의된 계층으로 구조화하는 것을 목표로 합니다.

인간이 짜는 프로그램은 인간의 인지 한계가 존재하기 때문에, 계층 분리를 통해 각 파일,클래스별 **관심사의 분리**가 필요하다.

이 패턴은 크고 복잡한 소프트웨어 시스템에서 모듈화, 관심사의 분리 및 유지보수성을 달성하는 데 도움을 줍니다.

일반적으로 레이어드 아키텍처는 다음과 같은 주요 계층으로 구성되며, 이러한 계층 간의 특정한 역할과 책임이
할당됩니다: [PresentationDomainDataLayering - Martin Fowler](https://martinfowler.com/bliki/PresentationDomainDataLayering.html)

![](https://martinfowler.com/bliki/images/presentationDomainDataLayering/all_basic.png)

- 표현 계층 (Presentation Layer): 사용자와의 상호작용을 처리하는 계층으로, 주로 사용자 인터페이스 (UI)와 관련됩니다. 이 계층은 사용자 입력을 받고 출력을 생성하며 비즈니스 로직 계층으로
  요청을 전달합니다.

- 비즈니스 로직 계층 (Business Logic Layer): 핵심 비즈니스 논리 및 데이터 처리(기능)를 수행하는 계층입니다. 비즈니스 규칙과 데이터 처리 로직을 포함하며 데이터베이스와 상호작용하여 데이터를
  처리합니다. ex. Controller package

- 데이터 액세스 계층 (Data Access Layer): 데이터베이스와의 상호작용을 담당하는 계층으로, 데이터베이스에 데이터를 저장하고 검색하는 작업을 처리합니다. 이 계층은 주로 데이터베이스 연결, 쿼리 작성
  및 실행을 다룹니다.

![](https://martinfowler.com/bliki/images/presentationDomainDataLayering/all_more.png)
종속성은 일반적으로 레이어 스택을 통해 위에서 아래로 실행됩니다. 표현은 도메인에 따라 달라지며 도메인은 데이터 소스에 따라 달라집니다. <br>일반적인 변형은 도메인과 데이터 소스 레이어 사이에 매퍼를 도입하여
도메인이 데이터 소스에 의존하지 않도록 항목을 정렬하는 것입니다 . 이 접근 방식을 종종 육각형 아키텍처라고 합니다

레이어드 아키텍처의 주요 목표는 각 계층 간의 결합도를 낮추고, 코드의 재사용성과 유지보수성을 향상시키는
것입니다. 또한 각 계층은 다른 계층으로부터 독립적으로 테스트가 가능하며, 변경 사항이 한 계층에서
다른 계층으로 영향을 미치지 않도록 하는 것이 중요합니다.
<details><summary>결합도</summary>

결합도(Coupling)는 소프트웨어 시스템의 구성 요소들 사이의 상호 의존성 정도를 나타내는 소프트웨어 공학의 개념입니다. 높은 결합도는 시스템을 유연하게 유지하는 데 어려움을 초래할 수 있으며, **낮은 결합도
**는 시스템의 유지 및 확장을 용이하게 만듭니다.

다음은 결합도의 몇 가지 주요 유형과 설명입니다:

1. 느슨한 결합도 (Low Coupling):

- 느슨한 결합도는 모듈 또는 구성 요소 간의 상호 의존성이 낮은 상태를 나타냅니다.
- 각 모듈이 독립적으로 작동하며, 하나의 모듈의 변경이 다른 모듈에 영향을 미치지 않습니다.
- 느슨한 결합도는 시스템의 유지보수, 테스트 및 확장을 쉽게 만듭니다.

2. 강한 결합도 (High Coupling):

- 강한 결합도는 모듈 또는 구성 요소 간의 상호 의존성이 높은 상태를 나타냅니다.
- 하나의 모듈의 변경이 다른 모듈에 영향을 미치며, 이로 인해 시스템의 유지보수와 변경이 어려워집니다.
- 강한 결합도는 코드를 이해하기 어렵게 하고 버그를 찾기 어렵게 만들 수 있습니다.

3. 논리적 결합도 (Logical Coupling):

- 논리적 결합도는 모듈 간의 논리적인 상호 의존성을 나타냅니다.
- 코드 내의 데이터 및 로직을 공유하거나 모듈 간의 긴밀한 관계가 있는 경우 논리적 결합도가 높아집니다.

4. 물리적 결합도 (Physical Coupling):

- 물리적 결합도는 모듈이 서로 직접 연결되어 있거나 서로에게 종속적인 상태를 나타냅니다.
- 두 모듈이 같은 데이터 구조를 공유하거나 하나의 모듈이 다른 모듈을 직접 호출하는 경우 물리적 결합도가 높아집니다.

결합도를 관리하고 최소화하는 것은 소프트웨어 시스템의 설계 및 유지보수에 중요한 역할을 합니다. 느슨한 결합도를 유지하면 코드의 재사용성이 높아지며, 시스템의 유연성과 확장성이 향상됩니다. 이로써 코드를 이해하기
쉽고 유지보수하기 편리한 소프트웨어 시스템을 구축할 수 있습니다.

> 의존성 주입(Dependency Injection, DI)은 결합도(Coupling)를 관리하고 느슨한 결합도(Loose Coupling)를 촉진하기 위한 소프트웨어 디자인 패턴 및 원칙 중 하나입니다.
>
</details>
<details><summary>응집도</summary>

응집도(Cohesion)는 모듈 내의 요소들이 서로 관련되어 있는 정도를 나타냅니다.

즉, 모듈이 단일 목표나 책임을 가지고 그 목표를 달성하는 정도를 의미합니다. **높은 응집도**는 모듈 내의 요소들이 밀접하게 관련되어 있고 함께 동작하는 것을 나타내며, 모듈의 목표를 달성하는 데 집중되어 있는
것을 의미합니다. 반대로, 낮은 응집도는 모듈 내의 요소들이 서로 관련성이 적고 서로 다른 작업을 수행하는 것을 나타내며, 모듈이 여러 목표를 혼합하거나 너무 많은 책임을 가지고 있는 경우를 나타냅니다.

응집도는 주로 다음과 같은 유형으로 나뉩니다:

1. 기능 응집도 (Functional Cohesion):

- 모듈 내의 요소들이 동일한 작업 또는 기능을 수행하는 경우입니다. 모듈은 특정 작업을 수행하며 모든 요소가 그 작업을 지원합니다.

2. 순차 응집도 (Sequential Cohesion):

- 모듈 내의 요소들이 순차적으로 실행되는 작업을 수행하는 경우입니다. 요소는 일련의 단계를 따라 실행되며 이러한 단계가 특정 작업의 일부로 연결됩니다.

3. 통신 응집도 (Communicational Cohesion):

- 모듈 내의 요소들이 동일한 데이터나 정보를 공유하고 데이터 처리에 관련된 작업을 수행하는 경우입니다. 요소들은 데이터 교환을 통해 상호작용합니다.

4. 순차 응집도 (Procedural Cohesion):

- 모듈 내의 요소들이 서로 연결된 특정 프로세스를 수행하는 경우입니다. 이러한 프로세스는 일반적으로 순차적으로 실행되며 특정 목표를 달성하는 데 사용됩니다.

5. 시간 응집도 (Temporal Cohesion):

- 모듈 내의 요소들이 동일한 시간 동안 실행되는 작업을 수행하는 경우입니다. 일반적으로 이러한 요소들은 특정 이벤트나 시간 조건 아래에서 동작합니다.

좋은 소프트웨어 설계는 높은 응집도를 갖는 모듈을 강조합니다. 높은 응집도를 가진 모듈은 단일 목표에 집중하고 코드를 이해하거나 유지보수하기 쉽게 만듭니다. 또한 응집도를 고려하여 모듈을 설계하면 모듈 간의 결합도를
관리하기 쉬워집니다.

```java
// 높은 응집도 예제: Calculator 모듈

public class Calculator {
    public int add(int a, int b) {
        return a + b;
    }

    public int subtract(int a, int b) {
        return a - b;
    }

    public int multiply(int a, int b) {
        return a * b;
    }

    public int divide(int a, int b) {
        if (b != 0) {
            return a / b;
        } else {
            throw new ArithmeticException("Division by zero");
        }
    }
}

```

</details>

- controllers: 웹 요청을 처리하는 컨트롤러 클래스 파일이 위치하는 곳입니다.
- services: 비즈니스 로직을 처리하는 서비스 클래스 파일이 위치하는 곳입니다.
- dtos: 데이터 전송 객체(Data Transfer Object) 클래스 파일이 위치하는 곳으로, 데이터를 전송하기 위한 객체를 정의합니다.

```lua
my-spring-boot-project/
|-- src/
|   |-- main/
|   |   |-- java/
|   |   |   |-- com/
|   |   |   |   |-- example/
|   |   |   |   |   |-- myproject/
|   |   |   |   |   |   |-- controllers/ zxczxc
|   |   |   |   |   |   |   |-- MyController.java
|   |   |   |   |   |   |-- services/
|   |   |   |   |   |   |   |-- MyService.java
|   |   |   |   |   |   |-- dtos/
|   |   |   |   |   |   |   |-- MyDTO.java
|   |   |-- resources/
|   |   |   |-- application.properties
|   |   |   |-- static/
|   |   |   |-- templates/
|   |-- test/
|   |   |-- java/
|   |   |   |-- com/
|   |   |   |   |-- example/
|   |   |   |   |   |-- myproject/
|   |   |   |   |   |   |-- controllers/
|   |   |   |   |   |   |   |-- MyControllerTest.java
|   |   |   |   |   |   |-- services/
|   |   |   |   |   |   |   |-- MyServiceTest.java
|   |   |   |   |   |   |-- dtos/
|   |   |   |   |   |   |   |-- MyDTOTest.java
|-- pom.xml
```