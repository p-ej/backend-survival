# Jackson ObjectMapper


- 이 도구, 매핑을 해줌 DTO랑 JSON
- 이것을 사용해 DTO를 JSON(문자열)혹은 JSON(문자열)을 DTO 
- 정확히는 JSON 노드를 사용 (toString)
- 먼저 DTO 클래스 사용
- JSON 스키마 작성하는 느낌으로 

 JSON 스키마
{
	“id”: “1234”,
	“title”: “제목”,
	“content”: “test”
}

Java 코드 작성
getter 메서드 이름을 따른다.
- getter 이름이랑 상관없이 다른 이름으로 하고 싶다면 @JsonProperty(이름) 데코레이터를 사용하면 된다.
Spring DI 를 통해 컨트롤러에서  잭슨 오브젝트매퍼를 얻을것이다. 
스프링이 등록된 객체(Bean)을 관리하고 있고 생성자에 명시하면 받아서 사용 가능함
사용 = 의존성/의존관계를 주입 받는다. 

* 컴포넌트 (스프링이 관리하는)
    * 스프링이 컴포넌트 붙어있는애들을 자동으로 검사 (시작할때)
    * 스프링은 컴포넌트를 검사해서 ioc에 넣어줌 
    * 미리 객체 준비해줌

나중에 도메인 모델과 DTO를 사용할때 ???

우리가 Jackson을 바로 사용할 수 있는것은 spring boot starater-web에 내장되어 있다. 
- 왜 지원해줄까 ?
- 
@GetMapping("/{id}")
public String detail(@PathVariable String id) throws JacksonException {
    PostDto postDto = new PostDto("1234", "제목", "test");
    String json = objectMapper.writeValueAsString(postDto);

    return json;
}


사실 위 코드처럼 할 필요는 없다. 

@GetMapping("/{id}")
public PostDto detail(@PathVariable String id) {
    PostDto postDto = new PostDto(id, "제목", "test");

    return postDto;
}


반환타입을 PostDto로 하고 인스턴스를 생성한 변수를 return하기만 해도 API를 호출했을때 json타입으로 자동변환되어 응답된다. 

왜 ????????????? 일까 

@GetMapping
public List<PostDto> list() {
    List<PostDto> postDtos = List.of(
            new PostDto("1", "dd", "zzz"),
            new PostDto("2", "aa", "ggg")
    );
    return postDtos;
}



curl -X POST localhost:8080/posts -d ‘{“title”: “제목”}’ -H ‘Content-Type: application/json’

데이터를 전달하는 POST 에서도 마찬가지로 잭슨 오브젝트 매퍼를 사용 가능 

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



## 학습 키워드
- Jackson ObjectMapper 란
- ObjectMapper
- `@JsonProperty`