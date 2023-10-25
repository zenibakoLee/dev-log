# Server

<details><summary>HTTP 클라이언트</summary>

-CONNECT</br>
호스트는 IP 주소 또는 도메인 이름을 사용할 수 있다. <br />
도메인의 경우 DNS를 활용하기 때문에 제대로 하려면 복잡해질 수 있지만, 알아서 처리해 준다.<br />
HTTP의 기본 포트 번호는 80.</br>
IP 주소와 포트 번호만 알면, 서버에 접속할 수 있다.</br>
Socket socket = new Socket(host, port);</br>

### REQUEST</br>

요청 메시지를 만들고, TCP로 전송하면 된다.</br>
<details><summary>Java text blocks(from Java13)</summary>여러 줄 문자열: Text blocks를 사용하면 여러 줄에 걸쳐 긴 문자열을 만들 수 있으며, 줄 바꿈 문자와 공백을 직접 지정할 필요가 없습니다.

줄 바꿈 및 들여쓰기: Text blocks 내에서 줄 바꿈과 들여쓰기는 그대로 유지되므로 가독성이 향상됩니다.

문자열 보간: Text blocks 내에서 ${변수}를 사용하여 문자열 보간(변수 삽입)이 가능합니다.</details>

```
String message = """
	GET / HTTP/1.1
	Host: example.com

	""";
```

또는

```
String message = "" +
	"GET / HTTP/1.1\n" +
	"Host: example.com\n" +
	"\n";
```

소켓에서 Output Stream을 얻어서 쓸 수 있다.</br>
그러나 Byte변환없이 문자열을 바로 전송하고 싶다면 Writer(OutputStreamWriter)를 쓴다(추천).</br>
⚠️ 내부적으로 버퍼가 있기 때문에 flush를 잊지 않아야 한다.

### RESPONSE</br>

소켓에서 Input Stream을 얻어서 쓸 수 있다.</br>
Byte 배열을 준비하고, 여기로 데이터를 읽어온다.</br>
응답 데이터가 우리가 준비한 배열보다 클 수도 있는데, 이 경우엔 반복해서 여러 번 읽는 작업이 필요하다. </br>
이 경우엔 우리가 준비한 배열이 Chunk(한번에 처리하는 단위)가 된다.

<details>
<summary>Java InputStream과 OutputStream </summary> 
InputStream과 OutputStream은 Java에서 바이트 기반의 입출력 작업을 수행하기 위한 중요한 추상 클래스입니다. 이러한 클래스는 입출력 스트림을 다룰 때 사용되며, 파일, 네트워크 연결, 메모리
버퍼 등 다양한 데이터 소스 및 대상과 상호작용하는 데 사용됩니다.<br>
 Java의 입출력 작업에서 중요한 역할을 하며, 데이터의 읽기와 쓰기, 버퍼링, 바이트 배열, 문자 인코딩 등을 처리하는 데 유용합니다. 

- InputStream은 바이트 단위로 데이터를 읽어오는 데 사용되는 추상 클래스입니다.
  주로 파일, 네트워크 연결, 시스템 입력 스트림 등에서 데이터를 읽을 때 사용됩니다.
- InputStream의 주요 메서드 중 일부는 read()로 시작하며, 이를 사용하여 데이터를 읽고 반환합니다. 예를 들어, read() 메서드를 사용하여 파일에서 한 바이트씩 데이터를 읽을 수 있습니다

```java
InputStream inputStream=new FileInputStream("파일경로");
        int data=inputStream.read();

``` 

- OutputStream은 바이트 단위로 데이터를 쓰는 데 사용되는 추상 클래스입니다.
  주로 파일, 네트워크 연결, 시스템 출력 스트림 등에 데이터를 쓸 때 사용됩니다.
- OutputStream의 주요 메서드 중 일부는 write()로 시작하며, 이를 사용하여 데이터를 쓰고 저장합니다. 예를 들어, write() 메서드를 사용하여 파일에 바이트 데이터를 쓸 수 있습니다.

</details>

```
InputStream inputStream = socket.getInputStream();
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

```
InputStreamReader reader = new InputStreamReader(socket.getInputStream());

CharBuffer charBuffer = CharBuffer.allocate(1_000_000);

reader.read(charBuffer);

charBuffer.flip();

System.out.println(charBuffer.toString());
```

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
<details><summary>HTTP 서버</summary>

```
int port = 8080;

//Java에서는 ServerSocket이라는 별도의 클래스를 사용한다 (Socket을 상속한 게 아니라, 완전히 구별된다는 점에 주의).
ServerSocket listener = new ServerSocket(port); 

//클라이언트의 접속을 기다린다. 클라이언트가 접속하면 통신용 소켓을 따로 만들어서 돌려준다.
/*
I/O에서 이렇게 기다리는 걸 Blocking이라고 한다. 
파일 읽기, 쓰기 등도 모두 Blocking 동작이지만, TCP 통신에서는 네트워크 상태 같은 요인에 의해 크게 지연될 수 있고,
accept처럼 상대방의 요청이 없으면 영원히 기다리는 일이 벌어질 수 있다. 
멀티스레드나 비동기, 이벤트 기반 처리 등이 필요한 이유다.
*/
Socket socket = listener.accept();

```

<details><summary>ServerSocket class</summary>

ServerSocket은 Java에서 서버 측 네트워크 프로그래밍을 구현하는 데 사용되는 클래스입니다.</br>
ServerSocket 클래스는 서버 측에서 사용됩니다. ServerSocket은 클라이언트의 연결을 수락하면 새로운 Socket 객체를 반환합니다. 이 Socket을 사용하여 클라이언트와의 데이터 통신을
처리합니다.</br>
ServerSocket은 클라이언트로부터의 연결 요청을 수락하고, 클라이언트와의 통신을 위한 소켓 연결을 생성하는 데 사용됩니다.</br>
ServerSocket 클래스는 Socket 클래스를 직접 상속하지 않으며, 서로 상속 관계에 있지 않습니다


</details>

⚠️ 마지막 줄에 Newline(\n)을 넣는 걸 잊지 말자.</br>
제대로 하려면 Content-Type과 Content-Length를 더해주는 게 좋다.</br>
⚠️ Content-Length로 정확한 크기를 알 수 있기 때문에 마지막 줄에 Newline(\n)을 넣지 않아도 된다.

```java
String body="Hello, world!";
        byte[]bytes=body.getBytes();
        String message=""+
        "HTTP/1.1 200 OK\n"+
        "Content-Type: text/html; charset=UTF-8\n"+
        "Content-Length: "+bytes.length+"\n"+
        "\n"+
        body;
```

</details>

<details><summary> Java httpserver</summary>

- [Module jdk.httpserver](https://docs.oracle.com/en/java/javase/17/docs/api/jdk.httpserver/module-summary.html)
- [Package com.sun.net.httpserver](https://docs.oracle.com/en/java/javase/17/docs/api/jdk.httpserver/com/sun/net/httpserver/package-summary.html)
- [Non-blocking_I/O_(Java)](https://en.wikipedia.org/wiki/Non-blocking_I/O_(Java))

내부적으로 NIO를 사용하는 고수준 HTTP 서버 API.
<details><summary>Socket과 HttpServer클래스 서버 리스닝 방식에 따른 차이</summary>

Socket을 사용하는 방식은 저수준의 제어와 다양한 프로토콜을 지원하므로 유연성이 높지만 복잡하며,<br>
HttpServer 클래스를 사용하는 방식은 HTTP 기반의 웹 서버를 빠르게 구현하는 데 유용하지만 HTTP에 한정된다는 제약이 있습니다.<br>
선택은 프로젝트의 요구사항 및 개발자의 선호에 따라 달라질 것입니다.

Socket을 이용한 서버 리스닝 방식:

- 장점:

1. 간단한 구현: HttpServer 클래스를 사용하면 HTTP 프로토콜을 기반으로 한 웹 서버를 간단하게 구현할 수 있습니다.

2. 빠른 시작: HttpServer를 사용하면 웹 서버를 빠르게 시작하고 기본적인 웹 애플리케이션을 만들 수 있습니다.

3. HTTP 관련 기능 지원: HttpServer는 HTTP 메서드 및 헤더를 처리하기 위한 다양한 클래스 및 메서드를 지원합니다.

- 단점:

1. HTTP 프로토콜에 한정: HttpServer는 HTTP 프로토콜에 특화되어 있으며 다른 프로토콜을 처리하기 어렵습니다.

2. 제한된 커스터마이징: HttpServer를 사용하면 빠르게 웹 서버를 구축할 수 있지만, 기본적으로 제공되는 기능에 한정됩니다. 고급 기능을 구현하려면 추가적인 개발 및 확장이 필요할 수 있습니다.

HttpServer 클래스를 이용한 서버 리스닝 방식:

- 장점:

1. 유연성: Socket을 이용하는 서버는 TCP나 UDP 소켓을 직접 다루므로 프로토콜과 연결 관리를 완전히 제어할 수 있습니다. 이것은 다양한 응용 프로그램에 유용합니다.

2. 다양한 프로토콜: Socket을 사용하면 다양한 프로토콜을 지원할 수 있으며, 사용자 정의 프로토콜을 쉽게 구현할 수 있습니다.

- 단점:

1. 저수준 작업: Socket을 사용하는 방식은 상대적으로 저수준 작업이 필요하므로 개발과 유지 관리가 복잡할 수 있습니다.

2. 스레드 관리: 동시 다중 연결을 처리하려면 스레드 또는 다중 프로세스를 사용해야 하며, 이로 인해 스레드 관리 및 동기화 문제가 발생할 수 있습니다

</details>
<details><summary>논블로킹(Non-blocking) I/O</summary>

Java의 HttpServer 클래스는 논블로킹(Non-blocking) I/O를 사용하여 HTTP 요청을 처리하는 데 효과적으로 설계된 클래스입니다.<br> 논블로킹 I/O는 다음과 같은 특징을 가지며, 이
클래스를 통해 HTTP 서버를 만들 때 이점을 제공합니다:

- 비차단적인 처리: 논블로킹 I/O는 I/O 작업이 블로킹하지 않고 비차단적으로 처리됩니다. 이는 I/O 작업을 기다리는 동안 CPU가 다른 작업을 수행할 수 있도록 합니다. 따라서 다수의 연결을 동시에 처리하고
  동시성을 확보할 수 있습니다.
- 스레드 효율성: 논블로킹 I/O를 사용하면 모든 연결에 대해 개별 스레드를 생성하지 않아도 됩니다. 이로써 스레드의 생성 및 관리에 따른 오버헤드를 감소시킬 수 있으며, 메모리 사용을 줄일 수 있습니다.
- 이벤트 주도 아키텍처: 논블로킹 I/O 서버는 이벤트 주도 아키텍처를 사용합니다. 이는 이벤트(예: HTTP 요청 도착)가 발생할 때 적절한 핸들러(콜백 함수)가 호출되어 처리를 수행하게 됩니다. 이벤트 주도
  아키텍처는 복잡한 상태 관리를 필요로 하지 않으며, 비동기 처리를 가능하게 합니다.
- 선택기(Selector) 사용: Java의 HttpServer는 주로 Selector 클래스와 함께 사용됩니다. Selector를 사용하면 다중 I/O 채널을 모니터링하고, 이벤트가 발생한 채널에만 작업을 수행할
  수 있습니다. 이를 통해 논블로킹 I/O를 효율적으로 구현할 수 있습니다.

HttpServer 클래스를 사용하면 이러한 논블로킹 I/O 원칙을 기반으로 간단하게 HTTP 서버를 작성할 수 있습니다. 이는 높은 동시성을 가진 웹 애플리케이션을 만드는 데 유용하며, 블로킹 I/O를 사용하는
전통적인 웹 서버보다 성능과 확장성이 더 뛰어날 수 있습니다.

</details>

```java
//서버 객체 준비
InetSocketAddress address=new InetSocketAddress(8080);
        int backlog=0;
        HttpServer httpServer=HttpServer.create(address,backlog);
//URL(정확히는 path)에 핸들러 지정
        httpServer.createContext("/",(exchange)->{
        // TODO
        });
//Listen
        httpServer.start();
```

### Request

```java
String method=exchange.getRequestMethod();
        System.out.println(method);

        URI uri=exchange.getRequestURI();
        String path=uri.getPath();
        System.out.println(path);

        Headers headers=exchange.getRequestHeaders();
        for(String key:headers.keySet()){
        System.out.println(key+": "+headers.get(key));
        }

        InputStream inputStream=exchange.getRequestBody();
        String body=new String(inputStream.readAllBytes());
        System.out.println(body);
```

### Response

```java
String body="Hello, world!";
        byte[]bytes=body.getBytes();
        exchange.sendResponseHeaders(200,bytes.length);
        OutputStream outputStream=exchange.getResponseBody();
        outputStream.write(bytes);
        outputStream.flush();
```

</details>