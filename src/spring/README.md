# Spring Framework

> 좋은 [**객체 지향**](../architecture/oop.md) 어플리케이션을 개발할 수 있게 도와주는 프레임워크
>
> 다형성이 가장 중요하다!
> 스프링은 다형성을 극대화해서 이용할 수 있게 도와준다.
>
> 스프링에서 이야기하는 제어의 역전(IoC), 의존관계 주입(DI)는 다형성을 활용해 역할과 구현을 편리하게 다룰 수 있도록 지원한다.
>
> 스프링을 사용하면 마치 레고 블럭 조립하듯이! 공연 무대의 배우를 선택하듯이! 구현을 편리하게 변경할 수 있다.
>
>-스프링 핵심 원리 -기본편 (김영한)
>

Java Spring은 Java 기반의 웹 응용 프로그램 및 서비스를 개발하기 위한 오픈 소스 프레임워크입니다. Spring은 애플리케이션 개발을 단순화하고 확장 가능성과 유지 관리성을 향상시키는 목적으로 만들어진
프레임워크로, 대규모 엔터프라이즈 애플리케이션 개발에 매우 인기가 있습니다. (이전의 EJB의 대체재)

## 특징과 구성요소

Spring Framework는 엔터프라이즈 애플리케이션을 빠르고 효율적으로 개발하기 위한 강력한 도구로, Java 애플리케이션 개발에 매우 널리 사용됩니다. Spring Boot를 통해 간단하게 Spring
애플리케이션을 시작하고 빠르게 개발할 수 있습니다.

- 의존성 주입(Dependency Injection): Spring은 제어의 역전(IoC, Inversion of Control) 컨테이너를 제공하여 객체의 생성 및 관리를 단순화하고 의존성 주입을 통해 객체 간의
  결합을 낮춥니다. 이것은 테스트 가능한 코드 작성 및 코드의 재사용을 용이하게 합니다.

- AOP (Aspect-Oriented Programming): Spring은 관점 지향 프로그래밍(AOP)을 지원합니다. AOP를 통해 애플리케이션에서 횡단 관심사(concern)를 분리하여 코드 중복을 줄이고
  관심사에 대한 모듈화를 허용합니다.

- 웹 개발 지원: Spring은 Spring MVC를 포함한 다양한 웹 개발 모듈을 제공하여 웹 애플리케이션을 빠르게 개발할 수 있습니다. Spring Boot는 Spring 기반의 마이크로서비스 및 웹
  애플리케이션을
  구축하는 데 도움을 주는 도구입니다.

- 데이터 액세스/통합: Spring은 JDBC, ORM 프레임워크 (예: Hibernate, JPA), NoSQL 데이터베이스 및 메시징 시스템과 통합할 수 있는 다양한 모듈을 제공합니다.

- 보안: Spring Security는 애플리케이션의 보안 요구사항을 충족시키기 위한 강력한 기능을 제공합니다.

- 배치 프레임워크: Spring Batch는 대용량 데이터 처리 및 배치 작업을 위한 프레임워크를 제공합니다.

- 테스트 지원: Spring은 JUnit, TestNG 등과 통합하여 테스트 주도 개발 및 단위 테스트를 쉽게 할 수 있도록 도와줍니다.

- 클라우드 지원: Spring Cloud 프로젝트는 마이크로서비스 아키텍처를 구축하고 클라우드 환경에서 애플리케이션을 배포하는 데 사용됩니다.

- 커뮤니티 및 생태계: Spring은 크고 활발한 개발자 커뮤니티와 생태계를 갖고 있어 다양한 문제에 대한 지원과 라이브러리가 풍부합니다.

---

#### 내부 동작방식 관련 읽으면 좋은 글

[Spring Boot 대체 어떻게 실행되는걸까 ?](https://code-run.tistory.com/5)