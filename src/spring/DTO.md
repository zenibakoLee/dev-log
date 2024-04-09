# DTO

Data Transfer Object

백엔드를 구성할 때 계층(tier) 간 통신을 하거나 서로 다른 프로그램(백엔드, 프론트엔드) 끼리 통신을 할때 올바른 형태의 요청과 응답을 주고 받아야 한다. 이때 사용할 수 있는 DTO와 JSON에 대해 알아보자.

### **제약 조건**

* “between processes”
* “working with a remote interface, such as Remote Facade”
* 다른 프로세스와 통신, 그것도 원격(네트워크를 통해)으로!

<details>

<summary>IPC</summary>

#### [**IPC (Inter-Process Communication)**](https://ko.wikipedia.org/wiki/%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4\_%EA%B0%84\_%ED%86%B5%EC%8B%A0)

* 서로 다른 프로세스, 거칠 게 이야기하면 서로 다른 프로그램이 서로 통신.
* B/E와 F/E로 Tier를 나누면 IPC가 필수적이다.
* IPC에서 쓸 수 있는 기술
  * File → 가장 기본적인 접근. 원격 환경에서 활용하기 어렵다.
  * Socket → 파일과 유사하게 읽고 쓸 수 있지만, 원격 환경에서도 활용할 수 있다.
    * HTTP 같은 고수준 프로토콜을 활용하면 어느 정도 정해진 틀이 있기 때문에 상대적으로 쉬워진다.
    * REST를 활용하면 RPC(SOAP의 일반적인 활용)가 아닌 Resource에 대한 CRUD로 정리해야 함.
  * Java에선 RPC를 위해 RMI(Remote Method Invocation)란 기술을 제공한다.
    * [RPC](https://ko.wikipedia.org/wiki/%EC%9B%90%EA%B2%A9\_%ED%94%84%EB%A1%9C%EC%8B%9C%EC%A0%80\_%ED%98%B8%EC%B6%9C)
    * [RMI](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94\_%EC%9B%90%EA%B2%A9\_%ED%95%A8%EC%88%98\_%ED%98%B8%EC%B6%9C)

REST에선 표현을 다뤄야 하고, 이를 위해 데이터를 담는 것 외엔 사실상 아무 것도 하지 않아서 제대로 된 객체라고 볼 수 없는 (하지만 Java에선 어쩔 수 없이 class를 활용해서 쓸 수 밖에 없는) 특별한 객체를 사용하게 된다.

* [“무기력한 도메인 모델” 안티패턴](../architecture/anemicDomainModel.md)

</details>

<details>

<summary>DTO</summary>

DTO(DTO는 Data Transfer Object의 약자)는 일반적으로 데이터 전송을 목적으로 사용되는 객체입니다. 주로 데이터베이스와 애플리케이션 간의 데이터 교환에 사용됩니다. DTO는 데이터를 캡슐화하고 전송하는 데 필요한 필드를 포함하는 단순한 데이터 컨테이너 클래스입니다. DTO를 사용하는 이유는 데이터 전송의 효율성과 보안을 개선하는 데 도움이 됩니다.

일반적으로 DTO 클래스는 다음과 같은 특징을 갖습니다:

1. **필드 (Fields)**: DTO 클래스는 데이터를 저장하는 필드(멤버 변수)를 포함합니다. 이 필드는 전송하려는 데이터의 구조와 형식을 반영합니다.
2. **게터와 세터 (Getters and Setters)**: DTO 클래스는 각 필드에 접근하기 위한 게터(Getter)와 설정하기 위한 세터(Setter) 메서드를 가질 수 있습니다.
3. **직렬화 가능 (Serializable)**: DTO 객체는 직렬화(Serialization)를 지원하여 데이터를 이진 형식으로 변환하고 전송할 수 있어야 합니다.
4. **무엇을 나타내는지 명확한 이름**: DTO 클래스의 이름은 어떤 데이터를 나타내는지 명확해야 합니다. 예를 들어, "UserDTO"는 사용자 정보를 나타내는 DTO 클래스의 이름일 수 있습니다.

DTO는 주로 웹 서비스, RESTful API, 데이터베이스와의 상호작용, 클라이언트와 서버 간의 데이터 교환 등과 관련된 작업에서 사용됩니다. 이를 통해 데이터의 일관성과 보안을 유지하면서 데이터를 효과적으로 전송할 수 있습니다.

* 아주 단순하게 보면 setter와 getter로만 이뤄짐.
* JavaBeans에서 유래한 Java Bean 또는 그냥 Bean이라고 부르는 형태에 가까움(Spring Bean, POJO와 다름에 주의!).
* 제대로 된 객체가 아니라 그냥 무기력한 데이터 덩어리. C/C++ 등에선 구조체(struct)로 구분할 수 있지만, Java에선 불가능. 최신 Java에선 record를 활용할 수 있지만, 오래된 Bean 관련 라이브러리에선 지원하지 않음.

</details>

<details>

<summary>JavaBeans</summary>

[JavaBeans](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EB%B9%88%EC%A6%88)

[Introduction to the Spring IoC Container and Beans](https://docs.spring.io/spring-framework/docs/6.0.x/reference/html/core.html#beans-introduction)

자바빈즈(JavaBeans)는 자바 프로그래밍 언어에서 재사용 가능한 소프트웨어 컴포넌트를 구현하기 위한 규칙 및 관례를 제공하는 표준 방식입니다.

자바빈즈는 자바 객체로써 특정한 관례를 따르는 클래스를 가리키며, 주로 그래픽 사용자 인터페이스(GUI) 구성 요소나 데이터 저장 및 전달을 위한 데이터 객체로 사용됩니다.

1. **구조와 특징**:
   * 자바빈즈는 특정한 관례를 따르는 자바 클래스로, 다음과 같은 특징을 갖습니다:
     * 기본 생성자 (파라미터 없는 생성자)를 가지고 있어야 합니다.
     * 필드(속성)는 private으로 선언하고, getter 및 setter 메서드로 접근합니다.
     * 직렬화(Serialization)를 지원하도록 `Serializable` 인터페이스를 구현할 수 있습니다.
     * 이벤트를 처리하는 리스너 메서드를 지원할 수 있습니다.
2. **용도**:
   * 자바빈즈는 주로 그래픽 사용자 인터페이스(GUI) 요소 (예: 스윙 컴포넌트)를 구현할 때 사용됩니다.
   * 데이터 전송과 데이터 저장을 위한 데이터 객체로 사용됩니다. 이를 통해 데이터를 캡슐화하고 쉽게 전송하거나 저장할 수 있습니다.
3. **자바빈즈 관례**:
   * 자바빈즈의 관례를 따르는 클래스는 이름이 "Bean"으로 끝나며, 관련된 속성에 대한 getter와 setter 메서드가 일반적으로 제공됩니다.
   * 예를 들어, "PersonBean" 클래스는 "name" 속성에 대한 "getName()" 및 "setName()" 메서드를 가집니다.
4. **Spring 프레임워크와 자바빈즈**:
   * Spring 프레임워크에서 자바빈즈는 애플리케이션 컨텍스트에서 사용되는 객체로 자주 활용됩니다.
   * Spring은 이러한 자바빈즈 객체를 관리하고 의존성 주입(Dependency Injection)을 통해 애플리케이션 컨텍스트에서 활용합니다.

자바빈즈는 자바에서 컴포넌트 기반 개발 및 데이터 관리를 단순화하고 객체 지향 설계 원칙을 따르도록 도와주는 중요한 개념 중 하나입니다. 이를 통해 코드의 재사용성과 유지보수성을 향상시키고, 개발자 간의 협업을 용이하게 합니다.

</details>

<details>

<summary>EJB(Enterprise JavaBeans)</summary>

EJB (Enterprise JavaBeans)는 자바 기반의 엔터프라이즈(기업환경) 애플리케이션 개발을 위한 서버 측 컴포넌트 모델 및 스펙입니다.

EJB는 자바 기술의 일부로서, 분산 애플리케이션 개발을 간소화하고 관리를 용이하게 하는데 사용됩니다. EJB는 다양한 엔터프라이즈 애플리케이션에서 사용되며, 주로 비즈니스 로직을 서버 측 컴포넌트로 분리하고 분산 처리 및 트랜잭션 관리를 지원합니다.

Enterprise JavaBeans(EJB)는 독립한 부품이 아닌, 미국 Sun Microsystems사가 제창한 규약이다. EJB는 서버 어플리케이션의 개발을 용이하게해 다양한 Platform과 제품간의 이동성을 실현하기 위하여 비지니스로직과 시스템 서비스를 이용하는 로직을 분산해 그 사이의 규약을 규정하고 있다.

비지니스 로직을 탑제한 부품을 "Enterprise Bean"이라고 불린다. Database처리, Transaction처리등의 시스템 서비스를 이용한 로직을 감추고 있는 부품을 "컨테이너"라고 불린다.

<img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&#x26;fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbdGAPu%2FbtqFkIyIeic%2FULXTzFbnkJR4kZlVJWIDpk%2Fimg.jpg" alt="EJB Components" data-size="original">

1. **종류**:
   * EJB는 여러 종류가 있으며, 주요 종류로는 Session Bean, Entity Bean, 그리고 Message-Driven Bean이 있습니다.
   * Session Bean은 클라이언트와 상호작용하기 위한 논리를 가진 컴포넌트로, Stateless와 Stateful 두 가지 유형이 있습니다.
   * Entity Bean은 데이터베이스와 연결되는 영속적인 데이터를 표현하기 위한 컴포넌트입니다. (EJB 3.0부터는 대부분의 경우 JPA를 사용하여 대체)
   * Message-Driven Bean은 비동기 메시지를 처리하는 데 사용됩니다.
2. **분산 애플리케이션 지원**:
   * EJB는 분산 애플리케이션을 위한 기술로서, 원격 호출 및 분산 트랜잭션 관리를 지원합니다. 이를 통해 여러 서버에서 실행 중인 EJB 컴포넌트 간에 투명한 통신이 가능하고, 분산 환경에서도 트랜잭션을 관리할 수 있습니다.
3. **트랜잭션 관리**:
   * EJB는 트랜잭션을 자동으로 관리하며, 분산 환경에서도 ACID(원자성, 일관성, 고립성, 지속성) 트랜잭션을 제공합니다.
4. **보안 및 권한**:
   * EJB는 보안 및 권한 관리를 지원하며, 엔터프라이즈 애플리케이션에서 사용자 권한 및 인증을 간편하게 처리할 수 있습니다.
5. **스케일링 및 확장성**:
   * EJB 컨테이너는 스케일링과 확장성을 지원하며, 여러 EJB 인스턴스를 관리하여 고부하를 처리할 수 있습니다.
6. **XML 기반 설정**:
   * EJB 컴포넌트는 XML 파일을 사용하여 설정할 수 있으며, 이를 통해 애플리케이션의 설정을 외부화할 수 있습니다.

EJB는 초기 버전에서는 복잡하고 무거웠지만, EJB 3.0부터는 간소화되고 개발자 친화적으로 변경되었습니다. 현재는 EJB 3.2가 최신 버전이며, Java EE (Java Platform, Enterprise Edition) 스펙의 일부로 제공되며, 엔터프라이즈 애플리케이션 개발을 지원합니다. EJB를 사용하면 엔터프라이즈 애플리케이션의 개발과 관리를 효율적으로 수행할 수 있습니다.

* EJB에는 [ORM](../database/orm.md)기술인 엔티티빈 기술을 가지고 있었다.
* Spring 컨테이너와 마찬가지로 EJB의 엔티티빈 기술을 후에 Hibernate가 등장하였고, 현재 JPA 표준 인터페이스의 구현체 중 가장 많이 사용되고 있다.
* 하이버네이트 ORM(Hibernate ORM)은 자바 언어를 위한 객체 관계 매핑 프레임워크이다.
* 객체 지향 도메인 모델을 관계형 데이터베이스로 매핑하기 위한 프레임워크를 제공한다.

[도움이 되는 블로그 링크](https://rainbow97.tistory.com/entry/JAVA-EJBEnterprise-JavaBeans)

[J2EE Engine EJB Architecture - SAP](https://help.sap.com/saphelp\_gbt10/helpdata/DE/c8/844ceb153a134ab5e9a17a54bbc067/content.htm?no\_cache=true)

</details>

<details>

<summary>Tier간 통신</summary>

* F/E와 B/E 사이
  * DTO 자체를 그대로 전송할 수는 없고, 직렬화(마샬링)를 통해야 한다.
  * 어떤 직렬화 기술을 사용할 건지도 결정해야 함. → XML, JSON
* B/E와 DB 사이
  * 아주 옛날에는 Value Object를 DTO란 의미로 썼지만, 재빨리 Transfer Object라고 정정함. 아직도 한국의 오래된 SI 기업에서는 VO(Value Object)를 DTO란 의미로 사용( DAO와 VO를 쓰고 있다면 대부분 여기에 속함).
  * JPA를 지양하고 DDD를 따르는 사람 중 일부는 ORM(JPA, 하이버네이트)은 Active Record + DTO처럼 접근.
  * 아샬은 Kotlin과 Exposed를 쓸 때도 이렇게 접근함.

“Data Transfer”란 측면에 집중하면, “원격(remote)”이 아닌 경우에도 DTO를 사용할 수 있다.

</details>

<details>

<summary>Serialization / Marshalling</summary>

[직렬화](https://ko.wikipedia.org/wiki/%EC%A7%81%EB%A0%AC%ED%99%94)

[마샬링(컴퓨터 과학)](https://ko.wikipedia.org/wiki/%EB%A7%88%EC%83%AC%EB%A7%81\_\(%EC%BB%B4%ED%93%A8%ED%84%B0\_%EA%B3%BC%ED%95%99\))

객체를 그 자체로 DB에 저장하거나 네트워크로 전송하는 건 불가능. 객체를 복구할 수 있도록 데이터화하는 게 필요함. 바이너리라면 Byte Stream, 텍스트라면 기계가 파싱할 수 있고 사람도 읽을 수 있는 형태를 사용. XML, JSON, YAML 같은 형식이 인기.

[JavaBeans](https://ko.wikipedia.org/wiki/%EC%9E%90%EB%B0%94%EB%B9%88%EC%A6%88)의 관례에 직렬화요소가 있는 이유: 클래스의 상태를 지속적으로 저장 혹은 복원시키기 위해

* 직렬화(Serialization): 역직렬화(Deserialization)를 통해 객체 또는 데이터의 복사본을 만들 수 있음.
* 마샬링(Marshalling): 직렬화와 같거나, 원격 객체로 복원할 수 있음. 원격 객체의 경우 메서드 호출은 RPC(또는 RMI)가 됨.

**직렬화**와 **마샬링**은 거의 같지만, Java에선 마샬링을 특수하게 다룸. Java에서의 직렬화(Serialization)와 마샬링(Marshalling)은 비슷한 개념이지만 다소 차이가 있습니다.

1. 직렬화(Serialization):
   * Java에서 객체를 이진 형태로 변환하여 저장하거나 네트워크를 통해 전송할 수 있는 프로세스를 의미합니다.
   * `java.io.Serializable` 인터페이스를 구현한 클래스의 객체를 직렬화할 수 있습니다.
   * 주로 객체의 상태를 저장하거나 전송하는 데 사용됩니다.
   * Java 객체를 Java 직렬화 형식으로 저장하거나 전송할 때 사용됩니다.

```java
// 직렬화 예제
MyClass obj=new MyClass();
        ObjectOutputStream out=new ObjectOutputStream(new FileOutputStream("serializedObject.ser"));
        out.writeObject(obj);
        out.close();
```

2. 마샬링(Marshalling):
   * 마샬링은 객체를 일반적으로 이진 형태로 변환하여 저장하거나 전송하는 작업을 의미합니다. 이 용어는 주로 분산 시스템 및 네트워크 통신에서 사용됩니다.
   * 마샬링은 객체의 상태를 외부 형식으로 변환하고 다른 시스템 또는 프로그래밍 언어에서 읽을 수 있도록 만드는 작업입니다.
   * 마샬링은 Java 객체를 **다른 언어 또는 형식** 으로 변환할 때 사용될 수 있습니다.

Java의 직렬화는 Java 객체를 Java 직렬화 형식으로 저장하거나 전송하는 것을 의미하며, 객체의 상태를 유지합니다. 반면에 마샬링은 Java 객체를 다른 프로그래밍 언어 또는 외부 형식으로 변환할 때 사용되며, 상호 운용성과 데이터 교환을 목적으로 합니다.

예를 들어, 웹 서비스에서 JSON 또는 XML과 같은 외부 데이터 형식으로 데이터를 전송할 때, Java 객체를 해당 형식으로 변환하는 과정은 마샬링의 한 예입니다.

</details>

<details>

<summary>JSON</summary>

#### JSON (JavaScript Object Notation)

[JSON](https://en.wikipedia.org/wiki/JSON)

[JSON 개요](https://www.json.org/json-ko.html)

[JSON으로 작업하기](https://developer.mozilla.org/ko/docs/Learn/JavaScript/Objects/JSON)

JavaScript Good Parts로 유명한 Douglas Crockford가 만든 데이터 포맷. **사람이 읽기 쉽고**, **기계도 해석 또는 생성하기 쉽다**. 보안 문제만 없다면 JavaScript에서 그대로 사용하는 것도 가능하지만, 대부분 JSON.parse(역직렬화)와 JSON.stringify(직렬화)로 안전하게 사용한다.

```json
{
  "Influencers": [
    {
      "name": "Jaxon",
      "age": 42,
      "Works At": "Tech News"
    },
    {
      "name": "Miller",
      "age": 35,
      "Works At": "IT Day"
    }
  ]
}
```

**JSON은 임시 데이터의 저장에 적합합니다.** 예를 들어, 웹사이트에 제출된 양식과 같은 사용자 생성 데이터는 임시 데이터입니다. JSON은 또한 모든 유형의 프로그래밍 언어를 위한 데이터 포맷으로 사용될 수 있기 때문에 고도의 상호 운용성을 제공합니다.

JavaScript의 object는 기본적으로 key-value 쌍이다(심지어 Array도 제한된 key-value + length 관리에 불과함). Java는 Map이 이와 유사하지만, 스키마 관리 및 타입 안전성을 위해 DTO를 활용한다.

* 생성: DTO (Java 세계) → 변환기 → JSON 문자열
* 해석: JSON 문자열 → 변환기 → DTO (Java 세계)

Java에선 [Jackson](https://github.com/FasterXML/jackson)이란 도구가 유명하고, Spring Boot에서 Web 의존성을 추가하면 바로 사용할 수 있다(즉, 우리는 딱히 아무 것도 안 해도 된다).

변환할 때 getter 사용. @JsonProperty로 속성 이름(key) 지정 가능. 다른 객체(DTO)를 포함하고 있어도 됨.

[JSON 문서 데이터베이스](https://www.oracle.com/kr/database/what-is-json/#document-database)

</details>

<details>

<summary>JSON 스키마로서의 DTO (class)</summary>

DTO는 여러 곳에서 사용될 수 있고, 그 의미는 계속 확대됨. 예를 들어 Tier, 즉 Remote 통신이 아닌 상황인 Layer 사이나 내부 객체를 감춘 공개 인터페이스를 만들 때도 DTO를 활용. “데이터 전송”이기만 하면 딱히 틀리지 않다.

우리가 여기서 쓰는 건 JSON 스키마로서의 DTO. 이게 DTO의 전부라고 생각하지 말고, DTO를 쓰는 다양한 상황을 상상해 보자. 이는 “DTO 변환을 어디에서 해야 하나요?” 같은 질문과 연결된다.

DTO는 데이터 전송에 국한되지 않고 다양한 상황에서 사용될 수 있는 다목적 도구로 생각할 수 있습니다. 어떤 상황에서 DTO를 사용하고 어떻게 변환하느냐는 구체적인 요구 사항과 아키텍처에 따라 다를 수 있으며, 이러한 선택은 프로젝트의 복잡성과 목표에 따라 다를 수 있습니다.

\*[JSON 응용 및 장단점](https://inpa.tistory.com/entry/JAVA-%E2%98%95-%EC%A7%81%EB%A0%AC%ED%99%94Serializable-%EC%99%84%EB%B2%BD-%EB%A7%88%EC%8A%A4%ED%84%B0%ED%95%98%EA%B8%B0)

</details>
