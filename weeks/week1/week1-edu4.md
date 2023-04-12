# Java HTTP Server


### HttpServer Library
- 이 전에는 소켓 클래스와 서버소켓 클래스를 이용하여 HTTP 서버를 구성한 반면 좀 더 구현하기 쉽게 자바에서는 HttpServer라는 라이브러리를 제공한다. 
- 이 라이브러리를 사용하여 HTTP 서버를 간단하게 구축해보자.
- 해당 라이브러리는 내부적으로 NIO 즉, 논-블로킹을 사용한다는데 이 형태가 자바 애플리케이션으로써 코드를 구현 후 어떻게 재현하여 동작하는지는 아직 감이 안온다.

#### 서버 객체 준비 (Listen)

> - InetSocketAddress은 IP 소켓 주소뿐 아니라 hostname과 portname으로도 구성이 가능하다고 한다.
> - HttpServer 클래스를 사용하여 서버의 주소와 포트를 정한다.
```java
InetSocketAddress address = new InetSocketAddress(8080);
HttpServer httpServer = HttpServer.create(address, 0);
```

#### Path 핸들러 지정
- 클라이언트는 각기 다른 액션을 취해서 요청을 보낼텐데 이것을 서버에서는 url을 구분하여 요청들을 핸들링한다.
- 예를 들어서 localhost/boards와 localhost/users 처럼 서버에서는 url의 리소스를 구분하여 핸들링한다고 보면 될 것 같다.
- 실습 코드는 아래와 같다.

> - 이렇게 각각의 라우터를 생성하면 요청 URL에 따라서 각기 다른 동작을 구현할 수 있다.
```java
httpServer.createContext("/boards", (exchange) -> {
  String content = "boards\n";
  byte[] bytes = content.getBytes();

  exchange.sendResponseHeaders(200, bytes.length);

  OutputStream responseBody = exchange.getResponseBody();
  responseBody.write(bytes);
  responseBody.flush();
});

httpServer.createContext("/users", (exchange) -> {
  String content = "users\n";
  byte[] bytes = content.getBytes();

  exchange.sendResponseHeaders(200, bytes.length);

  OutputStream responseBody = exchange.getResponseBody();
  responseBody.write(bytes);
  responseBody.flush();
});
```

> 실행화면
![ex_screenshot](./../../resources/images/javaHttpServer-image.png)
