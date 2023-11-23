# Test Concepts

<details><summary>V-Model</summary>

뒤로 돌아갈 수 없는 폭포수 모델은 현실에 부합하지 않지만, 폭포수 모델이 다루는 단계들은 매우 유용하다.

폭포수 모델은 선형하고 순차적인 진행을 강조하는 반면, V-모델은 개발과 테스트가 병행되는 모델.

> V 모델은 각 단계에 대한 테스트를 나누고, 처음부터 어떻게 테스트해야 하는지 결정하려고 노력한다.
>
V 모델은 소프트웨어 개발 및 테스트 접근 방식으로,</br>
전체 소프트웨어 개발 수명 주기 동안 초기 테스트와 검증 활동의 중요성을 강조합니다.

모델은 'V' 문자와 유사한 형태를 가지고 있어 "V 모델"이라 불립니다.</br>
개발 수명 주기의 각 단계는 테스트 또는 검증 단계와 연관되어 V 모양의 구조를 형성합니다.

![](https://upload.wikimedia.org/wikipedia/commons/9/96/V-model.JPG)

"V"의 왼쪽 하강 부분은 개발 단계를 나타내며, 오른쪽 상승 부분은 테스트 단계를 나타냅니다.

1. 요구사항 분석 → 사용자 중심 ⇒ 인수 테스트
2. 시스템 설계 → 시스템 사양 결정 ⇒ 시스템 테스트
3. 아키텍처 설계 → 고수준 설계 ⇒ 통합 테스트
4. 모듈 설계 → 저수준 설계 ⇒ 단위 테스트
5. 구현 → 코딩

V 모델의 핵심 아이디어는 테스트 활동이 해당 개발 활동과 병행하여 수행된다는 것입니다. 이는 개발 프로세스의 초기에 결함을 감지하고 수정함으로써 나중에 문제를 수정하는 데 드는 비용과 노력을 줄일 수 있습니다.

V 모델은 초기 결함 감지 측면에서 이점을 가지고 있지만 모든 프로젝트 유형에 적합하지 않을 수 있습니다. 일부 비평가들은 그것이 엄격하다고 주장하며 변화를 잘 수용하지 못할 수 있다고 말합니다. Scrum 및
Kanban과 같은 애자일 방법론은 특정 개발 환경에서 전통적인 V 모델의 대안으로 인기를 얻고 있습니다.
</details>

<details><summary>Test Matrix</summary>

> 테스트 매트릭스(Test Matrix)는 소프트웨어 테스트에서 다양한 테스트 케이스를 개요화하고 실행해야 하는 문서입니다.
>
이는 테스트 시나리오를 체계적으로 조직하고 관리하며 여러 가지 테스트 조건의 조합을 고려하는 데 사용됩니다. 매트릭스는 일반적으로 테스트 케이스 또는 테스트 시나리오를 나타내는 차원과 테스트 조건 또는 매개변수를
나타내는 다른 차원을 가지고 있습니다.

테스트 매트릭스의 주요 구성 요소는 다음과 같습니다.

1. **테스트 케이스/시나리오:**
    - 이 열에는 실행해야 하는 다양한 테스트 케이스 또는 시나리오가 나열됩니다. 테스트 케이스는 테스터가 소프트웨어가 예상대로 동작하는지 확인하기 위해 수행하는 특정 조건 또는 작업입니다.

2. **테스트 조건/매개변수:**
    - 이 행은 소프트웨어의 동작에 영향을 미칠 수 있는 다양한 조건이나 매개변수를 나타냅니다. 이러한 조건에는 다양한 입력 값, 구성, 환경 또는 기타 요소가 포함될 수 있습니다.

3. **교차 셀:**
    - 테스트 케이스 및 테스트 조건의 교차점에 있는 셀은 특정 조건 하에서 해당 테스트 케이스가 실행되었는지를 나타냅니다. 각 셀에는 테스트의 상태(예: 통과, 실패, 미실행), 추가 참고 사항 또는 상세한
      테스트 스크립트에 대한 링크와 같은 정보가 포함될 수 있습니다.

테스트 매트릭스는 다양한 테스트 케이스 및 조건의 조합을 체계적으로 다루어 포괄적인 테스트 커버리지를 보장합니다. 주로 다음과 같은 목적으로 사용됩니다.

- **회귀 테스트:** 새로운 변경 사항이나 기능이 기존 기능에 부정적인 영향을 미치지 않는지 확인합니다.
- **통합 테스트:** 컴포넌트나 모듈이 예상대로 함께 작동하는지 확인합니다.
- **호환성 테스트:** 소프트웨어가 다양한 구성에서(예: 다른 운영 체제, 브라우저, 기기) 작동하는지 확인합니다.
- **사용자 수용 테스트 (UAT):** 소프트웨어가 사용자 요구 사항을 충족하는지 확인합니다.

다음은 테스트 매트릭스의 간단한 예시입니다.

```
| 테스트 케이스    | 테스트 조건 1    | 테스트 조건 2    | 테스트 조건 3    |
|-------------------|------------------|------------------|------------------|
| 시나리오 1        | 통과             | 실패             | 미실행           |
| 시나리오 2        | 통과             | 통과             | 통과             |
| 시나리오 3        | 미실행           | 통과             | 실패             |
```

이 예시에서 "테스트 조건 1", "테스트 조건 2", "테스트 조건 3"은 서로 다른 조건 집합을 나타내며, "시나리오 1", "시나리오 2", "시나리오 3"은 서로 다른 테스트 케이스 또는 시나리오를
나타냅니다. 교차점의 셀은 각 특정 조건에서의 각 테스트 케이스의 상태를 나타냅니다.

</details>

<details><summary>내적품질</summary>
우리가 일반적으로 말하는 품질은 외적 품질. 눈에 보이지 않는 내적 품질은 당장에 큰 성과를 내지 않기 때문에 놓칠 때가 많다. 클린 코드 등이 회자되는 까닭도, 거기에 어떤 비즈니스 가치가 있어서가 아니라
“엉망인채로 가면 큰일난다”라는 걸 본능적으로 알기 때문. 맛만 좋으면 되지 주방이 깨끗할 필요가 있냐고 항변하는 식당, 어떻게든 수술만 하면 되지 손을 씻을 필요가 있냐고 항변하는 의사 등을 옹호할 사람은 아무도
없다.

품질이 내적 품질과 외적 품질로 나눠진다는 점에서도, 그리고 테스트 영역이 굉장히 넓다는 점에서도 우리는 절반만 감당할 수 있다. 하지만, 내적 품질이 우수하면 외적 품질을 끌어올리거나 대응하기 좋아진다.
산업혁명이나 정보혁명이 폭발적인 이유는 “도구를 개선하는 도구”의 존재가 장기적으로 “생산성”을 높였기 때문.

내적 품질을 높이는 것은 소프트웨어 개발 프로세스에서 중요한 목표 중 하나입니다. 여기에는 테스트 코드 작성과 같은 다양한 활동이 포함됩니다. 내적 품질 향상의 몇 가지 이유는 다음과 같습니다:

1. **버그와 결함의 감소:**
    - 테스트 코드를 작성하고 실행함으로써 코드의 버그와 결함을 미리 감지할 수 있습니다. 강력한 테스트 코드는 예상치 못한 동작을 미리 확인하여 실제 코드의 버그를 줄여줍니다.

2. **유지보수 용이성:**
    - 내적으로 품질 높은 코드는 유지보수가 쉽습니다. 테스트 코드를 작성하면 새로운 기능을 추가하거나 변경할 때 기존 기능이 영향을 받는지 여부를 빠르게 확인할 수 있습니다.

3. **코드 가독성 및 이해도 향상:**
    - 테스트 코드는 주석이나 문서화와 유사한 역할을 하며, 코드를 이해하고 다른 개발자들이 협업하기 쉽도록 돕습니다. 가독성이 높은 코드는 버그를 예방하고 유지보수를 효율적으로 수행하는 데 도움이 됩니다.

4. **리팩터링 지원:**
    - 내적 품질이 높은 코드는 리팩터링(코드의 구조를 변경하여 유지보수와 확장을 쉽게 하는 작업)에 용이합니다. 테스트 코드가 있는 경우, 리팩터링 후에도 코드가 여전히 기대한 대로 작동하는지 확인할 수
      있습니다.

5. **신뢰성 향상:**
    - 테스트 코드를 작성하면 소프트웨어의 신뢰성이 향상됩니다. 코드 변경이나 업데이트가 예상치 못한 부작용을 초래하지 않도록 보장할 수 있습니다.

6. **빠른 개발 주기:**
    - 테스트 코드를 작성하면 버그를 빠르게 감지하고 수정할 수 있습니다. 이는 개발 주기를 단축하고 릴리스 주기를 빠르게 만들어 개발 및 테스트의 효율성을 향상시킵니다.

7. **자동화된 테스트 실행:**
    - 테스트 코드를 작성하면 자동화된 테스트를 구현할 수 있습니다. 이는 반복적이고 빈번한 테스트를 효율적으로 수행하고 인간의 실수를 줄일 수 있습니다.

8. **품질 관리의 용이성:**
    - 테스트 코드를 통해 품질 관리를 쉽게 할 수 있습니다. 코드의 품질 및 안정성을 평가하고 추적할 수 있으며, 품질 향상을 위한 명확한 기준을 제공합니다.

내적 품질을 높이는 것은 소프트웨어의 전반적인 품질과 안정성을 향상시키며, 팀의 생산성과 유지보수 효율성을 향상시킵니다. 이는 최종적으로 사용자 경험과 고객 만족도에도 긍정적인 영향을 미칩니다.

---
https://wiki.c2.com/?InternalAndExternalQuality
http://www.exampler.com/old-blog/2003/08/21.1.html
https://developertesting.rocks/

</details>


Spring 공식 문서는 크게 둘로 나눠서 설명함:

- Unit Testing
- Integration Testing

<details><summary>단위(Unit) 테스트</summary>

함수 하나하나와 같이 코드의 작은 부분을 테스트 하는 것

단위 테스트의 관점에서 질문을 던져보자:

- 믿고 쓸 수 있는 부품인가?
- 믿을 수 있는 부품이 있다면 어떻게 하면 되는가?

[SUT](http://xunitpatterns.com/SUT.html)

참고

- [The Practical Test Pyramid](https://martinfowler.com/articles/practical-test-pyramid.html)
- [“Mocking 때문에 테스트 코드를 작성하기 어렵나요” 영상](https://youtu.be/RoQtNLl-Wko)
- [“프론트엔드도 테스트해야 하나요?” 영상](https://youtu.be/-kUmsKRmOnA)

</details>
<details><summary>종단 간(E2E) 테스트</summary>

사용자와 어플리케이션의 상호작용이 잘 이뤄지는지 테스트하는 것

## Spring Test (Integration Test)

> [Testing](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html)
>

> [Testing improvements in Spring Boot 1.4](https://spring.io/blog/2016/04/15/testing-improvements-in-spring-boot-1-4)
>

> [Testing the Web Layer](https://spring.io/guides/gs/testing-web/)
>

[Spring 공식 문서는 크게 둘로 나눠서 설명함](https://docs.spring.io/spring-integration/docs/current/reference/html/testing.html):

- Unit Testing
- Integration Testing

Spring은 코드에 구조적으로 개입하는 게 적어서, 단위 테스트를 쉽게 작성할 수 있다.

Spring의 힘을 빌려서 테스트하는 건 IoC 컨테이너를 적극적으로 활용하고 싶거나, Spring Web MVC로 구현된 부분을 테스트하고 싶을 때, 즉 Spring에서 통합 테스트라고 부르는 경우다.

</details>
<details><summary>통합 테스트(Integeration)</summary>

# Spring Test (Integration Test)

> [Testing](https://docs.spring.io/spring-framework/docs/current/reference/html/testing.html)
>

> [Testing improvements in Spring Boot 1.4](https://spring.io/blog/2016/04/15/testing-improvements-in-spring-boot-1-4)
>

> [Testing the Web Layer](https://spring.io/guides/gs/testing-web/)
>
>

Spring은 코드에 구조적으로 개입하는 게 적어서, 단위 테스트를 쉽게 작성할 수 있다.

Spring의 힘을 빌려서 테스트하는 건 IoC 컨테이너를 적극적으로 활용하고 싶거나, Spring Web MVC로 구현된 부분을 테스트하고 싶을 때, 즉 Spring에서 통합 테스트라고 부르는 경우다.

단위 테스트로는 RequestMapping, Data Binding, Type Conversion, Validation, 등등을 커버할 수 없습니다.

따라서 코드 커버리지를 높이기 위해서는 통합테스트를 실시해야합니다.

장점

- 모든 빈을 컨테이너에 올리고 테스트 하기 때문에 운영환경과 유사한 환경에서 테스트를 할 수 있습니다.
- 통합테스트 이름 그대로, 전체적인 테스트를 진행할 수 있어, 코드 커버리지가 높아집니다.

단점

- 모든 빈을 컨테이너에 올리고 테스트 하기 때문에 시간이 오래걸립니다.
- 전체적인 테스트를 한번에 진행하기 때문에, 특정 계층 또는 특정 빈에서 발생하는 오류의 디버깅이 어렵습니다.

```java

// 이 애노테이션은 랜덤 포트를 사용하여 테스트용 웹 환경을 설정합니다. 이것은 애플리케이션을 실제 웹 서버로 띄우고 테스트를 수행하게 합니다.
@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
class PostFeatureTest {
    // 테스트 중인 애플리케이션의 랜덤 포트 값을 가져와서 port 변수에 주입합니다.
    @Value("${local.server.port}")
    private int port;

    // TestRestTemplate은 테스트 환경에서 RESTful 웹 서비스를 테스트하기 위한 스프링의 편리한 도구입니다. 이를 사용하여 HTTP 요청을 보내고 응답을 받을 수 있습니다.
    @Autowired
    private TestRestTemplate restTemplate;

    // 실제 테스트 메소드입니다
    @Test
    public void post() {
        String url = "http://localhost:" + port + "/posts";

        PostDto postDto = new PostDto("ID", "새 글", "제곧내");

        restTemplate.postForLocation(url, postDto); // POST 요청을 보냅니다

        String body = restTemplate.getForObject(url, String.class);

        assertThat(body).contains("새 글");
        assertThat(body).contains("제곧내");

        String id = findLastId(body);

        restTemplate.delete(url + "/" + id);

        body = this.restTemplate.getForObject(url, String.class);

        assertThat(body).doesNotContain("새 글");
    }

    // 정규 표현식을 사용하여 주어진 목록에서 마지막으로 추가된 포스트의 ID를 추출하는 메소드입니다. 
    // 이 메소드는 정규 표현식을 활용하여 JSON 형태의 목록에서 "id" 필드의 값을 추출합니다.
    private String findLastId(String body) {
        Pattern pattern = Pattern.compile("\"id\":\"([^\"]+)\"");
        Matcher matcher = pattern.matcher(body);

        String id = "";
        while (matcher.find()) {
            id = matcher.group(1);
        }
        return id;
    }
}

```

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