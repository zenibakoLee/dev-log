# Test

우리가 프로그래밍을 하는 이유: 문제(특히 비즈니스 문제)를 해결하기 위해서.

문제가 잘 해결됐는지 어떻게 확신할 수 있을까?
문제를 잘 정의하고, 문제가 해결된 모습을 미리 그려보면 절반 정도는 확신할 수 있게 된다.

---

## TDD (Test Driven Development)

테스트 코드를 먼저 작성하고, 구현하고, 그 과정에서 배운 걸 활용하는 개발 방법.

> Portland Pattern Repository (Original Wiki)
> - [Test Driven Development](http://wiki.c2.com/?TestDrivenDevelopment)
> - [Portland Pattern Repository](https://ko.wikipedia.org/wiki/포틀랜드_패턴_리포지토리)
>

> [TDDBE (Test Driven Development: By Example)](http://aladin.kr/p/dGXdZ)
>

> [TDD FAQ](https://github.com/ahastudio/til/blob/main/blog/2016/12-03-tdd-faq.md)
>

> [Java+JUnit TDD 실습](https://www.youtube.com/playlist?list=PLbdtsbZUwdeRirBYnWrMSvKYS4CcmXCeU)
>

> [우아한형제들 TDD 세미나 (낡은 코드란 점에 주의!)](https://youtu.be/-hqiLswBiY8)
>

> [참고 자료](https://github.com/ahastudio/til/blob/main/agile/test-driven-development.md)
>

**TDD Cycle**:

1. Red → 실패하는 테스트 코드를 작성.
2. Green → 최대한 빨리 테스트를 통과시킴.
3. Refactor → 리팩터링. TDD에서 가장 중요하지만, 많은 사람들이 간과하고 있는 부분.

Red-Green 정도는 Newton’s Method로 제곱근을 구할 때 살짝 경험했다. 리팩터링에 집중해서 연습해 보자.