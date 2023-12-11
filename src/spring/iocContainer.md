# IoC Container

Inversion of Control. 제어 역전.

Hollywood Principle이라고도 한다: “Don't call us, we'll call you”.

IoC는 프레임워크의 공통적인 특징.

> *“saying that these lightweight containers are special because they use **inversion of control** is like
saying **my car is special because it has wheels**.”*
>
> -Martin Fowler (https://martinfowler.com/articles/injection.html)


Spring 등이 EJB를 비판하면서 POJO, IoC Container를 이야기했고, 이 전쟁을 끝내기 위해 Martin Fowler가 새로운 용어를 제안함.

프레임워크에서 제공하는 보일러 플레이트를 이용하면,
프로그램은 프레임워크의 주도하에 실행되게된다. => 결과적으로 휴먼 에러를 줄이며, 특정한 철학(ex.oop)를 잘 준수하는 프로그램을 만들 수 있다.

해당 방식의 구현 중 하나로,
D.I 가 있다. Dependency Injection.

원래는 생성자 내부에서 타 클래스 생성자를 직접 호출해, 지역 변수에 인스턴스를 할당했다면,
파라미터를 통해 해당 클래스 인스턴스를 받도록 하면, Spring Bean에 등록되어 있는 클래스의 인스턴스, 즉 Bean이 자동으로 주입된다.

Bean 등록 방법

1. XML 파일에서
1. 루트 어플리케이션 파일에서 @Bean 어노테이션 이용
2. Component 관련 어노테이션을 클래스 상단에 직접 코딩

의존성 주입(Dependency Injection, DI)은 소프트웨어 개발에서 중요한 디자인 패턴 중 하나로, 다음과 같은 여러 가지 장점을 제공한다:

1. 느슨한 결합(Loose Coupling): DI는 컴포넌트 간의 결합도를 줄여줍니다. 이는 코드를 더 유연하고 변경하기 쉽게 만들어줍니다. 의존성이 직접 생성되지 않고 주입되므로, 의존하는 객체를 교체하거나
   업그레이드할 때 다른 부분에 미치는 영향을 최소화할 수 있습니다.

2. 테스트 용이성: DI는 테스트 주도 개발(Test-Driven Development, TDD) 및 단위 테스트를 수행하기 용이하게 만들어줍니다. 의존성을 모의 객체(Mock)나 대체 구현체로 쉽게 주입하여
   테스트 케이스를 작성할 수 있습니다.

3. 재사용성: 의존성을 주입하면 동일한 컴포넌트를 여러 곳에서 재사용할 수 있습니다. 이로 인해 코드 중복을 줄이고 개발 시간을 절약할 수 있습니다.

4. 가독성과 유지보수성: DI를 사용하면 코드의 의존성이 명시적으로 드러나기 때문에 코드의 가독성이 향상됩니다. 또한 코드 수정 및 유지보수가 쉬워집니다.

5. 의존성 관리: DI 컨테이너를 사용하면 객체의 라이프사이클 및 의존성을 관리하는 것이 용이해집니다. 이로 인해 객체를 생성하고 제거하는 작업을 자동화할 수 있으며, 메모리 누수 및 다른 문제를 방지할 수
   있습니다.

6. 확장성: 새로운 의존성을 추가하거나 변경할 때 DI를 사용하면 더 쉽게 처리할 수 있습니다. 새로운 의존성을 주입하면 기존 코드를 수정하지 않고도 시스템의 동작을 변경할 수 있습니다.

7. 코드의 품질 향상: DI는 단일 책임 원칙(Single Responsibility Principle)을 지키기 용이하게 하며, 객체 지향 디자인 원칙을 준수하도록 돕습니다.

이러한 장점으로 인해 DI는 소프트웨어 개발에서 인기 있는 디자인 패턴 중 하나이며, 코드의 유연성, 품질, 테스트 용이성, 유지보수성 등을 향상시키는 데 도움을 줍니다.

Factory 패턴을 사용하면 객체를 생성하는 코드를 별도로 분리하고, SRP를 준수하여 클래스가 단일 책임을 갖도록 유지할 수 있습니다. 이로써 SRP를 준수하고 객체 생성 코드를 관리할 책임을 Factory에
위임할 수 있습니다.
Factory 패턴은 SRP를 준수하면서도 객체 생성과 관련된 코드를 단일 책임으로 유지할 수 있는 효과적인 방법 중 하나이며, 코드의 가독성과 유지보수성을 향상시킬 수 있습니다.

---

https://martinfowler.com/bliki/InversionOfControl.html
