# Grammar

<details>

<summary>Jackson ObjectMapper</summary>

Java에서 Jackson ObjectMapper는 JSON 데이터와 Java 객체 간의 직렬화 (serialization) 및 역직렬화 (deserialization)를 처리하는 데 사용되는 중요한 라이브러리입니다.

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

위 예제에서는 ObjectMapper를 사용하여 Java 객체와 JSON 데이터 간의 변환을 수행합니다. ObjectMapper를 사용하려면 Jackson 라이브러리를 프로젝트에 추가해야 합니다. 이 예제에서는 User 클래스를 정의하고 해당 클래스를 JSON으로 직렬화하고 JSON을 User 객체로 역직렬화합니다.

중요한 메서드:

* `writeValueAsString(Object value)`: Java 객체를 JSON 문자열로 직렬화합니다.
* `readValue(String content, Class<T> valueType)`: JSON 문자열을 Java 객체로 역직렬화합니다.

이러한 ObjectMapper의 사용을 통해 Java 애플리케이션에서 JSON 데이터를 쉽게 처리할 수 있습니다.

</details>

<details>

<summary>record</summary>

Java 16부터 도입된 "record"는 자바의 새로운 데이터 클래스 유형을 나타내며, **불변(immutable)한 데이터 객체를 생성하는 데 사용됩니다**. Record는 데이터 저장과 액세스를 위한 메서드를 자동으로 생성하며, 간결하게 데이터 클래스를 정의할 수 있도록 도와줍니다.

1. **불변(Immutable) 데이터 클래스**:
   * Record 클래스는 필드 값을 변경할 수 없는 불변 클래스로 정의됩니다. 필드 값이 한 번 설정되면 변경할 수 없으므로 안전성을 보장합니다.
2. **간결한 선언**:
   * Record는 데이터 클래스를 정의하는 데 간결한 구문을 사용합니다. 필드를 선언하고 자동으로 생성되는 생성자, `equals()`, `hashCode()`, `toString()` 메서드를 얻을 수 있습니다.

```java
record Person(String name, int age) {
    // 생성자, equals(), hashCode(), toString() 메서드는 자동 생성됨
}
```

3. **자동 생성 메서드**:
   * Record는 필드의 값을 설정하는 생성자를 자동으로 생성하며, 필드 값을 읽는 getter 메서드를 제공합니다.

```java
Person person=new Person("Alice",30);
        String name=person.name(); // Getter 메서드를 통해 필드 값 읽기
```

4. **equals() 및 hashCode() 메서드**:
   * Record 클래스는 필드의 값에 기반하여 `equals()` 및 `hashCode()` 메서드를 자동으로 생성합니다. 이를 통해 객체의 동등성 비교와 해시 코드 계산이 간단하게 수행됩니다.
5. **toString() 메서드**:
   * Record는 `toString()` 메서드를 자동으로 생성하여 객체를 문자열로 변환할 때 기본 동작을 정의합니다.

```java
Person person=new Person("Alice",30);
        System.out.println(person); // 출력: Person[name=Alice, age=30]
```

6. **패턴 매칭 지원**:
   * Java 17부터는 Record를 활용하여 패턴 매칭(Pattern Matching)을 수행할 수 있으며, 더욱 강력한 기능을 제공합니다.

Record는 데이터를 효율적으로 표현하고 다루는 데 유용한 Java의 새로운 언어 기능 중 하나이며, 특히 불변 데이터 객체를 간결하게 정의하고 사용할 때 강력한 장점을 제공합니다.

DTO와 record는 둘 다 데이터를 표현하는데 사용되지만, DTO는 데이터 전송에 주로 사용되는 반면, record는 데이터 표현과 조작을 위한 Java의 불변 클래스입니다

</details>

<details>

<summary>Date/LocalDateTime/Epoch Time</summary>

일반적으로는 java.time 패키지의 새로운 API를 사용하는 것이 좋습니다. 이러한 API는 이전의 Date와 Calendar 클래스의 한계를 극복하고 풍부한 기능을 제공하기 때문입니다. Epoch Time은 특정 상황에서 유용하지만, 대부분의 경우에는 LocalDateTime과 Instant를 사용하는 것이 더 선호됩니다.

`java.util.Date`와 `java.time.LocalDateTime`은 Java에서 날짜와 시간을 처리하기 위한 두 가지 다른 API입니다.

1.  **`java.util.Date`:**

    * `java.util.Date` 클래스는 Java 1.0부터 존재하며, JDK 8 이전에 주로 사용되었습니다.
    * `Date` 객체는 특정 시점의 타임스탬프를 나타냅니다.
    * `Date`는 가변적이며, 쓰레드 세이프하지 않기 때문에 동시성 문제가 발생할 수 있습니다.
    * JDK 8 이전 버전에서는 `SimpleDateFormat` 등을 사용하여 날짜 및 시간 형식을 처리했습니다.

    ```java
    // 예전 방식의 날짜 생성
    Date date = new Date();
    ```
2.  **`java.time.LocalDateTime`:**

    * `java.time` 패키지는 JDK 8에서 도입된 새로운 날짜 및 시간 API를 제공합니다.
    * `LocalDateTime`은 날짜와 시간 정보를 갖는 불변 클래스로, 특정 시간대에 묶이지 않은 날짜와 시간을 표현합니다.
    * `LocalDateTime`은 스레드 세이프하며 가변적이지 않아서 불변 객체의 성질을 가집니다.
    * `DateTimeFormatter`를 사용하여 날짜 및 시간 형식을 처리합니다.

    ```java
    // 현재 날짜와 시간 가져오기
    LocalDateTime localDateTime = LocalDateTime.now();
    ```
3.  **JDK 8 이후의 선택:**

    * JDK 8 이상을 사용하는 경우, `java.time.LocalDateTime`과 같은 새로운 API를 사용하는 것이 좋습니다. 이 API는 이전의 `Date` 클래스보다 풍부하며 사용하기 쉽습니다.
    * `LocalDateTime`는 불변 객체로 설계되어 있어서 쓰레드 세이프하고, 다양한 메서드를 통해 날짜 및 시간을 다루는데 유연성을 제공합니다.

    ```java
    // LocalDateTime을 문자열로 변환
    DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
    String formattedDateTime = localDateTime.format(formatter);
    ```

### 이제는 `java.time` 패키지를 사용하는 것이 권장되며, 특히 `LocalDateTime`은 로컬 날짜와 시간을 다루기에 매우 편리합니다. `Date` 클래스는 특정 시간대에 묶여 있고 가변적이기 때문에 사용을 피하는 것이 좋습니다.

Epoch Time은 1970년 1월 1일 00:00:00 UTC부터 현재까지의 시간을 초로 표현한 값입니다. Java 8에서는 이 값을 나타내는 Instant 클래스가 제공됩니다.

```java
// 현재 날짜와 시간 가져오기
LocalDateTime localDateTime=LocalDateTime.now();

// Epoch Time으로 변환
        Instant instant=localDateTime.atZone(ZoneId.systemDefault()).toInstant();
        long epochTime=instant.toEpochMilli();

```

LocalDateTime와 Epoch Time를 연결하여 사용할 수 있습니다.

Epoch Time은 특히 시간을 숫자로 간편하게 표현할 수 있어 데이터베이스와의 상호 작용 등에 유용합니다.

```java
// Epoch Time을 LocalDateTime으로 변환
Instant instant=Instant.ofEpochMilli(epochTime);
        LocalDateTime convertedDateTime=LocalDateTime.ofInstant(instant,ZoneId.systemDefault());

```

</details>

<details>

<summary>I/O</summary>

**class FileOutputStream**

java.lang.Object\
ㄴ java.io.OutputStream\
ㄴ java.io.FileOutputStream

인터페이스: Closeable, Flushable

: File 또는 FileDescriptor에 데이터를 출력하기 위한 파일 출력 스트림, 관계된 파일이 열려 있는 경우는 이 클래스의 생성자는 실패

</details>
