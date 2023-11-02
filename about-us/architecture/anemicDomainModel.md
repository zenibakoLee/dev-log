# 무기력한 도메인 모델” 안티패턴

Anemic Domain Model

> [“무기력한 도메인 모델” 안티패턴 원문](https://martinfowler.com/bliki/AnemicDomainModel.html)
>
> > [“무기력한 도메인 모델” 안티패턴 번역글](https://m.blog.naver.com/muchine98/220304821784)
>

**"무기력한 도메인 모델"은 도메인 모델링에서 도메인 객체가 상태만을 가지고 비즈니스 로직을 거의 가지고 있지 않는 상태를 나타냅니다. </br>이러한 상태 중심의 모델링은 객체지향 설계 원칙에 어긋나며,
안티패턴으로 간주됩니다.**

마틴 파울러(Martin Fowler)의 "Anemic Domain Model(무기력한 도메인 모델 안티패턴)" 개념은 소프트웨어 개발에서 도메인 모델링에 관한 중요한 주제 중 하나입니다. 이 개념은 소프트웨어
시스템에서
도메인 모델의 부족한
특징을 지칭합니다. 아래는 "Anemic Domain Model"의 주요 내용을 요약한 것입니다:

1. 도메인 모델:
    - 도메인 모델은 소프트웨어 시스템이 다루는 비즈니스 도메인의 핵심 개념, 규칙 및 엔터티를 반영하는 객체의 모음입니다.

2. Anemic Domain Model:
    - Anemic Domain Model은 도메인 객체가 데이터와 데이터 처리 로직을 분리하는 방식을 의미합니다.
    - 즉, 객체는 데이터를 저장하고 데이터를 조작하는 메서드만 가지고 있으며, 비즈니스 규칙이나 도메인 로직은 다른 계층(보통 서비스 계층)에 위치한 별도의 클래스에서 처리됩니다.

3. 문제점:
    - **저질의 객체 캡슐화**: Anemic Domain Model은 객체지향 프로그래밍의 장점을 활용하지 못하고, 객체의 캡슐화를 약화시킵니다.
    - **비즈니스 로직 분산**: 비즈니스 도메인 로직이 분산되어 유지보수가 어렵고 이해하기 어려워집니다.
    - **응집성 부족**: 객체 간 협력과 의미 있는 메서드가 부족해 객체 모델의 가독성과 확장성이 떨어집니다.

4. 해결 방법:
    - Anemic Domain Model을 해결하기 위해 도메인 객체에 비즈니스 로직을 추가하고, 객체의 상태와 행위를 통합하여 객체의 응집성을 강화합니다.
    - 객체가 자체적으로 데이터와 동작을 캡슐화하며, 객체 자체가 자신의 상태를 관리하고 도메인 로직을 실행합니다.

"Anemic Domain Model"의 주요 메시지는 객체지향 프로그래밍의 가치를 최대한 활용하고, 도메인 모델을 더 강력하게 만들기 위해 객체의 책임을 확장하고 응집성을 강화하는 것입니다. 이를 통해 코드의
가독성과 유지보수성을 향상시키고, 비즈니스 도메인을 더 명확하게 반영할 수 있습니다.
