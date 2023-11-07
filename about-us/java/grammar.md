# Grammar

<details><summary>Jackson ObjectMapper</summary>

> Java에서 Jackson ObjectMapper는 JSON 데이터와 Java 객체 간의 직렬화 (serialization) 및 역직렬화 (deserialization)를 처리하는 데 사용되는 중요한
> 라이브러리입니다.
>

ObjectMapper를 사용하여 JSON 데이터를 Java 객체로 변환하거나 Java 객체를 JSON 데이터로 변환할 수 있습니다. 아래는 ObjectMapper의 사용 예제입니다:

```java
import com.fasterxml.jackson.databind.ObjectMapper;

public class JsonExample {
    public static void main(String[] args) {
        // ObjectMapper 생성
        ObjectMapper objectMapper = new ObjectMapper();

        try {
            // Java 객체를 JSON으로 직렬화 (Object to JSON)
            User user = new User("John Doe", 30);
            String userJson = objectMapper.writeValueAsString(user);
            System.out.println("User JSON: " + userJson);

            // JSON을 Java 객체로 역직렬화 (JSON to Object)
            String json = "{\"name\":\"Alice\",\"age\":25}";
            User newUser = objectMapper.readValue(json, User.class);
            System.out.println("User Object: " + newUser);

        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}

class User {
    private String name;
    private int age;

    // 기본 생성자 및 Getter/Setter 생략 (ObjectMapper가 자동으로 생성)

    public User(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return "User [name=" + name + ", age=" + age + "]";
    }
}
```

위 예제에서는 ObjectMapper를 사용하여 Java 객체와 JSON 데이터 간의 변환을 수행합니다. ObjectMapper를 사용하려면 Jackson 라이브러리를 프로젝트에 추가해야 합니다. 이 예제에서는
User 클래스를 정의하고 해당 클래스를 JSON으로 직렬화하고 JSON을 User 객체로 역직렬화합니다.

중요한 메서드:

- `writeValueAsString(Object value)`: Java 객체를 JSON 문자열로 직렬화합니다.
- `readValue(String content, Class<T> valueType)`: JSON 문자열을 Java 객체로 역직렬화합니다.

이러한 ObjectMapper의 사용을 통해 Java 애플리케이션에서 JSON 데이터를 쉽게 처리할 수 있습니다.
</details>
<details><summary>record</summary>

Java 16부터 도입된 "record"는 자바의 새로운 데이터 클래스 유형을 나타내며, **불변(immutable)한 데이터 객체를 생성하는 데 사용됩니다**. Record는 데이터 저장과 액세스를 위한 메서드를
자동으로 생성하며, 간결하게 데이터 클래스를 정의할 수 있도록 도와줍니다.

1. **불변(Immutable) 데이터 클래스**:
    - Record 클래스는 필드 값을 변경할 수 없는 불변 클래스로 정의됩니다. 필드 값이 한 번 설정되면 변경할 수 없으므로 안전성을 보장합니다.

2. **간결한 선언**:
    - Record는 데이터 클래스를 정의하는 데 간결한 구문을 사용합니다. 필드를 선언하고 자동으로 생성되는 생성자, `equals()`, `hashCode()`, `toString()` 메서드를 얻을 수
      있습니다.

```java
record Person(String name, int age) {
    // 생성자, equals(), hashCode(), toString() 메서드는 자동 생성됨
}
```

3. **자동 생성 메서드**:
    - Record는 필드의 값을 설정하는 생성자를 자동으로 생성하며, 필드 값을 읽는 getter 메서드를 제공합니다.

```java
Person person=new Person("Alice",30);
        String name=person.name(); // Getter 메서드를 통해 필드 값 읽기
```

4. **equals() 및 hashCode() 메서드**:
    - Record 클래스는 필드의 값에 기반하여 `equals()` 및 `hashCode()` 메서드를 자동으로 생성합니다. 이를 통해 객체의 동등성 비교와 해시 코드 계산이 간단하게 수행됩니다.

5. **toString() 메서드**:
    - Record는 `toString()` 메서드를 자동으로 생성하여 객체를 문자열로 변환할 때 기본 동작을 정의합니다.

```java
Person person=new Person("Alice",30);
        System.out.println(person); // 출력: Person[name=Alice, age=30]
```

6. **패턴 매칭 지원**:
    - Java 17부터는 Record를 활용하여 패턴 매칭(Pattern Matching)을 수행할 수 있으며, 더욱 강력한 기능을 제공합니다.

Record는 데이터를 효율적으로 표현하고 다루는 데 유용한 Java의 새로운 언어 기능 중 하나이며, 특히 불변 데이터 객체를 간결하게 정의하고 사용할 때 강력한 장점을 제공합니다.

> DTO와 record는 둘 다 데이터를 표현하는데 사용되지만, DTO는 데이터 전송에 주로 사용되는 반면, record는 데이터 표현과 조작을 위한 Java의 불변 클래스입니다
>

