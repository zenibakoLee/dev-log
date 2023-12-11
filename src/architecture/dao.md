# Java DAO (Data Access Object)

DAO (Data Access Object)는 데이터베이스와 상호작용하고 데이터베이스 연산을 추상화하는 객체지향 디자인 패턴입니다.
> 실질적으로 DB에 접근하여 데이터를 조회하거나 조작하는 기능을 전담하는 객체
>

DAO는 데이터베이스와의 통신을 캡슐화하고 비즈니스 로직에서 데이터 액세스 로직을 분리함으로써 데이터베이스와의 결합도를 낮추는 역할을 합니다. 주요 특징은 다음과 같습니다:

1. **데이터 액세스 추상화**: DAO는 데이터베이스와의 통신을 추상화하고 데이터베이스와 직접 상호작용하는 코드를 비즈니스 로직으로부터 분리합니다. 이를 통해 데이터 액세스 로직을 더 효율적으로 관리할 수
   있습니다.

2. **CRUD 작업**: DAO는 주로 CRUD (Create, Read, Update, Delete) 작업을 수행합니다. 즉, 데이터를 생성, 조회, 업데이트 및 삭제하는 메서드를 제공합니다.

3. **트랜잭션 관리**: DAO는 데이터베이스 트랜잭션을 관리할 수 있으며, 데이터베이스 연산의 일관성과 안정성을 보장합니다. 예외 발생 시 롤백 등의 트랜잭션 관리 기능을 제공합니다.

4. **재사용성**: DAO는 데이터 액세스 로직을 재사용 가능한 컴포넌트로 만듭니다. 이로써 같은 데이터베이스 테이블에 대한 다양한 애플리케이션 부분에서 동일한 데이터 액세스 로직을 사용할 수 있습니다.

5. **데이터베이스 독립성**: DAO는 데이터베이스 종속성을 최소화하며, 데이터베이스가 변경되어도 애플리케이션 코드를 수정할 필요가 없도록 합니다.

6. **설계 원칙 준수**: DAO는 SOLID 원칙과 같은 객체지향 설계 원칙을 준수하고, 코드의 가독성과 유지보수성을 향상시킵니다.

일반적으로 DAO는 특정 데이터베이스 테이블 또는 엔터티에 대한 클래스로 구현되며, 해당 데이터베이스 테이블에 대한 데이터 액세스 로직을 제공합니다. 예를 들어, "UserDAO" 클래스는 "User" 엔터티와
상호작용하고, 사용자 데이터베이스 테이블에 대한 CRUD 작업을 제공할 수 있습니다. 이를 통해 데이터베이스와의 상호작용을 추상화하고 코드의 모듈성을 향상시킵니다.

Service 레이어에서 다양한 Dto 객체 저장 관리 방식을 interface를 통해 사용하도록 하는 예제

```java
public interface PostDAO {

    List<PostDto> findAll();

    PostDto find(String id);

    void save(PostDto postDto);

    void remove(String id);
}
```

```java
public class PostListDAO implements PostDAO {
    private final List<PostDto> postDtos;

    public PostListDAO() {
        List<PostDto> postDtos = new ArrayList<>();
        postDtos.add(new PostDto("1", "test title2", "test content"));
        postDtos.add(new PostDto("2", "test title2", "test content"));
        this.postDtos = new ArrayList(postDtos);
    }

    @Override
    public List<PostDto> findAll() {
        return new ArrayList<>(postDtos);
    }

    @Override
    public PostDto find(String id) {
        return postDtos.stream().filter(postDto -> postDto.getId().equals(id)).findFirst().get();
    }

    @Override
    public void save(PostDto postDto) {
        postDtos.add(postDto);
    }

    @Override
    public void remove(String id) {
        PostDto foundPost = find(id);
        postDtos.remove(foundPost);
    }
}
```

```java
public class PostMapDao implements PostDAO {
    private Map<String, PostDto> postDtos;

    public PostMapDao() {
        this.postDtos = new HashMap<>();
        postDtos.put("1", new PostDto("1", "test title2", "test content"));
        postDtos.put("2", new PostDto("2", "test title2", "test content"));
    }

    @Override
    public List<PostDto> findAll() {
        // ID 순서대로 나오지 않음
        return new ArrayList<>(postDtos.values());
    }

    @Override
    public PostDto find(String id) {
        PostDto postDto = postDtos.get(id);
        if (postDto == null) {
            throw new PostNotFound();
        }
        return postDto;
    }

    @Override
    public void save(PostDto postDto) {
        postDtos.put(postDto.getId(), postDto);
    }

    @Override
    public void remove(String id) {
        postDtos.remove(id);
    }
}
```

구현 클래스를 직접 사용하는 대신 해당 인터페이스를 사용하는 방법에는 여러 장점이 있습니다. 이러한 접근 방식은 코드의 유연성, 테스트 용이성, 유지보수성, 그리고 의존성 관리 측면에서 다음과 같은 이점을 가질 수
있습니다:

1. **의존성 역전 원칙 (Dependency Inversion Principle, DIP):** 구현 클래스 대신 인터페이스에 의존하는 것은 DIP를 준수하는 것이며, 이것은 좋은 소프트웨어 아키텍처의 기본 원칙
   중 하나입니다. 이렇게 하면 고수준 모듈이 저수준 모듈에 의존하지 않고, 둘 다 추상화에 의존하게 됩니다.

2. **코드 유연성:** `PostService`는 `PostDAO` 인터페이스에만 의존하므로 필요한 경우 서비스 계층을 수정하지 않고도 다른 구현 클래스를 쉽게 교체할 수 있습니다. 이것은 시스템이 더 쉽게 확장
   가능하고 변경 가능하도록 만듭니다.

3. **테스트 용이성:** 의존성 주입을 통해 Mock 또는 가짜 객체를 사용하여 단위 테스트를 수행할 수 있습니다. 이것은 테스트 주도 개발 (TDD) 및 테스트 가능한 코드 작성에 매우 중요합니다.

4. **유지보수성:** 인터페이스와 구현 클래스를 분리함으로써 코드의 유지보수가 용이해집니다. 새로운 구현 클래스를 추가하거나 기존 클래스를 변경해도 서비스 계층의 변경을 최소화할 수 있습니다.

5. **의존성 관리 및 인젝션:** 의존성 주입 프레임워크 또는 패턴을 사용하여 런타임에 적절한 구현 클래스를 주입할 수 있습니다. 이렇게 하면 코드의 결합도를 낮출 수 있으며 더 좋은 테스트 가능성과 모듈성을
   제공합니다.

6. **다형성 활용:** 다형성을 활용하여 동일한 인터페이스를 구현하는 다양한 클래스를 처리할 수 있습니다. 이는 동적인 구현 교체와 다양한 구현 간의 상호 작용을 허용합니다.

결론적으로, 구현 클래스 대신 인터페이스를 사용하는 방식은 코드의 유연성을 향상시키고 의존성을 효과적으로 관리하여 좀 더 모듈화된, 테스트 가능한, 확장 가능한 소프트웨어 시스템을 만드는 데 도움이 됩니다.

---

### DAO & Repository

> [repository](./repositoryPattern.md) := dao (비슷함) 이 둘은 거의 같다고 생각하셔도 무방합니다.
>
> 좀 더 깊이있게 차이를 설명하면, repotiroy는 엔티티 객체를 보관하고 관리하는 저장소이고,
>
> dao는 데이터에 접근하도록 DB접근 관련 로직을 모아둔 객체입니다.
>
> 둘다 개념의 차이일뿐 실제로 개발할 때는 비슷하게 사용됩니다.
>
>  김영한님 - https://www.inflearn.com/questions/111159/comment/84415
>

DAO와 Repository는 모두 데이터베이스 액세스에 사용되지만, DAO는 전통적인 패턴으로 주로 저수준 API를 사용하고 SQL 쿼리를 직접 작성합니다. 반면에 Repository는 주로 스프링 데이터 JPA와
같은 고수준 ORM 기술과 함께 사용되며, 객체와 데이터베이스 간의 매핑을 자동으로 처리하고 높은 수준의 추상화를 제공합니다. 최근에는 두 용어 간의 구분이 희미해지고 있습니다.

* DAO는 Data Peristence의 추상화이고, Repository는 객체 Collection의 추상화이다.
* DAO는 storage system에 더 가까운 개념이고 상대적으로 low level concept, Repository는 Domain객체에 가까운 개념이며 상대적으로 high level concept
* DAO는 데이터 맵핑/접근 계층으로 쿼리를 숨기지만, Repository는 Domain과 DAL사이의 계층으로 데이터를 대조하고 Domain 객체로 Mapping하는 로직을 숨긴다.
* DAO는 Repository를 사용하여 구현할 수 없지만, Repository는 DAO를 사용해 구현할 수 있다.

Data Access한다는 점에서 Repository와 DAO는 공통점을 갖지만, Repository는 객체 중심, DAO는 데이터 저장소(DB 테이블) 중심인 것이다.

또한, Repository는 객체 중심으로 데이터를 다루기 위해 하나 이상의 DAO를 사용할 수 있으며, 따라서 DAO보다 higher layer이다.


---
참고:

- https://isaac56.github.io/etc/2021/08/29/difference_DAO_Repository/