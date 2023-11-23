# Test tool

<details><summary>JUnit</summary>

https://ko.wikipedia.org/wiki/JUnit

JUnit은 자바 언어용으로 개발된 테스트 프레임워크로, 자바 애플리케이션에서 **단위(Unit)** 테스트를 쉽게 작성하고 실행할 수 있도록 도와주는 도구입니다.

JUnit은 개발자들이 코드의 품질을 향상시키고 소프트웨어를 안정적으로 유지보수할 수 있도록 돕는데 사용됩니다. 다음은 JUnit의 주요 특징과 사용법에 대한 간략한 설명입니다:

### JUnit의 주요 특징:

1. **애노테이션 기반:**
    - JUnit은 애노테이션을 사용하여 테스트 메소드를 식별하고 실행합니다. `@Test` 애노테이션은 테스트 메소드를 정의할 때 사용됩니다.

2. **단정문(assertions):**
    - 테스트 결과를 평가하기 위해 JUnit은 단정문(assertions)을 제공합니다. 예상 결과와 실제 결과를 비교하여 테스트를 평가할 수 있습니다.

3. **테스트 픽스처(Test Fixture):**
    - `@Before` 및 `@After` 애노테이션을 사용하여 각각 테스트 메소드가 실행되기 전과 후에 필요한 설정 또는 정리 작업을 수행할 수 있습니다.

4. **테스트 스위트(Test Suite):**
    - 여러 테스트 케이스를 그룹화하여 한 번에 실행할 수 있는 테스트 스위트를 만들 수 있습니다.

5. **파라미터화된 테스트(Parameterized Tests):**
    - `@RunWith(Parameterized.class)`를 사용하여 동일한 테스트를 여러 다른 입력 값으로 반복적으로 실행할 수 있습니다.

6. **테스트 결과 리포트:**
    - 테스트 실행 후에는 테스트 결과를 보고하는 리포트를 생성할 수 있습니다.

### JUnit 사용법:

1. **테스트 클래스 생성:**
    - JUnit 테스트 클래스는 테스트 메소드를 포함하는 일반적인 자바 클래스입니다. 테스트 클래스는 `@Test` 애노테이션으로 표시된 테스트 메소드를 포함합니다.

    ```java
    import org.junit.Test;

    public class MyTest {
        @Test
        public void myTestMethod() {
            // 테스트 로직 작성
        }
    }
    ```

2. **단정문 활용:**
    - `assertEquals`, `assertTrue`, `assertFalse` 등의 단정문을 사용하여 예상 결과와 실제 결과를 비교하여 테스트를 수행합니다.

    ```java
    import static org.junit.Assert.assertEquals;

    public class MyTest {
        @Test
        public void testAddition() {
            int result = Calculator.add(2, 3);
            assertEquals(5, result);
        }
    }
    ```

3. **테스트 실행:**
    - IDE나 빌드 도구(예: Maven, Gradle)에서 JUnit 테스트를 실행할 수 있습니다. 테스트 결과는 성공, 실패, 또는 에러로 표시됩니다.

JUnit은 단위 테스트의 자동화와 반복적인 테스트 수행을 통해 개발자가 소프트웨어의 안정성과 신뢰성을 유지할 수 있도록 도와줍니다. 이는 개발 프로세스를 안정화하고 소프트웨어를 효과적으로 유지보수하는 데 도움이
됩니다.
</details>
<details><summary>Mockito</summary>

[Mockito 위키 - 한글버전](https://github.com/mockito/mockito/wiki/Mockito-features-in-Korean)

</details>