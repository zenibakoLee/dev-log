# HTTP

## Hypertext Transfer Protocol

**GitBook tip:** 하이퍼텍스트를 빠르게 교환하기 위한 프로토콜의 일종으로, 서버와 클라이언트의 사이에서 어떻게 메시지를 교환할지를 정해 놓은 규칙이다. 교통시스템과 그 법규와 비슷한 개념으로 느껴짐.

* 클라이언트-서버 모델\
  말그대로 고객과 서비스제공자(serve+er)의 관계.\
  클라이언트가 요청을 하고 서버가 그에 따라 준비된 것을 제공한다.\

* Stateless : HTTP는 상태를 갖지 않는다. 모든 요청이 독립적이다. 따라서

1. 요청과 응답을 통해 계속 주고 받는 쿠키
2. 데이터는 서버에서 관리하고 쿠키 등으로 key를 관리하는 세션
3. 웹 브라우저의 기능 (localStorage 등) 등을 통해 클라이언트는 자신의 state를 서버에 알려야 한다.

HTTP 메세지는 사람이 읽을 수 있는 형태, Non-binary이다. 전송속도에 최적화되어있지는 않다.\
HTTP 메세지의 동일한 요청/응답 구조: Start line / Headers / blank line / Body\
Start line은 요청/응답의 형태가 다르다.

메세지 body 특성:

1. 크기를 알기 어렵다. Headers의 Content-Length 항목 등을 활용한다.
2. 위와 다르게 꼭 사람이 읽을 수 있는 텍스트 형태일 필요는 없다. 바이너리 등 가능.
3. 하나가 아니라 여럿일 수도 있다. 파일 업로드 등을 위해 쓰이는 multipart/form-data가 대표적.

<details>

<summary>OSI 7 계층</summary>

1계층부터 기반이 되어 7계층까지 구성됨. (하부 계층이 구성되어야 상부 계층이 구성 가능)

2계층 - 데이터 링크 계층 ⇒ MAC address

3계층 - 네트워크 계층 ⇒ IP address

4계층 - 전송 계층 → TCP, UDP ⇒ Port number

7계층 - 응용 계층 → HTTP 등

\*HTTPS를 위한 TLS 같은 보안 계층이 먼저 들어갈 수도 있다.

HTTPS는 암호화 프로토콜을 사용하여 통신을 암호화한다.\
이 프로토콜은 이전에는 보안 소켓 계층(SSL)으로 알려졌지만, 전송 계층 보안(TLS)이라고 불린다.\
이 프로토콜은 비대칭 공개 키 인프라로 알려진 것을 사용하여 통신을 보호.\
이 유형의 보안 시스템에서는 두 개의 서로 다른 키를 사용하여 두 당사자 간의 통신을 암호화.

</details>

<details>

<summary>HTTP Method &#x26; Status Code</summary>

HTTP Method (요청)

1. GET → Read
2. HEAD → GET without body
3. POST → Submit (멱등성X) ⇒ Collection Pattern에서 Create로 사용
4. PUT → Update (+Create) ⇒ Overwrite!
5. PATCH → Update (partial) (멱등성X)
6. DELETE → Delete
7. OPTIONS → 지원 확인

HTTP Status Code (응답)

1. 1xx → 정보 ⇒ 우리가 직접 쓰는 일은 드믐.
2. 2xx → 성공 ⇒ 200 OK, 201 Created, 204 No Content
3. 3xx → 리다이렉션 ⇒ 304 Not Modified가 특수한 형태로 자주 보임.\
   4\. 영구 리다이렉션: 특정 리소스의 URI가 영구적으로 이동 5. 301 (Moved Permantly): 리다이렉트시 요청 메서드가 GET으로 변하고, 본문(body)이 제거될 수 있음\
   6\. 308 (Permanet Redirect): 301과 기능은 같고, 메서드와 본문이 유지됨 5. 일시 리다이렉션 6. 302 (Found): 리다이렉트시 요청 메서드가 GET으로 변경되기도 하고, 본문이 제거될 수 있음 6. 307 (Temporary Redirect): 302와 기능은 같음, 리다이렉트시 메서드와 본문 유지 7. 303 (See Other): 302와 기능은 같음, 리다이렉트시 요청 메서드가 무조건 GET으로 변경 6. 304 Not Modified: 캐시를 목적으로 사용, 클라이언트에게 리소스가 수정되지 않았음을 알려준다.\
   \-> 클라이언트는 로컬 PC에 저장된 캐시를 재사용(캐시로 리다이렉트 한다.)\
   \*304 응답은 응답에 메시지 바디를 포함하면 안됨 ( 로컬 캐시를 사용하니까 )
4. 4xx → 클라이언트 쪽 문제 ⇒ 404 Not Found
5. 5xx → 서버 쪽 문제 ⇒ 500 Internal Server Error

</details>

<details>

<summary>TCP/IP 통신</summary>

TCP(Transmission Control Protocol) / IP(Internet Protocol)

현재 수많은 프로그램들이 인터넷으로 통신하는 데 있어 가장 기반이 되는 프로토콜. 실제 대다수 프로그램은 TCP와 IP로 통신(정확히는 '네트워킹')하고 있다.

"TCP/IP 지원"이라 써있으면 단순히 인터넷에 연결하여 쓰는 기능이 포함되어 있다고 해석해도 충분

보통 하나로 싸잡아 표현하긴 하나 TCP와 IP는 별개이다. 네트워크의 경우 계층이 정의되어 있고 각 계층마다 하는 역할과 책임지는 영역이 나뉘어져 있기 때문에 묶어서 표현한다는 것뿐이지 역할에는 많은 차이가 있다. 당장 둘만 봐도 TCP(4,전송계층)와 IP(3,네트워크계층)는 완전히 다른 계층이다.

**TCP와 UDP**

전송 계층의 대표적인 프로토콜

* TCP: 연결이 필요함. 전달 및 순서 보장. (전화)
* UDP: 연결하지 않고 데이터를 보냄. 전달 및 순서를 보장하지 않음. (편지)

Socket

* [Berkeley\_sockets](https://en.wikipedia.org/wiki/Berkeley\_sockets)
* [네트워크\_소켓](https://ko.wikipedia.org/wiki/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC\_%EC%86%8C%EC%BC%93)

Socket과 Socket API를 구분해야 헷갈리지 않는다.

Socket은 기본적으로 파일과 유사하게 다룰 수 있다(유닉스에서는 파일 디스크립터의 일종).

Java에서는 키보드 입력, 화면 출력, 파일 입출력 등과 마찬가지로 Stream(Java 8에서 도입된 Stream API가 아님에 주의!)으로 다룰 수 있다.

#### TCP 통신 순서

1. 서버는 접속 요청을 받기 위한 소켓을 연다. → Listen
2. 클라이언트는 소켓을 만들고, 서버에 접속을 요청한다. → Connect
3. 서버는 접속 요청을 받아서 클라이언트와 통신할 소켓을 따로 만든다. → Accept
4. 소켓을 통해 서로 데이터를 주고 받는다. → Send & Receive ⇒ 반복!
5. 통신을 마치면 소켓을 닫는다. → Close ⇒ 상대방은 Receive로 인지할 수 있다.

</details>

<details>

<summary>URI/URL/URN</summary>

리소스를 식별하는 방법.

식별할 때는 식별자(Identifier = ID)를 활용.

URI은 크게 둘로 나뉨:

1. URL(Uniform Resource Locator) → 리소스의 위치. 위치 변경에 취약함. (ex. 배송지)
2. URN(Uniform Resource Name) → 리소스의 “유니크”한 이름. 사실상 쓰이지 않음. (ex. 주민등록번호)

URI라고 쓰는 건 대부분 URL을 의미함. URN 쓰는 걸 거의 본 적이 없음. 따라서 URI와 URL을 크게 구별하지 않고 동의어에 가깝게 사용함.

<img src="https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1b143a39-0445-4906-baca-25633217e5c0_1539x1536.jpeg" alt="URI/URL" data-size="original"><img src="https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1b143a39-0445-4906-baca-25633217e5c0_1539x1536.jpeg" alt="URI/URL/URN" data-size="original">

</details>

<details>

<summary>Collection Pattern in URI design</summary>

1. 그룹화 및 복수형 사용: 여러 리소스를 하나의 그룹으로 묶어 복수형으로 표현합니다. 예를 들어, 게시물을 모두 "posts"라는 그룹으로 묶어서 표현합니다. 이렇게 하면 게시물 목록을 나타내는 URI가 " /posts"로 표현됩니다.
2. 요소와 컬렉션 구분: 개별 리소스를 나타내는 URI와 해당 리소스 그룹을 나타내는 URI를 구분합니다. 개별 게시물의 URI는 "/posts/{id}" 형태로 표현되고, 컬렉션은 "/posts"로 표현됩니다.
3. 하위 리소스: 특정 리소스의 하위 리소스를 나타내기 위해 디렉터리 구조처럼 사용할 수 있습니다. 예를 들어, 게시물의 댓글을 표현할 때 "/posts/{post\_id}/comments"와 같이 사용하며, 댓글의 개별 URI는 "/posts/{post\_id}/comments/{id}"와 같이 표현됩니다.
4. 편집 페이지: 리소스의 편집 페이지를 나타낼 때도 URI를 사용하며, "/posts/{id}/edit"와 같이 표현할 수 있습니다. 그러나 REST API의 경우 편집 페이지를 표현할 필요가 없는 경우도 있습니다.

</details>

<details>

<summary>MIME (content,media) type</summary>

MIME (Multipurpose Internet Mail Extensions) 타입은 인터넷에서 전자 메일 및 웹에서 다양한 미디어 유형을 식별하고 나타내는 데 사용되는 표준화된 방법입니다. MIME 타입은 데이터의 형식을 정의하며, 클라이언트 및 서버 간에 어떤 종류의 데이터가 전송되는지 알려줍니다. MIME 타입은 HTTP, 이메일 시스템 및 다른 네트워크 프로토콜에서 널리 사용됩니다.

MIME 타입은 보통 다음과 같은 형식을 가집니다:

```
type/subtype
```

여기서:

* `type`: 주요 미디어 유형을 나타내며, 예를 들어 "text", "image", "audio", "video", "application" 등이 있습니다.
* `subtype`: 주요 유형 내에서 미디어의 하위 유형을 나타냅니다. 예를 들어 "plain" (평문 텍스트), "html" (HTML 문서), "jpeg" (JPEG 이미지) 등이 있습니다.

MIME 타입의 몇 가지 예시:

* `text/plain`: 평문 텍스트, E-mail에서 자주 사용
* `text/html`: HTML 문서, 일반적인 웹 문서
* `image/jpeg`: JPEG 이미지
* `application/pdf`: PDF 문서
* `audio/mpeg`: MP3 오디오 파일
* `video/mp4`: MP4 동영상 파일
* `application/json`: JSON 데이터, 자기서술적이기에 굉장히 어렵다.
* `application/xml`: XML 데이터, 범용. 자기서술적이기에 상대적으로 어렵다.
* `text/css`
* `text/javascript`
* `application/atom+xml`
* `application/dns+json`

HTTP Headers에 `Content-Type` 속성으로 전달함. [IANA](https://www.iana.org/assignments/media-types/media-types.xhtml)에 등록된 목록을 참고하자.

</details>

<details>

<summary>쿠키와 세션</summary>

* 저장 위치:
  * HTTP 쿠키: 쿠키는 클라이언트 측에 저장됩니다. 즉, 브라우저가 사용자의 컴퓨터에 쿠키를 저장하고, 이를 웹 서버에 전달합니다.
  * 세션: 세션 데이터는 웹 서버에 저장됩니다. 클라이언트에는 세션 ID만 저장되며, 이 세션 ID를 사용하여 서버에서 세션 데이터를 검색합니다.
* 수명:
  * HTTP 쿠키: 쿠키는 설정된 만료 날짜 또는 세션 종료 시간까지 지속됩니다. 브라우저를 닫아도 지정된 만료 날짜까지 유지될 수 있습니다.
  * 세션: 세션 데이터는 일반적으로 사용자가 웹 사이트를 떠날 때 또는 일정 시간(세션 타임아웃) 후에 파기됩니다.
* 용도:
  * HTTP 쿠키: 주로 사용자의 로그인 상태, 사용자 환경 설정 및 사용자 추적과 같은 상태 정보를 저장하는 데 사용됩니다.
  * 세션: 주로 사용자의 로그인 상태와 관련된 데이터를 저장하고 유지하는 데 사용됩니다.
* 보안:
  * HTTP 쿠키: 쿠키는 클라이언트 측에 저장되므로 보안상 취약할 수 있으며, 중요한 데이터를 저장할 때 적절한 보안 조치가 필요합니다. HTTPS를 사용하여 통신을 암호화하는 것이 중요합니다.
  * 세션: 세션 데이터는 서버 측에서 유지되므로 더 안전합니다. 그러나 세션 ID를 안전하게 관리해야 하며, HTTPS를 사용하는 것이 중요합니다.
* 저장 용량:
  * HTTP 쿠키: 쿠키는 클라이언트 측에서 저장되므로 브라우저의 저장 용량에 제한이 있으며, 한 도메인에서 저장할 수 있는 쿠키의 수에도 제한이 있습니다.
  * 세션: 세션 데이터는 서버 측에서 저장되므로 저장 용량에 더 큰 유연성이 있습니다.

</details>

<details>

<summary>multipart/form-data</summary>

multipart/form-data는 웹 폼 데이터를 웹 서버로 전송하는 데 사용되는 인코딩 방식 중 하나입니다.\
이 방식은 파일 업로드와 같이 바이너리 데이터를 함께 전송해야 하는 경우에 주로 사용됩니다. 주요 특징은 다음과 같습니다:

1. 바이너리 데이터 전송: multipart/form-data를 사용하면 텍스트 데이터 외에도 이미지, 동영상, 오디오 및 기타 바이너리 데이터를 전송할 수 있습니다. 이를 통해 파일 업로드와 같은 작업을 수행할 수 있습니다.
2. 인코딩: multipart/form-data는 폼 데이터를 멀티파트(Multipart) 메시지로 인코딩하여 전송합니다. 이 메시지는 각 파트(part)로 구성되며, 각 파트는 텍스트나 바이너리 데이터로 이루어질 수 있습니다.
3. Boundary: multipart/form-data 메시지는 각 파트를 구분하기 위한 구분자(boundary)를 사용합니다. 이 구분자는 파트의 시작과 끝을 식별하는 역할을 합니다.
4. 서버 처리: 웹 서버는 multipart/form-data 메시지를 파싱하여 각 파트의 데이터를 추출하고 처리합니다. 이러한 처리를 통해 파일 업로드 및 다른 데이터 전송 작업을 수행할 수 있습니다.
5. 웹 프레임워크 지원: 대부분의 웹 프레임워크 및 라이브러리는 multipart/form-data를 처리하고 파일 업로드를 지원합니다. 이러한 도구를 사용하면 웹 애플리케이션에서 이러한 데이터를 쉽게 다룰 수 있습니다.

multipart/form-data를 사용하는 예시로 HTML 폼 요소를 통해 파일을 업로드할 때,

요소의 enctype 속성을 "multipart/form-data"로 설정하여 해당 폼이 이 인코딩 방식을 사용하도록 지정할 수 있습니다. 아래는 간단한 HTML 폼의 예시입니다

```html

<form action="upload.php" method="post" enctype="multipart/form-data">
    <input type="file" name="fileToUpload">
    <input type="submit" value="Upload File">
</form>

```

위의 예제에서 "enctype"가 "multipart/form-data"로 설정되었으므로 이 폼은 파일 업로드를 지원하며, 웹 서버는 multipart/form-data로 전송된 데이터를 파싱하여 파일 업로드를 처리합니다.

```java
//client
  LinkedMultiValueMap<String, Object> body=new LinkedMultiValueMap<>();

        // 주의! spring의 org.springframework.core.io.Resource 클래스 타입을 사용함!
        // java.io.File 사용하면 안된다!!!
        // new UrlResource("file:" + "D:/uploadFile/123.jpg"); 대체가능
        Resource resource1=new FileSystemResource("D:/uploadFile/123.jpg");

        // new UrlResource("file:" + "D:/uploadFile/testImg.jpg");
        Resource resource2=new FileSystemResource("D:/uploadFile/testImg.jpg");

        body.add("files",resource);
        body.add("files",resource2);
        // body.add("wow", "this is amazing"); // 문자열도 전송 가능하다 😁

        HttpHeaders headers=new HttpHeaders();
        headers.setContentType(MediaType.MULTIPART_FORM_DATA);

        HttpEntity<LinkedMultiValueMap<String, Object>>httpEntity
        =new HttpEntity<>(body,headers);

        String serverUrl="http://localhost:8080/test/multipart";

        ResponseEntity<JsonNode> postForEntity
        =REST_TEMPLATE.postForEntity(serverUrl,httpEntity,JsonNode.class);
//sever
@PostMapping("/test/multipart")
public ResponseEntity<Map<String, String>>testMultipart(List<MultipartFile> files){
        files.forEach(file->{
        System.out.println(file.getContentType());
        System.out.println(file.getOriginalFilename());
        });
        HashMap<String, String> resultMap=new HashMap<>();
        resultMap.put("result","success");
        return ResponseEntity.ok(resultMap);
        }
```

</details>

<details>

<summary>CRUD &#x26; CQS</summary>

데이터에 대해 취하는 모든 기능은 4가지로 나눌 수 있다.

1. CREATE
2. READ
3. UPDATE
4. DELETE

동작에 의한 상태의 변화 유무를 기준으로 나누면

* Command => Create,Update,Delete: 상태가 변함. 안전하지 않음.
* Query => Read : 상태가 변하지 않음. 안전함. 분산,캐시등이 수월함.

**HTTP Method**

Collection Pattern과 HTTP Method를 이용해 CRUD를 표현할 수 있다.

1. GET → Read
2. POST → Create
3. PUT, PATCH → Update
4. DELETE → Delete

**게시물**

1. `GET /posts` → 게시물 목록 (Read, Collection) → List (습관적인 표현 중 하나)
2. `GET /posts/{id}` → 게시물 상세 (Read, Element) → Detail (습관적인 표현 중 하나)
3. `POST /posts` → 게시물 생성 (Create) → Post ID는 서버에서 생성함.
4. `PUT 또는 PATCH /posts/{id}` → 게시물 수정 (Update, Element)
5. `DELETE /posts/{id}` → 게시물 삭제 (Delement, Element)

종종 Bulk update, Bulk delete 등을 하기도 함. 이럴 때는 Collection을 활용하고, API 스펙 문서에 정확히 기록.

**댓글**

그냥 바로 comments로 시작하는 경우:

1. `GET /comments` → 전체 댓글 목록
2. `GET /comments?post_id={post_id}` → 특정 게시물에 대한 댓글 목록
3. `GET /comments/{id}` → 댓글 상세
4. `POST /comments` → 댓글 생성 ⇒ 어떤 게시물에 대해? → Body에 Post ID 정보를 담아줘야 함.
5. `POST /comments?post_id={post_id}` → 특정 게시물에 대한 댓글 생성
6. `PUT 또는 PATCH /comments/{id}` → 댓글 수정
7. `DELETE /comments/{id}` → 댓글 삭제

특정 게시물 아래로 표현하는 경우:

1. `GET /posts/{post_id}/comments` → 특정 게시물에 대한 댓글 목록
2. `GET /posts/{post_id}/comments/{id}` → 특정 게시물에 달린 특정 댓글 상세
3. `POST /posts/{post_id}/comments` → 특정 게시물에 대한 댓글 작성
4. `PUT 또는 PATCH /posts/{post_id}/comments/{id}` → 특정 게시물에 달린 특정 댓글 수정
5. `DELETE /posts/{post_id}/comments/{id}` → 특정 게시물에 달린 특정 댓글 삭제

**로그인/로그아웃**

로그인과 로그아웃을 어떻게 리소스로 표현할 수 있을까? 이럴 때는 추상적인 개념인 “세션”을 도입하곤 한다.

1. `POST /session` → 세션 생성 = 로그인
2. `DELETE /session` → 세션 파괴 = 로그아웃

덤으로…

1. `GET /session` → 세션 확인 → 내 정보 확인?
2. `GET /users/me` → User ID를 me라고 쓰면, 현재 사용자의 User ID로 처리하게 정하고, API 스펙 문서에 기록.

</details>

<details>

<summary>CORS</summary>

CORS (Cross-Origin Resource Sharing)는 웹 보안 정책 중 하나로, 다른 도메인(출처)에서 리소스에 접근하는 것을 제어하는 브라우저 정책.

CORS는 웹 애플리케이션에서 다른 도메인의 API 또는 서버로 요청을 보낼 때, 보안 상의 이슈를 방지하고 동일 출처 정책 (Same-Origin Policy)을 허용하는 방법을 제공합니다.

<img src="https://docs.aws.amazon.com/ko_kr/sdk-for-javascript/v3/developer-guide/images/cors-overview.png" alt="cors-aws" data-size="original">

CORS (Cross-Origin Resource Sharing)는 웹 보안 정책 중 하나로, 다른 도메인(출처)에서 리소스에 접근하는 것을 제어하는 브라우저 정책입니다. CORS는 웹 애플리케이션에서 다른 도메인의 API 또는 서버로 요청을 보낼 때, 보안 상의 이슈를 방지하고 동일 출처 정책 (Same-Origin Policy)을 허용하는 방법을 제공합니다.

1. **동일 출처 정책 (Same-Origin Policy)**:
   * 웹 보안의 핵심 원칙 중 하나로, 스크립트가 다른 출처의 리소스에 접근하는 것을 제한합니다.
   * 동일 출처 정책은 보안을 강화하기 위한 것이지만, 웹 애플리케이션에서 외부 도메인의 데이터 또는 서비스에 접근할 필요가 있는 경우 이를 제한합니다.
2. **JSONP (JSON with Padding)**:
   * JSONP는 JSON 데이터를 가져오기 위한 다른 출천에서 스크립트를 로드하는 방법입니다.
   * JSONP는 보안에 취약할 수 있으며, CORS와 비교하여 덜 안전합니다.
3. **Access-Control-Allow-Origin**:
   * CORS를 사용하여 다른 출천의 요청을 허용하는 방법 중 하나입니다.
   * 서버 응답 헤더에 `Access-Control-Allow-Origin` 헤더를 추가하여 다른 출처의 도메인을 명시적으로 허용합니다.

```java
@GetMapping("/")
public List<PostDto> list(HttpServletResponse response){
        response.addHeader("Access-Controll-Allow-Origin","http://localhost:3000"); // front request origin
        return postDtos;
        }
```

4. **`@CrossOrigin` 어노테이션**:
   * `@CrossOrigin` 어노테이션은 Spring Framework에서 제공하는 것으로, 서버 측에서 CORS 정책을 관리하고 특정 컨트롤러 또는 핸들러 메서드에 적용할 수 있습니다.
   * `@CrossOrigin` 어노테이션을 사용하면 특정 출처에서의 요청을 허용하고, 허용할 출처를 설정할 수 있습니다.
   * 이를 통해 서버 측에서 CORS 정책을 관리하고 클라이언트와 안전하게 데이터를 교환할 수 있습니다.

```java
 @CrossOrigin("")
@GetMapping("/{id}")
public String detail(@PathVariable String id)throws JacksonException{
        PostDto postDto=new PostDto(id,"test title","test content");
        String json=objectMapper.writeValueAsString(postDto);
        return json;
        }
```

CORS와 `@CrossOrigin` 어노테이션은 웹 애플리케이션에서 다른 도메인과의 통신을 안전하게 허용하는 중요한 보안 기능입니다. 이를 사용하여 웹 애플리케이션의 데이터 접근성을 향상시킬 수 있습니다.

</details>

<details>

<summary>여러 데이터를 보내는 멀티 파트</summary>

Multipart: 여러 다른 종류의 데이터를 수용하는 방법 http 메시지 바디 내부에 엔티티를 여러개 포함시켜 보낼 수 있다.

주로 이미지나 텍스트 파일 등을 업로드할 떄 사용됨.

* mulipart/form-data: web form으로부터 파일 업로드에 사용됨 application/x-www-form-urlencoded 은 폼의 가장 기본적인 타입

```html

<form action="/save" method="post" enctype="multipart/form-data">
    <input type="text" name="username"/>
    <input type="text" name="age"/>
    <input type="file" name="file1"/>
    <button type="submit">전송</button>
</form>
```

파일을 전송할 때 파일만 '띡' 전송하는 경우는 드물고 다른 정보도 함께 전송하는 경우가 많다.

username,age,file1 을 다 같이 보내야 하는 경우.

http request header의 Content-type은 한 type만 명시할 수 있기 때문에

application/x-www-form-urlencoded 와 image/jpeg 을 알려줄 방법이 없다.

그래서 등장하는 것이 multipart 타입.

```http
POST /save HTTP/1.1
Host: localhost:8080
Content-Type: multipart/form-data; boundary=-----%%%
Content-Length: 12309

-----%%%
Content-Disposition: form-data; name="username"

steve
-----%%%
Content-Disposition: form-data; name="age"

25
-----%%%
Content-Disposition: form-data; name="file1"; filename="profile.png"
Content-Type: image/png

2q432#@$%#%#@REWFfsf3e3f3@4ewR$f4r4wF4gs4t4obvy734or84r4v#%Y^&B$%^$%&E^%$^@%C%$QRTA$f4btwRWwa3rw3r ...
-----%%%
```

* multipart/byteranges: 상태코드 206(Partial Content) 리스폰스 메시지가 복수 범위의 내용을 포함하는 때 사용됨

</details>
