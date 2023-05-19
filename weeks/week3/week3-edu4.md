# CORS

### CORS란

- 브라우저는 보안의 이유로 HTTP 요청들을 제한한다고 한다.
- 프론트에서 서버(REST API)에 요청을 하고 정상적인 응답(200 Status)을 받아도 웹에선 보안 정책때문에 에러가 나올 수 있다.
- 결국엔 서버측에서 응답을 해줄때 "여기(프론트)에서 요청한 곳은 괜찮아"라고 전달해줄만한 무언가가 필요하다.
- 무언가를 전달해줄 때 HTTP 헤더에다가 허락과 거절을 할만한 정보를 실어서 보낸다. 이를 **CORS(Cross-Origin Resource Sharing)** 라고 한다.

> - 서로 다른 도메인의 웹 페이지가 서로 안전하게 통신할 수 있도록 웹 브라우저에 구현된 보안 기능
> - 백엔드는 "a.com"의 웹 페이지가 "api.a.com"의 리소스에 접근할 수 있도록 하는 HTTP 응답에 특정 응답 헤더를 포함해야함.

- 헤더 메시지

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://a.com
(빈 줄)
```

#### Cross-origin
- 다음 중 한가지라도 다른 경우를 말한다.
1. 프로토콜 : http, https
2. 도메인(호스트) : a.com, api.a.com
3. 포트 번호 : 3000과 8000

#### 필요한 이유
- 이런 정책없이 모든 곳에서 데이터를 요청할 수 있게 되면 모방 사이트가 많아질 것이다. 
- 모방 사이트를 이용하여 로그인 정보를 탈취하거나 공격을 가하는 등의 행위가 일어날 수 있다. 
- 브라우저에선 이 행위를 방지하기 위해 보호 정책을 만들고, 필요의 경우에만 서버와 협의해서 요청할 수 있다.

### 동일 출처 정책 (Same-Origin Policy)

- 웹 브라우저 보안을 위해 프로토콜, 호스트, 포트가 동일한 서버로만 요청을 주고 받을 수 있도록 한 정책
- 어떤 리소스 요청 (local8080/posts) 얻으려는 이 리소스 출처가 현재 페이지와 다르면 접근할 수 없게한다.
- 두 URL의 호스트와 포트가 같아야 동일한 출처라고 볼 수 있다.

#### JSONP (JSON with Padding)

- HTML의 script 요소로부터 요청되는 호출에는 보안상 정책이 적용되지 않는다는 점을 이용한 우회 방법이라고 한다.
- `<script>` 태그는 동일 출처를 따지지 않는다는 점을 이용함.
- 서버에서 JSON을 직접 전달하는 게 아니라 실행되는 자바스크립트 코드를 전달하는 방식

```
<script>
function success(data) {
  // ...
}
</script>

<script src="http://server/posts?callback=success"></script>

callback 쿼리 파라미터로 들어온 콜백 함수명([
  { id: '1', title: 's', content: 'adsa'}
]);
```

### @CrossOrigin

- 스프링에선 이렇게 CORS를 해결해 줄 수 있는 객체도 존재한다.
- 클래스나 메서드 단위에 @CrossOrigin 어노테이션을 명시해준다.

```java
@CrossOrigin("*")

@CrossOrigin("http://localhost:3000")

@CrossOrigin
```

- 각각의 클래스나 메서드 단위에 일일이 어노테이션을 지정하기 번거로우니 WebMvcConfigurer라는 커스텀 Bean을 지정해줘서 애플리케이션이 실행될때 미리 등록하도록 CORS를 설정할 수 있다.

```java
@Bean
public WebMvcConfigurer webMvcConfigurer() {
  return new WebMvcConfigurer() {
  @Override
  public void addCorsMappings(CorsRegistry registry) {
    registry
      .addMapping("/**")
      .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
  //  .allowedOrigins("http://localhost:3000")
      .allowedOrigins("*");
    }
  };
}
```

- 이렇게 CORS를 허용할 응답 메서드들을 설정
- URL, 호스트, 포트 등 설정
- **"*"**을 하면 모든 URL에 대해 허용한다는 뜻이다. 



##### ??
- Simple requests인 경우
- preflight 요청일 경우

## 학습 키워드
- CORS 란
  - 동일 출처 정책
  - JSONP
  - `Access-Control-Allow-Origin`
- `@CrossOrigin`



[![Hits](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction.svg)](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction/)