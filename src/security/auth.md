# Authentication vs Authorization

### Authentication(인증)

![img.png](../../resources/security_authentication.png)


> “컴퓨터 보안에서 인증은 로그인 요청 등을 통해 통신 상에서 보내는 사람의 디지털 정체성을 확인하는 시도의 과정이다.”
>

인증은 거칠게 말하면 “나(사용자)는 누구인가?”라는 질문에 답하는 것이라고 할 수 있다. 오프라인이라면 신분증 등을 통해 내가 누구인지 밝힌다면, 온라인에서는 Username과 Password, OTP 같은 인증
시스템을 활용하게 된다.

인증은 여러 층위에서 정의할 수 있는데, 사용자 경험 관점에서 인증은 로그인 단계를 의미한다. 즉, 한번 인증(로그인)을 하고 나면, 세션이 유지되는 동안에는 다시 인증(로그인) 절차를 반복하지 않게 된다. 보안이
엄격한 장소에 입장할 때 입구에서 신분 확인을 하고, 그 다음엔 다시 신분 확인 절차를 반복하지 않는 것과 마찬가지다.

하지만 기술적으로 보면 조금 달라진다. HTTP는 Stateless를 전제로 한다. 각 요청은 개별적이고, 매 요청마다 “나는 누구인가?”라는 질문에 답을 줘야 한다. 즉, 매 요청마다 인증 절차가 있게 된다.
그리고 이 절차는 클라이언트와 서버가 각각 다르게 취급하게 되는데, 이는 인가(Authorization)에서 다시 다루겠다.

<details><summary>Cookie & LocalStorage</summary>

#### Cookie

웹사이트에 의해 유저의 컴퓨터에 놓여지는 작은 텍스트 파일

Cookies는 최대 4KB의 용량을 가진 매우 작은 양의 데이터입니다.
Cookies는 사이트에서 방문한 페이지를 저장하거나 유저의 로그인 정보를 저장하는 등 다양한 방법으로 사용됨. 그리고 문자열만 저장할 수 있다는 제한이 있다.

많은 보안 웹사이트들은 로그인을 한 후 Cookies를 사용해 유저의 신원을 확인하여 모든 페이지에서 재인증을 거치지않아도 됨. Cookies의 또 다른 용도는 사이트에서 제한된 인터넷 사용 기록을 기반으로
사용자 경험을 개선한다.

---

#### LocalStorage

HTML5가 나온 이후, cookies의 많은 사용 방법들은 LocalStorage의 사용으로 대체되었다.

LocalStorage는 cookies보다 더 많은 장점이 있기 때문

- cookies와는 달리 모든 HTTP 요청에서 데이터를 주고받을 필요가 없다. HTTP 요청에서 데이터를 주고받지 않고 LocalStorage를 이용하면 클라이언트와 서버간의 전체 트래픽과 낭비되는
  대역폭의 양을 줄일 수 있다.
- 데이터가 유저의 로컬 디스크에 저장되어 있으면 인터넷이 끊어져도 데이터가 삭제되거나 지워지지 않는다.
- LocalStorage는 최대 5MB의 정보를 저장할 수 있습니다. 이것은 cookies가 보유할 수 있는 4KB보다 훨씬 더 많다.
- LocalStorage의 만료 조건은 persistent cookies처럼 동작. Javascript 코드를 통해 삭제하지 않으면 데이터는 자동으로 삭제되지 않는다. 이 방식은더 오랜 시간동안 저장해야하는 큰
  데이터에 유용.
- LocalStorage를 사용하면 문자열 뿐만아니라 javascript의 primitives와 object도 저장할 수 있다.

</details>


---

### Authorization(인가)

![img.png](../../resources/security_authorization.png)

> 리소스 접근 권한의 제한적 부여
>

인증이 증명의 문제라면, 인가는 허가의 문제다. 엄격히 말하면 둘은 구분되지만, 일반적으로 인증과 인가는 함께 사용된다. 예를 들어 쇼핑몰 서비스를 만든다고 하면, 관리자만 모든 주문에 접근하고 처리할 수 있게 해야
한다. 이를 위해서 현재 사용자가 관리자임을 증명해야 하고(인증), 관리자가 아닌 사용자는 관리자 페이지에 접근할 수 없게 해야 한다(인가).

인증과 인가를 사용자, 클라이언트, 서버 입장에서 살펴보면 다음과 같다:

1. 사용자 입장에서는 로그인 페이지를 통해 인증 절차를 밟게 되고, 그 이후는 접근이 거부될 때만 인가 과정이 있었음을 알 수 있게 된다.
2. 클라이언트 입장에서는 로그인 과정을 통해 인증 결과로 토큰 등을 얻게 되고(세션을 활용한다면 Session ID를 통해 간접적으로 전달), 이를 쿠키나 localStorage 등으로 관리하면서 매 요청마다
   서버로 전달한다. 즉, 매 요청마다 인가 과정이 있다고 가정한다.
3. 서버 입장에서는 모든 요청에 대해 인증 작업이 수행된다. 로그인 등을 통해 발행한 토큰 등을 매번 확인해 사용자가 누구인지 알아내고(인증), 해당 사용자가 올바른 접근을 했는지 확인해서 허용 또는 금지한다(
   인가).

우리가 개발하는 서버(백엔드)에선 이렇게 모든 요청에 대해 인증과 인가 작업을 수행해야 한다. Spring Web MVC로 개발할 때는 Controller로 요청이 전달되기 전에 HandlerInterceptor로
우리가 원하는 코드를 먼저 실행할 수 있고, Spring Security를 사용하면 SecurityFilterChain을 통해 우리가 원하는 코드를 먼저 실행할 수 있다(여기서는 Spring Security에
집중한다).

- [HandlerInterceptor](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/web/servlet/HandlerInterceptor.html)
- [SecurityFilterChain](https://docs.spring.io/spring-security/reference/servlet/architecture.html#servlet-securityfilterchain)

---
출처:
https://erwinousy.medium.com/%EC%BF%A0%ED%82%A4-vs-%EB%A1%9C%EC%BB%AC%EC%8A%A4%ED%86%A0%EB%A6%AC%EC%A7%80-%EC%B0%A8%EC%9D%B4%EC%A0%90%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%BC%EA%B9%8C-28b8db2ca7b2
