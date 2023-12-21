# Spring Security

---
![img.png](../../resources/spring_security_interceptor.png)
![img.png](../../resources/spring_security_filter.png)

### HandlerInterceptor

HandlerInterceptors는 Spring MVC 프레임워크의 일부이며 DispatcherServlet과 컨트롤러 사이에 위치합니다.

요청이 컨트롤러에 도달하기 전,뷰가 렌더링 전,후에 요청을 가로챌 수 있습니다.

핸들러 인터셉터는 DispatcherServlet과 컨트롤러 간의 요청을 가로챕니다. 이 작업은 Spring MVC 프레임워크 내에서 수행되며, 핸들러 및 ModelAndView 객체에 대한 액세스를 제공합니다.
이렇게 하면 중복을 줄이고 다음과 같은 보다 세분화된 기능을 사용할 수 있습니다:

- 애플리케이션 로깅과 같은 cross-cutting 문제 처리
- 세부적인 권한 검사
- Spring 컨텍스트 또는 모델 조작

### FilterChain

필터는 Spring 프레임워크가 아닌 웹 서버의 일부입니다. 들어오는 요청의 경우 필터를 사용하여 Spring 프레임워크의 요청을 조작하고 차단할 수도 있습니다.

필터를 사용하여 request를 조작하고 서블릿으로의 도달을 차단할 수도 있습니다. 그 반대로 응답이 클라이언트에 전달되지 않도록 차단할 수도 있습니다.

Spring Security는 인증 및 권한 부여에 필터를 사용하는 좋은 예입니다. Spring Security를 구성하려면 DelegatingFilterProxy라는 단일 필터만 추가하면 됩니다.

그런 다음 Spring Security는 들어오고 나가는 모든 트래픽을 가로챌 수 있습니다. 이것이 Spring Security가 Spring MVC 외부에서 사용될 수 있는 이유입니다.

필터는 요청이 DispatcherServlet에 도달하기 전에 요청을 가로채기 때문에 다음과 같은 세분화된 작업에 이상적입니다:

- Authentication
- Logging and auditing
- Image and data compression
- Any functionality we want to be decoupled from Spring MVC

### 결론

핵심은 필터를 사용하면 요청이 컨트롤러에 도달하기 전, 그리고 Spring MVC 외부에서 요청을 조작할 수 있다는 것입니다.

반면에, 핸들러인터셉터는 애플리케이션별 크로스 커팅 문제를 해결하기에 좋은 곳입니다.
대상 핸들러 및 ModelAndView 객체에 대한 액세스를 제공함으로써 보다 세분화된 제어가 가능합니다.

---
출처:

https://www.baeldung.com/spring-mvc-handlerinterceptor-vs-filter