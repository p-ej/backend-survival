# Jackson ObjectMapper


### Jackson ObjectMapper는?
- DTO랑 JSON이랑 매핑을 해주는 라이브러리
- DTO 객체에서 JSON 혹은 JSON -> DTO
  - 심층분석 정확히는 JSON 노드를 사용한다고 한다. (toString)
- 먼저 DTO 클래스 사용하고 JSON으로 변환해보자

JSON 스키마 형태는 아래와 같다.
```json
{
	“id”: “1234”,
	“title”: “제목”,
	“content”: “test”
}
```

#### Java에서 DTO를 Json 스키마 형태로 작성할때
- getter 메서드 이름을 따른다.
  - getId() -> { id: } **(프로퍼티는 ID가 됨)**
- getter 이름이랑 상관없이 다른 이름으로 하고 싶다면 @JsonProperty(이름) 데코레이터를 사용하면 된다.
- 우리는 Spring DI 를 통해 컨트롤러에서 Jackson ObjectMapper를 얻을 수 있따.
  - 스프링이 등록된 객체(Bean)을 관리하고 있고 생성자에 명시하면 받아서 사용 가능함
  - 사용힌디 = 의존성/의존관계를 주입 받는다. 

* 컴포넌트 (스프링이 관리하는)
  * 스프링이 컴포넌트 붙어있는애들을 자동으로 검사 (시작할때)
    - `@Component` 라는 어노테이션을 스프링이 쭈르륵 스캔함.
  * 스프링은 컴포넌트를 검사해서 ioc에 넣어줌 
  * 미리 객체 준비해줌

나중에 도메인 모델과 DTO를 사용할때 ???

- 우리가 Jackson 라이브러리를 따로 Gradle에 등록안하고 바로 사용할 수 있는것은 Gradle에 등록되어 있는 spring boot starater-web에 내장되어 있다. 
  - 왜 지원해줄까 ? (알아보기~)


> Jackson ObjectMapper를 사용한 코드는 아래와 같다. 
``` java
@GetMapping("/{id}")
public String detail(@PathVariable String id) throws JacksonException {
    PostDto postDto = new PostDto("1234", "제목", "test");
    String json = objectMapper.writeValueAsString(postDto);

    return json;
}
```
- 이런식으로 라이브러리의 함수를 적절히 사용한다.

   
- **사실 위 코드처럼 할 필요는 없다.**

```java
@GetMapping("/{id}")
public PostDto detail(@PathVariable String id) {
    PostDto postDto = new PostDto(id, "제목", "test");

    return postDto;
}
```

> 반환타입을 PostDto로 하고 인스턴스를 생성한 변수를 return하기만 해도 API를 호출했을때 json타입으로 자동변환되어 응답된다. 
   
(왜 ????????????? 일까)

``` java
@GetMapping
public List<PostDto> list() {
  List<PostDto> postDtos = List.of(
    new PostDto("1", "dd", "zzz"),
    new PostDto("2", "aa", "ggg")
  );
  return postDtos;
}
```

- CLI API 테스트
` curl -X POST localhost:8080/posts -d ‘{“title”: “제목”}’ -H ‘Content-Type: application/json’ `
   
- 데이터를 전달하는 POST 에서도 마찬가지로 잭슨 오브젝트 매퍼를 사용 가능 
   
``` java
@PostMapping
@ResponseStatus(HttpStatus.CREATED)
public String create(@RequestBody String body) throws JsonProcessingException {
    // 얻어온거 변환
    PostDto postDto = objectMapper.readValue(body, PostDto.class);

    return postDto.getTitle();
}


@PostMapping
@ResponseStatus(HttpStatus.CREATED)
public PostDto create(@RequestBody PostDto postDto) {

    return postDto;
}
```

- 상당히 흥미롭다. DTO 객체를 JSON으로 변환해주는 부분과 그것을 가능하게 하는 라이브러리 ...
- 아직은 더 봐야 알겠지만 스프링 자체에서 편의를 위해 지원하는 기능도 상당한것 같다.
  - 예를 들면 ObjectMapper 라이브러리를 사용하지 않고도 JSON 형태로 응답이 가능하다는 것. 
  - 결국은 스프링에서 자동으로 변환되게끔 어노테이션이 있단 얘기인듯

## 학습 키워드
- Jackson ObjectMapper 란
- ObjectMapper
- `@JsonProperty`



[![Hits](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction.svg)](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction/)