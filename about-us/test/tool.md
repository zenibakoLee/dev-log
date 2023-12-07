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

- given()
- verify()

</details>
<details><summary>MockMVC</summary>

`MockMvc`는 스프링 프레임워크에서 제공하는 테스트용 클래스로, 컨트롤러 테스트를 위해 가상의 HTTP 요청을 생성하고, 컨트롤러의 응답을 검증하는 데 사용됩니다. 이는 실제 웹 서버를 띄우지 않고도 스프링
MVC 컨트롤러를 테스트할 수 있도록 도와줍니다.

일반적으로 스프링 부트 애플리케이션의 통합 테스트에서 `MockMvc`를 사용하여 다음과 같은 작업을 수행할 수 있습니다:

1. **HTTP 요청 생성**: `MockMvcRequestBuilders` 클래스를 사용하여 GET, POST, PUT, DELETE 등 다양한 HTTP 요청을 생성할 수 있습니다.

2. **HTTP 응답 검증**: `ResultMatcher`를 사용하여 HTTP 응답의 상태 코드, 헤더, 본문 등을 검증할 수 있습니다.

3. **모델 및 뷰 검증**: 모델 객체와 뷰에 대한 검증을 수행할 수 있습니다.

4. **세션 및 쿠키 검증**: HTTP 세션 및 쿠키와 관련된 검증을 수행할 수 있습니다.

아래는 `MockMvc` 사용 예제입니다:

```java
// 이 두 애노테이션은 스프링 부트 애플리케이션의 통합 테스트를 위한 환경을 설정하고, MockMvc를 자동으로 구성합니다.
@SpringBootTest
@AutoConfigureMockMvc
class PostControllerTest {
    // MockMvc 인스턴스를 주입받아서 사용할 수 있도록 하는 멤버 변수입니다. 이를 통해 컨트롤러를 테스트할 수 있습니다.
    @Autowired
    private MockMvc mockMvc;

    // PostRepository 인스턴스를 주입받아서 사용할 수 있도록 하는 멤버 변수입니다. 이는 데이터베이스와 상호작용하여 테스트에서 사용됩니다.
    @Autowired
    private PostRepository postRepository;

    @Test
    public void list() throws Exception {
        this.mockMvc.perform(get("/posts"))
                .andExpect(status().isOk())
                .andExpect(content().string(
                        containsString("테스트입니다")
                ));
    }

    @Test
    public void create() throws Exception {
        String json = """
                {
                	"title": "새 글",
                	"content": "제곧내"
                }
                """;

        int oldSize = postRepository.findAll().size();

        this.mockMvc.perform(
                        post("/posts")
                                .contentType(MediaType.APPLICATION_JSON)
                                .content(json)
                )
                .andExpect(status().isCreated());

        int newSize = postRepository.findAll().size();

        assertThat(newSize).isEqualTo(oldSize + 1);
    }
}
```

인코딩 문제를 해결하기 위해 application.properties 파일에 관련 설정 추가.

```properties
server.servlet.encoding.charset=UTF-8
server.servlet.encoding.force=true
```

</details>

Spring 에서는 보통 객체생성시 직접 생성하지 않고 Dependency Injection(DI)을 통해 프레임워크에 위임하여 생성하기 때문에 직접적으로 Mocking 테스트환경을 만드는것이 쉽지 않습니다.

이러한 어려움을 도와주는것이 바로 @MockBean과 @SpyBean입니다. Bean등록 과정에서 테스트에 필요한 Mocking객체를 기존 객체 대신에 Bean으로 등록시켜 사용할수 있게 만들어주기때문에 해당
Bean을 의존 하는 모든 다른 객체들에 DI하여 손쉽게 Mocking객체를 사용 할 수 있도록 해줍니다.

<details><summary>SpyBean</summary>

### SpyBean

> [SpyBean](https://docs.spring.io/spring-boot/docs/current/api/org/springframework/boot/test/mock/mockito/SpyBean.html)
>

> [TestDouble](https://martinfowler.com/bliki/TestDouble.html)
>

@SpyBean은 스프링 프레임워크의 테스트 환경에서 사용되는 애노테이션 중 하나입니다.

이 애노테이션은 목 객체(Mock)가 아니라 실제 빈 객체의 일부를 유지하면서 일부 동작을 변경하거나 특정 메소드를 감시(spy)하고자 할 때 사용됩니다.

주로 목 객체(Mock)와 유사하게 사용되지만, 실제 객체의 일부를 유지하면서 목 객체의 일부 동작을 대신하는 목적으로 활용됩니다.

@SpyBean을 사용할 때는 @SpringBootTest와 같이 테스트 환경을 설정하는 애노테이션과 함께 사용됩니다.

> “Spies are stubs that also **record** some information based on **how they were called**.”
> 어떻게 호출되었는지 기록을 남겨준다.
>

Post 개수를 세는 게 아니라, 그냥 PostRepository의 save를 호출했는지만 확인해 보자.

```java

@Autowired
private MockMvc mockMvc

@SpyBean
private PostREpository postRepository

@Test
public void create()throws Exception{
        String json="""
				{
					"title": "새 글",
					"content": "제곧내"
				}
				""";

        this.mockMvc.perform(
        post("/posts")
        .contentType(MediaType.APPLICATION_JSON)
        .content(json)
        )
        .andExpect(status().isCreated());

        // postRepository spyBean에서 Post객체를 .save()했는지 검증
        verify(postRepository).save(any(Post.class));
        }
```

전달한 값이 제대로 들어갔는지 확인할 수도 있다.

```java
@Test
public void create()throws Exception{
        String json="""
				{
					"title": "새 글",
					"content": "제곧내"
				}
				""";

        this.mockMvc.perform(
        post("/posts")
        .contentType(MediaType.APPLICATION_JSON)
        .content(json)
        )
        .andExpect(status().isCreated());

        verify(postRepository).save(argThat(post->{
        return post.title().equals("새 글");
        }));
        }
```

getter를 피하기 위해 Reflection을 쓸 수도 있다. (Overengineering?!)

```java
@Test
public void create()throws Exception{
        String json="""
				{
					"title": "새 글",
					"content": "제곧내"
				}
				""";

        this.mockMvc.perform(
        post("/posts")
        .contentType(MediaType.APPLICATION_JSON)
        .content(json)
        )
        .andExpect(status().isCreated());

        verify(postRepository).save(argThat(post->{
        return getFieldValue(post,"title").equals("새 글");
        }));
        }

private Object getFieldValue(Object object,String fieldName){
        try{
        Field field=object.getClass().getDeclaredField(fieldName);
        field.setAccessible(true);
        return field.get(object);
        }catch(NoSuchFieldException|IllegalAccessException e){
        throw new RuntimeException(e);
        }
        }
```

</details>
<details><summary>MockBean</summary>
Spring-test에서 제공되는 어노테이션

Mock은 껍데기만 있는 객체를 얘기합니다.
인터페이스의 추상메소드가 메소드 바디는 없고 파라미터 타입과 리턴타입만 선언된 것처럼, Mock Bean은 기존에 사용되던 Bean의 껍데기만 가져오고 내부의 구현 부분은 모두 사용자에게 위임한 형태입니다.

Spring 에서는 보통 객체생성시 직접 생성하지 않고 Dependency Injection(DI)을 통해 프레임워크에 위임하여 생성하기 때문에 직접적으로 Mocking 테스트환경을 만드는것이 쉽지 않습니다.

```java
//@MockBean을 통해서 MockBean을 주입
@MockBean
private PostRepository postRepository;
//given()을 통해서 실행시 동작 지정
        given(postRepository.findAll()) //postRepository.findAll() 호출 시 
        .willReturn(List<Post>);
```

> @MockBean은 given에서 선언한 코드 외에는 전부 사용할 수 없습니다
>

> 반면에 @SpyBean은 given에서 선언한 코드 외에는 전부 실제 객체의 것을 사용합니다.
>

philwebb은 repository에서는 spy보다는 mock을 써야한다고 합니다.

무슨 말이냐 하면, repository를 @SpyBean을 통해 테스트 하려면 해당 테스트 범위가 너무 커지고 통합테스트와 다름 없는 테스트가 될 수 있다는 것입니다.
repository만 테스트 한다면 repository만 테스트를 하고, 그 외에 repository를 호출하는 메소드들을 테스트할때는 repository의 메소드들 중 사용하는 부분만 Mock으로 구현하는 것이 좀
더 올바른 방식이라고 얘기합니다.

만약 위와 같은 케이스가 발생하신다면 테스트 단위가 너무 큰건 아닌지 다시 한번 생각해보셔도 좋을것 같습니다.

### 동작방식

우선 테스트코드가 수행될때는 TestExecutionListener에 의해 이벤트를 감지하고 처리할수 있도록 되어있습니다.
org.springframework.test.context.TestExecutionListener

@MockBean을 처리할때는 위 TestExecutionListener를 구현한 MockitoTestExecutionListener를 통해 처리되고 있습니다.

```java
public class MockitoTestExecutionListener extends AbstractTestExecutionListener {

    @Override
    public void prepareTestInstance(TestContext testContext) throws Exception {
        initMocks(testContext);
        injectFields(testContext);
    }
}
```

prepareTestInstance 테스트 준비단계에서 injectFields메서드를 통해 @MockBean 필드를 찾아 Mocking Bean등록을 위한 구성을 수행하는것을 내부로직을 통해 확인 하실 수 있습니다.

@MockBean, @SpyBean가 포함된 테스트클래스를 수행 할 때 마다 Spring Context reload가 수행됩니다.

Spring context를 reload하는 과정은 짧게는 1초 길게는 1분까지도 걸리는 경우도 있습니다. 100개의 테스트 클래스를 수행하면서 10번의 reload가 일어난다고 가정한다면 테스트코드 수행이
10분이나 delay 되는 현상이 발생 될 수 있습니다.

> The Spring test framework will cache an ApplicationContext whenever possible between test runs.
> In order to be cached, the context must have an exactly equivalent configuration.
> Whenever you use @MockBean, you are by definition changing the context configuration.
>
> -Phil Webb (https://github.com/spring-projects/spring-boot/issues/10015#issuecomment-322634989)
>

위 comment에서 확인하실수 있다시피 이는 의도된 방향이며, 그도 그럴것이 BeanFactory에서 해당 Bean만 Mocking객체로 교체하면 되는것이 아니라 해당 Bean을 의존하고 있는 모든 Bean들을
모두 교체해서 주입해주어야 하기 때문에 이미 등록된 Context상에서 처리하기에는 쉽지 않은 작업입니다.

내부로직을 조금더 들어가서 살펴보면 Mocking Bean 등록과정 중 applicationContext를 새롭게 동작시키면서 refresh 하는 로직이 포함되어있는것을 확인 할 수 있습니다.

잦은 MockBean 사용은 테스트 성능에 영향을 끼칠수 있고 이는 대규모시스템일수록 영향이 커질수 밖에 없습니다. 이러한 점을 해결하기 위해 Lazy-mock-bean을 시도해봤습니다.

이 시도의 주요 포인트는 위에서 Bean을 의존하는 모든 객체들을 찾아 교체하는작업이 쉽지 않아 reload를 수행한다고 언급했지만 사실 특정 테스트에서 사용되는 Bean은 한정적이기 때문에 꼭 모든 의존 객체들에
영향을 끼쳐야 하는것이 아니라 테스트에 사용되는 Bean에만 제한적으로 Mocking Bean을 사용하도록 교체되면 된다는 점 입니다.

> 출처 - https://taes-k.github.io/2022/02/02/spring-mockbean/
</details>