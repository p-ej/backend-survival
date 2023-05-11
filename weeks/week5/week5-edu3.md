Spring Test

학습 키워드
* 통합 테스트(Integration Test)
* @Autowired
* @SpyBean
* MockMvc
* MockBean
* @WebMvcTest




유닛 테스트랑 비교해서 통합 테스트라고도 함 
테스트 구분은 여러 방법이있음
스프링 공식문서는 크게 둘로 나눔
- 단위
- 통합

스프링 IoC 컨테이너 
스프링 부트에서 자동으로 내어주는, 그것을 끌어다 쓸 수 있도록 내어줌 

조금 더 모킹하는 법 몇가지 
모킹도 많이 할 필욘 없음 (단위테스트는)

대신 통합 테스트는 중요함

스프링은 코드에 구조적으로 개입하는게 적다. ,단위테스트 쉽게 작성 가능 

스프링의 힘을 빌려서 테스트하는 건 IoC컨테이너를 적극적으로 활용하고 싶거나 스프링 웹 MVC로 구현된 부분을 테스트하고 싶을 떄 즉 스프링에서 통합테스트라고 부르는 경우임.
- 스프링 웹 MVC
- 내가 구성안한것들이 많이 들어옴, DB, JPA 내가잡아준게 없음. 이때 스프링의 힘을 빌림

방법 여러
스프링 부트 1.4 (현재 3버전대) StpringBootTest 어노테이션으로 쉽게 테스트 가능 

기존에는 의존 관계들을 다 생성자로 주입받음. 
- 그런데 필드로 받아올것임 
- 테스트 같은경우 스프링 IoC에서 관ㄹ리하는게 아니기 떄문에 필드로 바로 가능 
- Value (값 가져다 쓸때)
- Autowired 의존성 주입할때 

￼

포스트에 대한 피처 테스트

restTemplate 실제로 뭔가 요청하는 걸 할 수  있음 .

@Value("${local.server.port}")
server.port를 하면 리소스 프로퍼티에 있는 개발용 포트가 들어감 


@SpringBootTest(webEnvironment = WebEnvironment.RANDOM_PORT)
public class PostFeatureTest {
    @Value("${local.server.port}")
    private int port;

    @Autowired // 테슽 할때만 만들어지는 의존성
    private TestRestTemplate restTemplate;

    @Test
    @DisplayName("게시물 목록 확인")
    void list() {
        String url = "http://localhost:" + port + "/posts";

        String body = restTemplate.getForObject(url, String.class);

        assertThat(body).contains("이런 건 없지");
    }
}


나온값하고 내가 원하는 값 비교 


방금 진행한 코드는  인수 테스트와 비슷 
진짜 서버 실행 후 값 가져온다 생각하면 됨 


컨트롤러 테스트 
MockMmvc
- HTTP 요청을 흉내가능함.
- HTTP 요청한거처럼 사용 가능함. 
- 


안에서 뭐 쓰는지 알아야함 .

SpyBean

프록시 방법을 씀
똑같은 목록을 가진 객체를 만듦.

내부에서 Repository 사용하는지 모른다.



전체를 다 올려서 테스트를 진행함 .
여기에 관련된 애들이 객체가 마련되어 들어감 
가볍게하는 방버이 있음
전부 가짜 


MockBean 
WebMvcTest



완전한 통합에 대한 부분들은 PostFeatureTest를 참고
컨트롤러에대해서 작업 애플리케이션 레이어와 잘 소통하는 것 뿐 

가능하면 내 관심사가 무엇인지 

놓칠 수 있는 부분은 ?

유닛텟트는 가능한 전부다 
두 가지를 같이 써야함 
스프링 테스트가 어려움 ..
- 유닛테스트가 더 중요함 


TDD