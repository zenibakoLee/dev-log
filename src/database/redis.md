## Redis

> [Redis](https://redis.io/)
>

> [Introduction to Redis](https://redis.io/docs/about/)
>

> [Redis란 무엇입니까?(ElasticCache) - AWS](https://aws.amazon.com/ko/elasticache/what-is-redis/)
>

“Redis is an open source (BSD licensed), in-memory data structure store used as a database, cache, message broker, and
streaming engine”

Redis는 어떤 Data Store인가?

1. In-memory: 메모리에 데이터를 저장하며, 디스크에는 영속성을 보장하기 위한 스냅샷이나 로그와 같은 기능에 사용됩니다. 메모리에 데이터를 저장하기 때문에 빠른 응답 시간을 얻을 수 있습니다.
2. Data Structure

우리는 어떤 용도로 쓸 것인가?

1. Database
2. Cache

Docker로 가볍게 띄울 수 있는가? → YES!

- [How to Use the Redis Docker Official Image](https://www.docker.com/blog/how-to-use-the-redis-docker-official-image/)

```bash
docker run -d --name redis -p 6379:6379 redis

docker ps

docker stop redis
docker rm redis
```

Redis CLI를 띄워서 기능 테스트를 해보자.

```bash
docker exec -it redis redis-cli
```

간단히 key-value 스토어 기능을 확인해 보자.

```bash
> GET my-name
(nil)
> SET my-name "Tester"
OK
> GET my-name
"Tester"
> KEYS *
1) "my-name"
> exit
# → 이렇게 하면 종료된다.
```

## Spring Data Redis

> [Spring Data Redis](https://docs.spring.io/spring-data/data-redis/docs/current/reference/html/)
>

`build.gradle` 파일에 의존성 추가.

```bash
implementation 'org.springframework.boot:spring-boot-starter-data-redis'
```

`application.yml`에 서버 정보를 넣는다.

```bash
spring:
  redis:
    host: localhost
    port: 6379
```

Redis 관련 설정.

```java

@Configuration
class RedisConfig {
    @Bean
    public LettuceConnectionFactory redisConnectionFactory() {
        return new LettuceConnectionFactory();
    }

    @Bean
    public RedisTemplate<String, Object> redisTemplate(
            RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();

        redisTemplate.setConnectionFactory(redisConnectionFactory);

        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new StringRedisSerializer());

        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashValueSerializer(new StringRedisSerializer());

        return redisTemplate;
    }
}
```

간단한 것도 처리할 수 있지만, Spring Data 패밀리답게 Redis Repository를 써보자.

- [Redis Repositories](https://docs.spring.io/spring-data/data-redis/docs/current/reference/html/#redis.repositories)

우리가 여기서 사용할 Read Model은 장바구니 정보를 그대로 F/E로 전달하는데 쓰는 `CartDto`다.

```java

@RedisHash("carts")
public class CartDto {
    @Id
    private String id;

    private List<LineItemDto> lineItems;

    private CartDto() {
    }

    public CartDto(String id, List<LineItemDto> lineItems) {
        this.id = id;
        this.lineItems = lineItems;
    }

    public List<LineItemDto> getLineItems() {
        return lineItems;
    }

    public record LineItemDto(
            String id,
            String productName,
            long unitPrice,
            int quantity,
            long totalPrice
    ) {
    }
}
```

Spring Data JPA와 마찬가지로 Repository 자체는 아주 간단히 만들 수 있다.

```java
public interface CartDtoRepository extends CrudRepository<CartDto, String> {

}
```

잘 작동하는지 JUnit으로 확인해 보자.

```java

@SpringBootTest
@ActiveProfiles("test")
class CartDtoRepositoryTest {
    @Autowired
    private CartDtoRepository cartDtoRepository;

    @Test
    void test() {
        String id = "test";

        CartDto.LineItemDto lineItem = new CartDto.LineItemDto(
                "line-item-01", "Product #1", 10_000, 2, 20_000);

        CartDto cartDto = new CartDto(id, List.of(lineItem));

        cartDtoRepository.save(cartDto);

        CartDto found = cartDtoRepository.findById(id).get();

        // 여기서는 그냥 간단히 toString으로 확인하지만… 🤔
        assertThat(found.toString()).contains(
                "CartDto{id='test', lineItems=[LineItemDto[id=line-item-01");
    }
}
```