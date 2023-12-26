# Modules

<details><summary>Bean</summary>
Spring이 관리하는 객체를 Bean이라고 함. 대개는 Spring Bean이라고 콕 짚어서 이야기한다.

예전에는 Bean에 대한 정보를 XML 파일로 써줬는데, 최근에는 Java의 Annotation으로 처리한다. 전자를 진정한 POJO라고 말하는 사람도 있지만, 후자가 너무 편하다. 여러 구현 중 선택하거나 값을
주입하는 건 소스코드 외부(XML 파일, 환경변수 등)를 활용하는 게 좋다.

Bean은 Java Config에서 @Bean 애너테이션을 써서 정의하거나, 해당 클래스에 @Component 애너테이션을 붙여주고 Scan한다(@ComponentScan 사용).

Spring Framework의 "bean" 모듈은 Spring Framework의 핵심 모듈 중 하나이며 Spring Framework의 기본 내장 모듈 입니다. 이 모듈은 Spring의 IoC (Inversion
of Control) 컨테이너와 DI (Dependency Injection) 기능을 제공하며, Spring 애플리케이션에서 객체를 관리하고 의존성을 주입하는 데 중요한 역할을 합니다.

bean 모듈의 핵심 기능은 다음과 같습니다:

1. **IoC 컨테이너**: Spring Framework의 IoC 컨테이너는 객체의 라이프사이클을 관리하고 객체를 생성, 초기화, 구성 및 파괴하는 역할을 합니다. 이를 통해 개발자는 자신이 작성한 클래스를
   일반적인 Java 객체처럼 사용하고 관리할 수 있습니다.

2. **의존성 주입 (DI)**: Spring은 의존성 주입(Dependency Injection)을 지원하여 객체 간의 의존성을 관리하고 주입할 수 있도록 합니다. 이를 통해 객체 간의 결합을 낮추고 테스트
   용이성을 향상시킵니다.

3. **빈 설정**: Spring의 bean 모듈은 XML, JavaConfig 또는 어노테이션을 사용하여 빈 설정을 정의할 수 있습니다. 이 설정을 통해 Spring은 어떤 클래스가 빈으로 생성되어 어떤 빈에
   주입될지를 알 수 있습니다.

4. **AOP (Aspect-Oriented Programming)**: Spring의 bean 모듈은 AOP를 지원하여 관점 지향 프로그래밍을 통해 관심사(Aspects)를 분리하고 적용할 수 있도록 합니다.

bean 모듈은 Spring Framework의 기본 내장 모듈 중 하나이며, Spring 애플리케이션을 개발하거나 구성할 때 가장 중요한 모듈 중 하나입니다. 이 모듈은 Spring Framework의 핵심이며,
Spring Boot 및 다른 Spring 프로젝트와 함께 사용되어 Spring 애플리케이션을 개발하는 데 필수적입니다.

Spring Framework의 "bean" 모듈에서 사용되는 주요 어노테이션들은 다음과 같습니다:

1. `@Component`: 이 어노테이션은 클래스를 Spring의 컴포넌트로 표시합니다. 스캔된 클래스를 Spring 컨테이너에 자동으로 등록하려고 할 때 사용합니다. `@Component` 어노테이션을 기반으로
   하는 다른 어노테이션들도 있으며, 이들은 특정 유형의 컴포넌트를 정의하는 데 사용됩니다.

```java

@Component
public class MyComponent {
    // 클래스 내용
}
```

2. `@Service`: 이 어노테이션은 `@Component`의 특수한 경우로, 비즈니스 로직을 수행하는 서비스 클래스를 정의할 때 사용합니다.

```java

@Service
public class MyService {
    // 서비스 로직
}
```

3. `@Repository`: 이 어노테이션은 데이터 액세스 객체 (DAO) 클래스를 정의할 때 사용됩니다. 주로 데이터베이스와 상호작용하는 클래스를 표시하는 데 사용됩니다.

```java

@Repository
public class MyRepository {
    // 데이터 액세스 로직
}
```

4. `@Controller`: Spring MVC 웹 애플리케이션에서 컨트롤러 클래스를 정의할 때 사용합니다. HTTP 요청을 처리하는 데 사용됩니다.

```java

@Controller
public class MyController {
    // 컨트롤러 로직
}
```

5. `@Configuration`: 이 어노테이션은 Java 기반의 설정 클래스를 정의할 때 사용합니다. 이 클래스는 Spring 빈 설정 정보를 포함하며, XML 설정 대신 Java 코드로 Spring 빈을
   구성할 수 있게 합니다.

```java

@Configuration
public class AppConfig {
    // 빈 설정 메서드들
}
```

6. `@Bean`: `@Configuration` 클래스 내에서 사용되며, 빈을 정의하는 메서드에 이 어노테이션을 추가합니다.

```java

@Configuration
public class AppConfig {
    @Bean
    public MyBean myBean() {
        return new MyBean();
    }
}
```

7. `@Autowired`: 이 어노테이션은 의존성 주입(DI)을 수행할 때 사용됩니다. Spring은 해당 필드, 생성자 또는 메서드의 매개변수에 알맞은 빈을 자동으로 주입합니다.

```java

@Service
public class MyService {
    private MyRepository myRepository;

    @Autowired
    public MyService(MyRepository myRepository) {
        this.myRepository = myRepository;
    }
}
```

이러한 어노테이션들은 Spring Framework의 bean 모듈을 사용하여 빈을 정의하고 관리하는 데 사용됩니다. 이를 통해 Spring 애플리케이션을 보다 간단하게 구성하고 의존성 주입을 수행할 수 있습니다.

</details>

<details><summary>Web</summary>
Spring Web 모듈은 Spring Framework의 일부로, 웹 애플리케이션을 개발하기 위한 핵심 기능과 도구를 제공하는 모듈입니다. 이 모듈은 주로 Spring MVC(모델-뷰-컨트롤러)와 Spring Webflux와 같은 웹 애플리케이션 프레임워크를 지원합니다.

Spring Web 모듈의 주요 특징과 기능은 다음과 같습니다:

1. **Spring MVC**: Spring MVC는 전통적인 웹 애플리케이션을 개발하는 데 사용되는 프레임워크로, 모델-뷰-컨트롤러 아키텍처를 기반으로 합니다. 이를 통해 웹 요청을 처리하고 응답을 생성하는 데
   사용됩니다. Spring MVC는 컨트롤러, 모델, 뷰와 같은 요소들을 구조화하고 이들을 쉽게 확장하고 사용자 지정할 수 있도록 지원합니다.

2. **RESTful 웹 서비스 지원**: Spring Web 모듈은 RESTful 웹 서비스를 빌드하고 제공하기 위한 다양한 도구와 어노테이션을 제공합니다. 이를 통해 REST API를 구축하고 사용할 수
   있습니다.

3. **데이터 바인딩 및 유효성 검사**: Spring Web은 HTTP 요청의 데이터를 자바 객체로 바인딩하고, 유효성 검사(validation)를 수행하는 기능을 제공합니다. 이를 통해 입력 데이터를 처리하고
   검증할 수 있습니다.

4. **보안 및 인증 지원**: Spring Security와 통합하여 보안 및 사용자 인증을 지원합니다. 이를 통해 웹 애플리케이션의 보안 요구사항을 쉽게 충족시킬 수 있습니다.

5. **웹소켓 지원**: Spring Web 모듈은 웹소켓 프로토콜을 지원하며, 실시간 웹 애플리케이션을 구축할 때 사용할 수 있습니다.

6. **통합 테스트 지원**: Spring Web 모듈은 웹 애플리케이션의 통합 테스트를 위한 기능을 제공하여 웹 애플리케이션의 동작을 검증할 수 있도록 도와줍니다.

Spring Web 모듈은 Spring Framework의 핵심 부분 중 하나로, 웹 애플리케이션을 빠르고 효과적으로 개발할 수 있도록 도와주는 강력한 도구와 기능을 제공합니다. 이 모듈은 Spring Boot와
함께 사용되어 개발자가 빠르게 웹 애플리케이션을 구축하고 관리할 수 있도록 도와줍니다.

- @Controller: 컨트롤러 클래스를 정의하는 어노테이션으로, 웹 요청을 처리하는 역할을 합니다.
- @RequestMapping (또는 @GetMapping, @PostMapping, @PutMapping, @DeleteMapping): 요청 URL과 메서드를 매핑하는 데 사용되는 어노테이션입니다.
- @RequestParam: URL 파라미터를 메서드의 매개변수로 바인딩하는 데 사용되는 어노테이션입니다.
- @PathVariable: URL 경로 변수를 메서드의 매개변수로 바인딩하는 데 사용되는 어노테이션입니다.
- @ResponseBody: 메서드가 JSON 또는 XML과 같은 데이터를 직접 반환하는 데 사용되는 어노테이션입니다.
- @ModelAttribute: 모델 객체를 생성하고 뷰에 전달하는 데 사용되는 어노테이션입니다.
- @Valid: 객체 유효성 검사를 활성화하는 데 사용되는 어노테이션입니다.
- @ModelAttribute: multipart/form-data 형태의 파라미터를 받아 객체로 사용하고 싶을 때 "가장 적절한" 생성자를 찾아 객체를 생성 및
  초기화. https://velog.io/@jmjmjmz732002/Springboot-ModelAttribute
  붙여진 @Controller 클래스에 지원

```java

@RestController// 해당 클래스가 RESTful 웹 서비스에서 JSON 또는 XML 형식의 응답을 생성하는 역할을 수행
@RequestMapping("images")
public class ImageController {
    @PostMapping(consumes = MediaType.MULTIPART_FORM_DATA_VALUE)
    // 컨트롤러 메서드 또는 클래스에 적용되며, 요청 URL과 요청 메서드(GET, POST, PUT, DELETE 등)를 매핑시킬 때 사용
    @ResponseStatus(HttpStatus.CREATED)
    public String create(
            @ModelAttribute UploadImageDto imageDto
    ) {
        MultipartFile multipartFile = imageDto.image();

        if (multipartFile == null || multipartFile.isEmpty()) {
            return "No image";
        }
        // 저장 로직은 분리 권장
        String id = TSID.Factory.getTsid().toString();
        File file = new File("data/" + id + ".jpg");

        try (OutputStream outputStream = new FileOutputStream(file)) {
            byte[] content = multipartFile.getBytes();
            outputStream.write(content);
        }

        return multipartFile.getOriginalFilename();
    }

    @PutMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    public void updateComment(@PathVariable("id") String id, @RequestParam("postId") String postId, @RequestBody CommentDto commentDto) {
        CommentDto foundCommentDto = getFoundCommentDto(id, postId);
        foundCommentDto.setContent(commentDto.getContent());
    }
}
```

</details>
<details><summary>UUID / ULID / CLID</summary>
모두 고유한 식별자를 나타내는 약어입니다. 이러한 식별자들은 대부분의 경우 데이터베이스 레코드, 세션, 객체, 트랜잭션 등을 고유하게 식별하기 위해 사용됩니다.

"UUID," "ULID," 그리고 "CLID"는 모두 고유한 식별자를 나타내는 약어이며, 이러한 식별자들은 다양한 컨텍스트에서 사용됩니다. 다음은 각각의 식별자에 대한 간단한 설명입니다:

1. [UUID](https://docs.oracle.com/javase/8/docs/api/java/util/UUID.html) (Universally Unique Identifier):
   f17cc4e3-208e-482b-9e06-480f5bfdd02b
    - UUID는 "Universally Unique Identifier"의 약어로, 범용적으로 사용되는 고유한 식별자입니다.
    - UUID는 128비트나 36자리의 16진수 문자열로 표현되며, 주로 **랜덤**하게 생성됩니다.
    - 주로 네트워크에서 고유성이 요구되는 경우나 여러 시스템 간에 데이터를 동기화하는 데 사용됩니다.

2. [ULID](https://github.com/f4b6a3/ulid-creator) (Universally Unique Lexicographically **Sortable** Identifier):
   01F48G3NW6S8TV0GQYX9DZCN8G
    - ULID는 "Universally Unique Lexicographically Sortable Identifier"의 약어로, 시간 순서대로 정렬 가능하면서 고유한 식별자입니다.
    - ULID는 128비트의 값으로, 타임스탬프와 임의의 값의 조합으로 생성됩니다.
    - 주로 분산 시스템에서 시간 순서대로 **정렬**되는 고유한 식별자가 필요한 경우에 사용됩니다.

3. [TSID](https://github.com/f4b6a3/tsid-creator) (Time-Sorted Unique Identifiers) : 38352658573940766
    - Twitter's [Snowflake](https://github.com/twitter-archive/snowflake/tree/snowflake-2010)
      와 [ULID Spec](https://github.com/ulid/spec)의 아이디어를 구현.
    - 날짜 및 시간 정보를 기반으로 정렬 가능한 고유 식별자를 생성하는 기능을 제공합니다.

이러한 식별자들은 각각의 목적과 사용 사례에 따라 다양한 구현 및 규칙을 가질 수 있으며, 고유성이 보장되고 정확한 용도에 맞게 사용되어야 합니다.

</details>
<details><summary>BeanFactory (Interface)</summary>

> The root interface for accessing a Spring bean container.
>
> This interface is implemented by objects that hold a number of bean definitions, each uniquely identified by a String
> name.
>
> Spring 빈 컨테이너에 액세스하기 위한 루트 인터페이스입니다.
> 이 인터페이스는 각각 문자열 이름으로 고유하게 식별되는 여러 빈 정의를 보유하는 객체에 의해 구현됩니다.
>
최소한의 IoC Container
> Depending on the bean definition, the factory will return either an independent instance of a contained object (the
> Prototype design pattern),
>
>  빈 정의에 따라, 팩토리는 포함된 객체의 독립적인 인스턴스(
> 프로토타입 디자인 패턴)
>
>or a single shared instance (a superior alternative to the Singleton design pattern, in which the instance is a
> singleton in the scope of the factory)
>
> 또는 단일 공유 인스턴스 (싱글톤 디자인 패턴의 우수한 대안으로, 인스턴스가 싱글톤 범위의
> 싱글톤 디자인 패턴보다 우수한 대안).

빈 팩토리 구현은 가능한 한 표준 빈 라이프사이클 인터페이스를 지원해야 합니다.

전체 초기화 메서드 세트와 그 표준 순서는 다음과 같습니다:

1. postProcessBeforeDestruction methods of DestructionAwareBeanPostProcessors
2. DisposableBean's destroy
3. a custom destroy-method definition1. BeanNameAware's setBeanName
2. BeanClassLoaderAware's setBeanClassLoader
3. BeanFactoryAware's setBeanFactory
4. EnvironmentAware's setEnvironment
5. EmbeddedValueResolverAware's setEmbeddedValueResolver
6. ResourceLoaderAware's setResourceLoader (애플리케이션 컨텍스트에서 실행할 때만 적용 가능)
7. ApplicationEventPublisherAware's setApplicationEventPublisher (애플리케이션 컨텍스트에서 실행할 때만 적용 가능)
8. MessageSourceAware's setMessageSource  (애플리케이션 컨텍스트에서 실행할 때만 적용 가능)
9. ApplicationContextAware's setApplicationContext  (애플리케이션 컨텍스트에서 실행할 때만 적용 가능)
10. ServletContextAware's setServletContext  (애플리케이션 컨텍스트에서 실행할 때만 적용 가능)
11. postProcessBeforeInitialization methods of BeanPostProcessors
12. InitializingBean's afterPropertiesSet
13. a custom init-method definition
14. postProcessAfterInitialization methods of BeanPostProcessors

빈 팩토리의 종료 시에는 다음과 같은 라이프사이클 메서드가 적용됩니다:

1. postProcessBeforeDestruction methods of DestructionAwareBeanPostProcessors
2. DisposableBean's destroy
3. a custom destroy-method definition

> [BeanFactory](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/BeanFactory.html)
>

> [BeanDefinitionRegistry](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/support/BeanDefinitionRegistry.html)
>

> [GenericBeanDefinition](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/beans/factory/support/GenericBeanDefinition.html)
>

> [Given-When-Then](https://github.com/ahastudio/til/blob/main/blog/2018/12-08-given-when-then.md)
>
</details>
<details><summary>Spring jdbcTemplate</summary>

> [JdbcTemplate](https://docs.spring.io/spring-framework/docs/6.0.4/javadoc-api/org/springframework/jdbc/core/JdbcTemplate.html)
>

> [Data Access](https://docs.spring.io/spring-framework/docs/6.0.4/reference/html/data-access.html#jdbc-core)
>

> [Spring Initializr](https://start.spring.io/)
>
⇒ JDBC API, Spring Boot DevTools 의존성 추가.

간단한 실험은 Spring Boot Command Line Runner 또는 JUnit을 사용할 수 있다.

JUnit을 강력히 추천하지만, 여기서는 Command Line Runner를 사용하는 법을 간단히 알아보자.

```java

@Component
public class AppRunner implements CommandLineRunner {
    private final JdbcTemplate jdbcTemplate;

    public AppRunner(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    @Override
    public void run(String... args) throws Exception {
        String sql = "SELECT name FROM people";

        jdbcTemplate.query(sql, resultSet -> {
            while (resultSet.next()) {
                String name = resultSet.getString("name");
                System.out.println(name);
            }
        });
    }

}

```

간단한 SQL 쿼리 실행. PreparedStatement 적용이 쉽다. 문자열을 포함하는지 확인할 때는 LIKE와 %를 사용한다.

```java
String sql="SELECT name FROM people WHERE name LIKE ?";

        jdbcTemplate.query(sql,resultSet->{
        String name=resultSet.getString("name");

        System.out.println(name);

        },"%우");
```

Query(SELECT)가 아니라 Command(INSERT, UPDATE, DELETE)를 실행할 때는 update 메서드 사용.

```java
String sql="""
INSERT INTO people (name, age, gender) VALUES (?, ?, ?)
""";

        jdbcTemplate.update(sql,"홍길동",15,"남");
```

TransactionTemplate을 써서 트랜잭션을 관리할 수 있다.

```java

@Component
public class AppRunner implements CommandLineRunner {
    private JdbcTemplate jdbcTemplate;
    private TransactionTemplate transactionTemplate;

    public AppRunner(JdbcTemplate jdbcTemplate,
                     TransactionTemplate transactionTemplate) {
        this.jdbcTemplate = jdbcTemplate;
        this.transactionTemplate = transactionTemplate;
    }

    @Override
    public void run(String... args) throws Exception {
        transactionTemplate.execute(status -> {
            String sql = """
                    INSERT INTO people(name, age, gender) VALUES(?, ?, ?)
                    """;

            jdbcTemplate.update(sql, "홍길동", 15, "남");

            // 이런 식으로 예외를 던지면 전부 없던 일로 만들 수 있다.
            // throw new RuntimeException("FAIL!");

            return null;
        });
    }

}
```

</details>
<details><summary>Cloudinary</summary>

Cloudinary는 클라우드로 이미지와 비디오를 관리할 수 있게 해주는 SaaS(Software as a Service)다.
일정 수준까지는 무료로 사용할 수 있고, 썸네일 이미지 등을 즉각적으로 생성 및 캐시할 수 있다

> [Cloudinary](https://cloudinary.com/)
>

> [Cloudinary Java SDK](https://cloudinary.com/documentation/java_integration)
>

> [Cloudinary](https://en.wikipedia.org/wiki/Cloudinary)
>

```yaml
cloudinary:
  url: ${CLOUDINARY_URL:cloudinary://key:secret@cloud}
```

application.yml 에서 환경변수를 호출하도록 설정 (기본값 cloudinary://key:secret@cloud으로)

```java

@Component
public class ImageStorage {
    private final Cloudinary cloudinary;

    public ImageStorage(
            @Value("${cloudinary.url}") String cloudinaryUrl
    ) {
        cloudinary = new Cloudinary(cloudinaryUrl);
        cloudinary.config.secure = true;
    }

    public String save(byte[] bytes) {
        String id = TSID.Factory.getTsid().toString();

        Map options = ObjectUtils.asMap(
                "public_id", "test/" + id
        );

        try {
            Map result = cloudinary.uploader().upload(bytes, options);
            return result.get("secure_url").toString();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }
}
```

업로드한 이미지를 다음과 같이 즉각적으로 변형해서 사용할 수 있다.

- 원본 이미지: https://res.cloudinary.com/demo/image/upload/w_800/lady.png
- 썸네일로 만들기: https://res.cloudinary.com/demo/image/upload/c_fill,w_400,h_400/lady.png
- 얼굴로 썸네일 만들기: https://res.cloudinary.com/demo/image/upload/c_crop,g_face,w_400,h_400/lady.png
- 원형으로 자르기: https://res.cloudinary.com/demo/image/upload/c_crop,g_face,w_400,h_400/r_max/lady.png
- 블러 효과 추가: https://res.cloudinary.com/demo/image/upload/c_crop,g_face,w_400,h_400/r_max/e_blur:500/lady.png

> 자세한 건 문서 참고: https://cloudinary.com/documentation/image_transformations
>
</details>