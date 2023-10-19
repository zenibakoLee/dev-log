# HTTP

## Hypertext Transfer Protocol

**GitBook tip:** 하이퍼텍스트를 빠르게 교환하기 위한 프로토콜의 일종으로, 서버와 클라이언트의 사이에서 어떻게 메시지를 교환할지를 정해 놓은 규칙이다.
교통시스템과 그 법규와 비슷한 개념으로 느껴짐.

<details>

<summary>OSI 7 계층</summary>
1계층부터 기반이 되어 7계층까지 구성됨. (하부 계층이 구성되어야 상부 계층이 구성 가능)

2계층 - 데이터 링크 계층 ⇒ MAC address

3계층 - 네트워크 계층 ⇒ IP address

4계층 - 전송 계층 → TCP, UDP ⇒ Port number

7계층 - 응용 계층 → HTTP 등

*HTTPS를 위한 TLS 같은 보안 계층이 먼저 들어갈 수도 있다.

HTTPS는 암호화 프로토콜을 사용하여 통신을 암호화한다.<br />
이 프로토콜은 이전에는 보안 소켓 계층(SSL)으로 알려졌지만, 전송 계층 보안(TLS)이라고 불린다.<br />
이 프로토콜은 비대칭 공개 키 인프라로 알려진 것을 사용하여 통신을 보호.<br />
이 유형의 보안 시스템에서는 두 개의 서로 다른 키를 사용하여 두 당사자 간의 통신을 암호화.

</details>


Stateless : HTTP는 상태를 갖지 않는다. 모든 요청이 독립적이다.따라서

1. 요청과 응답을 통해 계속 주고 받는 쿠키
2. 데이터는 서버에서 관리하고 쿠키 등으로 key를 관리하는 세션
3. 웹 브라우저의 기능 (localStorage 등) 등을 통해 클라이언트는 자신의 state를 서버에 알려야 한다.

HTTP 메세지는 사람이 읽을 수 있는 형태, Non-binary이다. 전송속도에 최적화되어있지는 않다.
<br />HTTP 메세지의 동일한 요청/응답 구조: Start line / Headers / blank line / Body<br />
Start line은 요청/응답의 형태가 다르다.

메세지 body 특성:

1. 크기를 알기 어렵다. Headers의 Content-Length 항목 등을 활용한다.
2. 위와 다르게 꼭 사람이 읽을 수 있는 텍스트 형태일 필요는 없다. 바이너리 등 가능.
3. 하나가 아니라 여럿일 수도 있다. 파일 업로드 등을 위해 쓰이는 multipart/form-data가 대표적.

<details>

<summary>HTTP Method & Status Code</summary>
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
3. 3xx → 리다이렉션 ⇒ 304 Not Modified가 특수한 형태로 자주 보임.
4. 4xx → 클라이언트 쪽 문제 ⇒ 404 Not Found
5. 5xx → 서버 쪽 문제 ⇒ 500 Internal Server Error

</details>

<details>

<summary>TCP/IP 통신</summary>
TCP(Transmission Control Protocol) / IP(Internet Protocol)

현재 수많은 프로그램들이 인터넷으로 통신하는 데 있어 가장 기반이 되는 프로토콜. 실제 대다수 프로그램은 TCP와 IP로 통신(정확히는 '네트워킹')하고 있다.

"TCP/IP 지원"이라 써있으면 단순히 인터넷에 연결하여 쓰는 기능이 포함되어 있다고 해석해도 충분

보통 하나로 싸잡아 표현하긴 하나 TCP와 IP는 별개이다. 네트워크의 경우 계층이 정의되어 있고 각 계층마다 하는 역할과 책임지는 영역이 나뉘어져 있기 때문에 묶어서 표현한다는 것뿐이지 역할에는 많은 차이가 있다.
당장 둘만 봐도 TCP(4,전송계층)와 IP(3,네트워크계층)는 완전히 다른 계층이다.

#### TCP와 UDP

전송 계층의 대표적인 프로토콜

- TCP: 연결이 필요함. 전달 및 순서 보장. (전화)
- UDP: 연결하지 않고 데이터를 보냄. 전달 및 순서를 보장하지 않음. (편지)

Socket

- [Berkeley_sockets](https://en.wikipedia.org/wiki/Berkeley_sockets)
- [네트워크_소켓](https://ko.wikipedia.org/wiki/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC_%EC%86%8C%EC%BC%93)

Socket과 Socket API를 구분해야 헷갈리지 않는다.

Socket은 기본적으로 파일과 유사하게 다룰 수 있다(유닉스에서는 파일 디스크립터의 일종).

Java에서는 키보드 입력, 화면 출력, 파일 입출력 등과 마찬가지로 Stream(Java 8에서 도입된 Stream API가 아님에 주의!)으로 다룰 수 있다.

### TCP 통신 순서

1. 서버는 접속 요청을 받기 위한 소켓을 연다. → Listen
2. 클라이언트는 소켓을 만들고, 서버에 접속을 요청한다. → Connect
3. 서버는 접속 요청을 받아서 클라이언트와 통신할 소켓을 따로 만든다. → Accept
4. 소켓을 통해 서로 데이터를 주고 받는다. → Send & Receive ⇒ 반복!
5. 통신을 마치면 소켓을 닫는다. → Close ⇒ 상대방은 Receive로 인지할 수 있다.

</details>

<details>

<summary>HTTP 클라이언트</summary>
-CONNECT</br>
호스트는 IP 주소 또는 도메인 이름을 사용할 수 있다. <br />
도메인의 경우 DNS를 활용하기 때문에 제대로 하려면 복잡해질 수 있지만, 알아서 처리해 준다.<br />
HTTP의 기본 포트 번호는 80.</br>
IP 주소와 포트 번호만 알면, 서버에 접속할 수 있다.</br>
Socket socket = new Socket(host, port);</br>

-REQUEST</br>
요청 메시지를 만들고, TCP로 전송하면 된다.</br>

```
GET http://example.com/ HTTP/1.1
(빈 줄)
```

소켓에서 Output Stream을 얻어서 쓸 수 있다.</br>
그러나 Byte변환없이 문자열을 바로 전송하고 싶다면 Writer(OutputStreamWriter)를 쓴다(추천).</br>
⚠️ 내부적으로 버퍼가 있기 때문에 flush를 잊지 않아야 한다.

-RESPONSE</br>
소켓에서 Input Stream을 얻어서 쓸 수 있다.</br>
Byte 배열을 준비하고, 여기로 데이터를 읽어온다.</br>
응답 데이터가 우리가 준비한 배열보다 클 수도 있는데, 이 경우엔 반복해서 여러 번 읽는 작업이 필요하다. </br>
이 경우엔 우리가 준비한 배열이 Chunk(한번에 처리하는 단위)가 된다.

```
byte[] bytes = new byte[1_000_000];
int size = inputStream.read(bytes);
```

System.out.println(text);
실제 데이터 크기만큼 Byte 배열을 자르고, 문자열로 변환해 출력한다.

```
byte[] data = Arrays.copyOf(bytes, size);
String text = new String(data);
```

요청과 마찬가지로 Reader(InputStreamReader)를 쓰면 훨씬 편하다(추천).

-CLOSE

```
socket.close();
```

Socket은 Closeable이기 때문에 try-with-resources를 써도 된다.

```
try (Socket socket = new Socket(host, port)) {
	// Request
	// Response
}
```

</details>