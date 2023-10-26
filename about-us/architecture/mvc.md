# MVC pattern

Model-View-Controller 아키텍처 패턴

- [Model View Controller](https://martinfowler.com/eaaCatalog/modelViewController.html)
- [GUI Architectures](https://martinfowler.com/eaaDev/uiArchs.html)

1. View → 표현
2. Controller → 입력에 대한 처리
3. Model → 그외의 모든 것 (사실상 도메인 모델, DB 한정 아님)
    1. 관심사의 분리 → MVC가 추구하는 것: 흔히 말하는 UI와 비즈니스 로직의 분리.
    2. View에서 참조하는 표현/데이터 → 원래 MVC와 다르게, 일반적으로 쓰이는 것.
        1. Rails는 Active Record 아키텍처 패턴을 통해 Model과 DB를 긴밀하게 연결시켰고, Rails의 성공은 다른 곳에도 많은 영향을 끼쳤다.
            - [Active Record](https://martinfowler.com/eaaCatalog/activeRecord.html)
            - [ActiveRecord considered harmful](https://steveklabnik.com/writing/active-record-considered-harmful)
        2. Spring Web MVC는 Map과 유사한 Model 인터페이스를 제공.
            1. https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/ui/package-summary.html
            2. 애플리케이션 전체가 아니라, 웹과 관련된 부분(UI Layer)에서만 MVC 개념을 활용할 수 있도록 도움. 관심사의 분리를 더 심화할 기회 제공.

최근에는 더 복잡한 걸 잘 다룰 수 있는 더 커다란 아키텍처가 필요하다는 걸 알게 됨. 웹에선 Controller만으로 충분함. 또는 Controller 대신 Handler란 개념을 사용하면 됨(Spring
WebFlux 등). Hexagonal Architecture를 적극 활용한다면 그냥 Input Adapter 중 하나가 됨(참고로, DB는 Output Adapter).

([Get Your Hands Dirty on Clean Architecture의 사례](https://github.com/thombergs/buckpal/blob/master/src/main/java/io/reflectoring/buckpal/account/adapter/in/web/SendMoneyController.java))

Spring에서 Controller 예제

```java

@RestController
public class WelcomeController {
    @GetMapping("/")
    public String home() {
        return "Hello, world!";
    }
}
```