# Multipart FormData


### Multipart FormData란
- HTTP 요청의 일부로 인터넷을 통해 데이터를 보내는 데 사용되는 형식이다.
- 파일을 업로드하거나 클라이언트(웹 브라우저 등)에서 서버로 대량의 데이터를 보낼 때 일반적으로 사용된다.
- 데이터를 보낼 때 데이터는 여러 부분 또는 "multipart" 섹션으로 나뉜다.
  - 결국 섹션에는 텍스트 필드, 파일 또는 이진 데이터와 같은 다양한 유형의 데이터가 포함될 수 있다.
  - 데이터의 각 부분은 요청 내에서 부분을 구분하는 고유한 경계 문자열로 식별된다.

> 파일만 보낼 수 있는것이 아닌 텍스트 등등의 다른 데이터도 같이 보낼 수 있다.


#### 특징
- 단일 요청으로 여러 유형의 데이터를 전송할 수 있기 때문에 이 형식을 "멀티파트"라고하는 것.
- 멀티파트 양식 데이터 내의 각 부분에는 전송되는 데이터 유형을 지정하는 Content-Type 헤더를 포함하여 자체 헤더가 있다.
- 서버가 Multipart FormData가 포함된 요청을 수신하면 요청을 구문 분석하여 데이터의 개별 부분에 액세스할 수 있다.
- 서버는 양식 필드를 처리하거나 업로드된 파일을 처리하거나 데이터에 필요한 작업을 수행할 수 있다.



### @ModelAttribute
- Spring MVC 애플리케이션에서 요청 매개변수 또는 양식 데이터를 모델 객체에 바인딩하는 데 사용되는 주석
- 일반적으로 컨트롤러 메서드에서 들어오는 요청의 데이터로 모델 개체를 채우는 데 사용된다.
- HTTP Body 내용과 HTTP 파라미터의 값들을 Getter, Setter, 생성자를  통해 주입하기 위해 사용한다.

#### 특징
- 메소드 매개변수 수준에서 `@ModelAttribute`가 사용되면 매개변수가 요청의 데이터로 채워져야 함
- 개별 메서드 매개변수 또는 전체 메서드에 적용하여 모든 매개변수가 요청에서 바인딩되어야 함을 나타낼 수 있다.

#### 사용예시
- 예를 들어서 아래와 같이 요청을 받는다.
```java
public String create(
  @ModelAttribute UploadImageDto imageDto
)
```

- `UploadImageDto` 클래스의 속성과 일치하는 요청 매개변수 또는 양식 필드를 기반으로 `imageDto` 객체가 요청의 데이터로 채워져야 한다.
- 결국 UploadImageDto 안에는 아래와 같은 필드로 구성될 수 있다.

```java
public record UploadImageDto(
  MultipartFile image
) {
}
```
- 이에 따라서 프론트는 멀티파트로 요청을 할때 각각의 필드 이름에 값을 담아서 전달한다.
- 서버는 @ModelAttribute를 메소드 단위 매개변수로 활용하여 프론트에서 요청받은 데이터를 UploadImageDto에 구성된 필드에 따라 바인딩하게 된다.



## 학습 키워드
- Multipart FormData
- `@ModelAttribute`