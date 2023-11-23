# springframework.boot.test

Package org.springframework.boot.test.autoconfigure.web.servlet

<details><summary>@WebMvcTest</summary>

## Annotation Interface WebMvcTest

> [Spring docs link](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/autoconfigure/web/servlet/WebMvcTest.html)
>
>Since: 1.4.0
>
>Author: Phillip Webb, Artsiom Yudovin
>
Spring MVC 컴포넌트 에만 초점을 맞춘 Spring MVC 테스트에 사용할 수 있는 어노테이션입니다.

=> MVC를 위한 테스트.
웹에서 테스트하기 힘든 컨트롤러를 테스트하는 데 적합.

이 어노테이션을 사용하면 전체 자동 구성이 비활성화되고 대신 MVC 테스트와 관련된 구성만 적용됩니다.(다음과 같은 내용만 스캔하도록 제한)

=> @SpringBootTest 어노테이션보다 가볍게 테스트할 수 있음.

(i.e. @Controller, @ControllerAdvice, @JsonComponent, Converter/GenericConverter, Filter, WebMvcConfigurer and
HandlerMethodArgumentResolver beans but not @Component, @Service or @Repository beans).

읿반적으로 @Controller 빈에 필요한 collaborator를 생성하는 데 @MockBean or @Import 를 같이 사용합니다.
</details>