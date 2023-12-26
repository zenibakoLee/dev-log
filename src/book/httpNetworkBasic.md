### 그림으로 배우는 Http&Network Basic - 우에노 센 / 이병억 (영진닷컴)

<details><summary>여러 데이터를 보내는 멀티 파트</summary>
Multipart: 여러 다른 종류의 데이터를 수용하는 방법
http 메시지 바디 내부에 엔티티를 여러개 포함시켜 보낼 수 있다. 

주로 이미지나 텍스트 파일 등을 업로드할 떄 사용됨.

- mulipart/form-data: web form으로부터 파일 업로드에 사용됨
  application/x-www-form-urlencoded 은 폼의 가장 기본적인 타입

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

```http request
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

- multipart/byteranges: 상태코드 206(Partial Content) 리스폰스 메시지가 복수 범위의 내용을 포함하는 때 사용됨

</details>