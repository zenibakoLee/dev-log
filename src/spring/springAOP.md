# Spring AOP (Aspect Oriented Programming)

> 흩어진 코드를 한 곳으로 모아
>
> - 백기선(예제로 배우는 스프링 입문)
>

ex) 각 메소드별 실행 시간을 측정해야 한다. 메소드의 갯수는 어림잡아 10000개.</br>
심지어 특정한 기준으로 측정할 메소드를 구분해야 한다. 어쩌면 좋아

- 측정 대상 기능은 핵심 관심 사항(core concern)
- 시간 측정 로직은 공통 관심 사항(cross-cutting-concern)

이 둘을 분리하는 것이 AOP

원하는 곳에 공통 관심 사항(cross-cutting-concern) 적용

AOP 구현 방법

1. 컴파일 시점에 클래스 파일에 공통 관심 사항이 추가되도록 (**AspectJ**)
2. 바이트코드 조작 : .java=>compile=>.class=> **classLoader**가 읽어서 메모리 올릴 때 추가(AspectJ)
3. 프록시 패턴(스프링 AOP): 기존의 코드를 건드리지 않고, 변경,적용된 객체를 만든다. https://refactoring.guru/ko/design-patterns/proxy

```java

@Aspect
@Compnent // bean 등록
public class TimeTraceAop {

    @Around("execution(*hello.hellospring..*(..))") // 공통 관심 사항 적용 범위
    public Object execute(ProceedingJointPoint joinPoint) throws Throwable {
        StopWatch stopWatch = new StopWatch();
        stopWatch.start();
        try {
            Object result = joingPoint.proceed();
        } finally {
            stopWatch.stop();
            System.out.println(stopWatch.prettyPrint());
        }
    }
}
```

---

### Spring AOP작동방식

AOP가 적용이 필요한 Bean인 경우

컨테이너에 가짜(Proxy) 스프링 빈을 등록. (해당 빈이 주입되는 생성자에서 주입되는 빈.getClass 찍어보면 확인 가능.)

가짜 스프링빈의 joinPoint.proceed() 종료 후

실제 빈 호출

![](https://docs.spring.io/spring-framework/reference/_images/aop-proxy-call.png)

---
https://docs.spring.io/spring-framework/reference/core/aop.html