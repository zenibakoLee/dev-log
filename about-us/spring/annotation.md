# Annotations

Spring Framework에서 어노테이션(Annotation)은 애플리케이션의 구성, 동작 및 관계를 정의하는 데 사용되는 메타데이터의 일종입니다.Spring Annotations은 주로 스프링 컴포넌트(빈)를
정의하거나 빈의 의존성을 주입하는 데 사용됩니다. Spring Annotations은 스프링의 IoC(Inversion of Control) 및 DI(Dependency Injection) 기능을 활용하기 위해
사용됩니다.<br>
어노테이션은 자바 클래스, 메서드, 필드 및 다른 요소에 주석으로 적용됩니다.

Spring Framework에서 사용되는 어노테이션 중 일부는 다음과 같습니다:

- @Component: 스프링 컨테이너가 해당 클래스를 빈(Bean)으로 관리하도록 표시하는 어노테이션입니다. @Component를 확장한 다른 어노테이션들도 있으며, 예를 들어 @Service,
  @Repository, @Controller 등이 있습니다.

- @Autowired: 의존성 주입을 수행할 때 사용하는 어노테이션입니다. 스프링은 해당 필드, 생성자 또는 메서드 파라미터에 필요한 빈을 자동으로 주입합니다.

- @Configuration: 스프링 설정 클래스임을 나타내는 어노테이션으로, XML 기반 설정 대신 자바 클래스를 사용하여 애플리케이션을 구성할 때 주로 사용됩니다.

- @Bean: 스프링 컨테이너가 해당 메서드의 반환 값을 빈으로 관리하도록 표시하는 어노테이션입니다. 주로 @Configuration 클래스 내에서 사용됩니다.

- @RequestMapping: Spring MVC 컨트롤러 클래스에서 HTTP 요청과 매핑할 메서드를 정의하는 데 사용됩니다. 요청 경로, HTTP 메소드 및 다른 매핑 관련 정보를 지정할 수 있습니다.

- @Service: 비즈니스 로직을 수행하는 서비스 클래스임을 나타내는 어노테이션입니다. 주로 서비스 계층의 클래스에 사용됩니다.

- @Repository: 데이터베이스 연동을 위한 데이터 액세스 객체(DAO)임을 나타내는 어노테이션입니다. 주로 데이터 액세스 계층의 클래스에 사용됩니다.

- @Scope: 빈의 범위를 정의하는데 사용되며, 기본적으로 싱글톤 범위로 설정됩니다. 다른 범위(프로토타입, 세션, 요청 등)을 지정할 수 있습니다.

- @Value: 프로퍼티 값을 주입하는 데 사용되며, 주로 외부 설정 파일(properties 또는 YAML)에서 값을 가져와 필드 또는 메서드 파라미터에 주입합니다.

- @Transactional: 데이터베이스 트랜잭션을 처리하는 데 사용되며, 메서드 레벨 또는 클래스 레벨에서 트랜잭션 관리를 활성화합니다.

이러한 어노테이션들은 Spring Framework를 사용하여 애플리케이션을 개발할 때 구성, 의존성 주입, 웹 요청 처리, 데이터 액세스, 트랜잭션 관리 등 다양한 측면에서 중요한 역할을 합니다. 어노테이션을
사용함으로써 XML 기반 설정보다 더 간결하고 가독성이 높은 코드를 작성할 수 있습니다.

- [@RestController](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RestController.html):
  해당 클래스가 RESTful 웹 서비스에서 JSON 또는 XML 형식의 응답을 생성하는 역할을 수행한다는 것을 나타냅니다.<br>
  이 애너테이션은 @Controller 및 @ResponseBody를 결합한 것으로, 컨트롤러 메서드에서 반환하는 값은 HTTP 응답으로 직접 변환되어 클라이언트에게 전송됩니다.
    - [@Controller](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/stereotype/Controller.html):
      해당 클래스가 웹 애플리케이션의 컨트롤러 역할을 한다는 것을 나타냅니다.
    - [@ResponseBody](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/ResponseBody.html):
      컨트롤러 메서드에 적용되며, 해당 메서드가 HTTP 응답의 본문(body) 부분을 직접 반환한다는 것을 나타냅니다.<br>메서드의 반환 값이 JSON, XML 또는 다른 형식의 데이터로 직렬화되어
      클라이언트에게 전송됩니다
- [@GetMapping](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/GetMapping.html):
  HTTP GET 요청에 응답하는 컨트롤러 메서드에 적용됩니다.
    - [@RequestMapping](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/bind/annotation/RequestMapping.html):
      컨트롤러 메서드 또는 클래스에 적용되며, 요청 URL과 요청 메서드(GET, POST, PUT, DELETE 등)를 매핑시킬 때 사용됩니다.