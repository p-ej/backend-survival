# HTTP Server

### HTTP 서버 구현
- 이번 목차는 실습을 통한 학습
구현 전 서버의 흐름은 다음과 같다
1. Listen : 접속 요청을 받기 위한 소켓 열기
2. Request : 접속 요청을 받아 통신 소켓을 따로 만든다.
3. Response : 2번과 같이 Send & Receive를 반복한다. 
4. Close : 통신이 끝나면 소켓을 닫는다.

### 소켓 프로그래밍
- 서버와 클라이언트간의 1:1 소켓 통신
- 결국은 서버가 먼저 실행되어서 통신을 대기하고 있는 상태여야 한다. 
- Sockect은 InputStream과 OutputStream을 가지고 있어서 I/O 작업이 이루어진다.
- ServerSocket은 Listen의 역할을 하는 소켓이다. 즉, 포트를 받아 통신을 열어놓고 외부의 연결요청을 기다리는 소켓이라고 보면된다.

### 소켓을 사용한 HTTP 서버 구현

#### Listen
- 서버는 요청을 받기만 하면 되는것이라 포트 번호만 정한다.
> - 포트 번호를 지정하고 ServerSockect 인스턴스를 생성한다.
> - Socket이랑은 별도의 독립적인 소켓 클래스이다.
```java
int port = 8080;
ServerSocket listener = new ServerSocket(port, 0); // 기본 50
```

#### Accept
> 여기서 Socket 클래스를 사용하여 클라이언트가 접속하면 소켓을 만든다. (통신용 소켓)
```java
Socket socket = listener.accept();
```

- Listen 같은 대기 상태를 블로킹(Blocking)이라고 한다. 
- TCP 통신 같은 경우 네트워크 상태나 클라이언트 요청이 없으면 무한정 기다려야 하는 등의 일이 벌어진다. (서버 자원 낭비)

#### Request & 처리
- 서버의 역할은 클라이언트의 요청을 받는 것이다. 
- 그래서 Stream으로부터 요청을 읽어야 하는 동작을 구현해야한다. 
```java
Reader reader = new InputStreamReader(socket.getInputStream());
CharBuffer charBuffer = CharBuffer.allocate(1_000_000);

reader.read(charBuffer);

charBuffer.flip();

System.out.println(charBuffer.toString()); // 여기서 얻은걸 가지고 url 분석을 해서 들어오는 리소스를 나눌수도 있다.
```

#### Response
- 서버는 클라이언트의 요청을 응답해주어야 한다.
- 응답 메세지를 만들어서 전송한다.
- 응답 메세지에는 시작줄, 헤더, 빈 줄, 바디(Option)을 포함한다.

> 기본적인 형태
```
HTTP/1.1 200 OK
(빈 줄)
Hello, world!
```

> - 헤더와 바디를 나누고 바디에는 HTML 형태로 전송하게끔 문자열을 담는다.
> - 또한 message에는 기본적인 헤더 정보를 담기 위해 Content-Type, charset, Content-Length을 추가한다.
```java
String body = """
  <!DOCTYPE html>
    <HTML lang="ko">
      <HEAD>
        <title>예제</title>
      </HEAD>
  <BODY>
    <p> Hello, World! </p>
  </BODY>
</HTML>
""";

byte[] bytes = body.getBytes();

String message = """
HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8
Content-Length:""" + bytes.length + "\n" + "\n" + body;
// text/plain 은 또 다름 (html 문서형식이 아니기 때문)


Writer writer = new OutputStreamWriter(socket.getOutputStream());
writer.write(message);
writer.flush();
```

#### Close 소켓 닫기
- 응답을 마쳤으면 사용했던 통신 소켓을 닫는다.
```java
socket.close();
```


+ 지금 구현한 코드는 한 번의 요청으로 서버가 종료된다. 만약 다른 요청도 계속 받고 싶다면 Accept 부분이 시작되기 전 While 문을 사용하면 된다.


### 학습 키워드
* Java ServerSocket
* Blocking vs Non-Blocking