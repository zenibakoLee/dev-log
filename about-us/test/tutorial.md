# Tutorial

### Model Test

```java
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;


class PostTest {
    @Test
    void creation() {
        Post post = new Post(
                "제목",
                "작성자",
                new MultilineText("내용")
        );

        assertThat(post.id()).isNotNull();
        assertThat(post.title()).isEqualTo("제목");
        assertThat(post.author()).isEqualTo("작성자");
        assertThat(post.content().toString()).isEqualTo("내용");
    }

    @Test
    void update() {
        Post post = new Post(
                new PostId("001POST"),
                "제목",
                "작성자",
                new MultilineText("내용")
        );

        post.update("변경된 제목", new MultilineText("변경된 내용"));

        assertThat(post.title()).isEqualTo("변경된 제목");
        assertThat(post.author()).isEqualTo("작성자");
        assertThat(post.content().toString()).isEqualTo("변경된 내용");
    }
}


```

---

### Service Test

Repository 모킹

```java
import org.junit.jupiter.api.BeforeEach;
import org.junit.jupiter.api.DisplayName;
import org.junit.jupiter.api.Test;

import static org.assertj.core.api.Assertions.assertThat;
import static org.mockito.BDDMockito.given;
import static org.mockito.Mockito.mock;

class UpdatePostServiceTest {
    private PostRepository postRepository;

    private UpdatePostService updatePostService;

    @BeforeEach
    void setUp() {
        postRepository = mock(PostRepository.class);

        updatePostService = new UpdatePostService(postRepository);
    }

    @Test
    @DisplayName("게시물 수정")
    void update() {
        PostId postId = new PostId("001POST");

        Post post =
                new Post(postId, "제목", "작성자", new MultilineText("내용"));

        given(postRepository.find(postId)).willReturn(post);

        PostUpdateDto postUpdateDto =
                new PostUpdateDto("변경된 제목", "변경된 내용");

        updatePostService.updatePost(postId.toString(), postUpdateDto);

        assertThat(post.title()).isEqualTo("변경된 제목");
        assertThat(post.content()).isEqualTo(new MultilineText("변경된 내용"));
    }
}

```

---

### Controller Test

[@WebMvcTest](./springBootTest.md)
[MockMvc, @MockBean](./tool.md)

```java

@WebMvcTest(PostController.class)
class PostControllerTest {
    @Autowired
    MockMvc mockMvc;

    @MockBean
    private UpdatePostService updatePostService;

    @Test
    @DisplayName("PATCH /posts/{id}")
    void update() throws Exception {
        String id = "001POST";

        String json = """
                {
                  "title": "데브로드",
                  "content": "열심히 합시다"
                }
                """;

        mockMvc.perform(patch("/posts/" + id)
                        .contentType(MediaType.APPLICATION_JSON)
                        .content(json)
                )
                .andExpect(status().isOk());

        verify(updatePostService)
                .updatePost(eq(id), any(PostUpdateDto.class));
    }
}
```