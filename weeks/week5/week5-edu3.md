# Spring Test


### 통합 테스트 (Spring Test)
- 유닛 테스트랑 비교해서 통합 테스트라고도 함 
- Spring 기반 애플리케이션에서 통합 테스트를 지원하는 Spring Framework에서 제공하는 모듈이다.
- Spring 구성 요소와의 통합 테스트 작성 및 실행을 용이하게 하는 일련의 유틸리티, 주석 및 클래스를 제공한다.


#### 특징
1. 테스트 어노테이션 
    - 통합 테스트의 동작을 구성하고 제어하는 데 사용할 수 있는 주석을 제공한다.
```java
// 일반적으로 사용되는 일부 주석에는 
    `@RunWith(SpringRunner.class)`
    // (스프링 통합 테스트를 위한 테스트 러너 구성), 
    `@SpringBootTest` //(통합 테스트를 위한 Spring 애플리케이션 컨텍스트 로드) 
    `@MockBean`// (스프링 통합 테스트를 위한 빈 모의)이 포함
```

2. Spring Test Context
- 통합 테스트가 Spring 빈 및 기타 애플리케이션 구성 요소에 액세스하고 상호 작용할 수 있도록 하는 테스트별 Spring 애플리케이션 컨텍스트를 생성하고 관리한다.
- 테스트 컨텍스트는 일반적으로 실제 애플리케이션에 정의된 구성 파일 및 빈을 기반으로 한다.

3. 종속성 주입 지원
- Spring 프레임워크의 종속성 주입 기능을 활용하여 통합 테스트가 Spring 빈의 자동 연결 및 종속성 주입의 이점을 누릴 수 있도록한다.
- 테스트 종속성을 쉽게 설정하고 구성할 수 있다.

4. 트랜잭션 관리
- 통합 테스트를 위한 트랜잭션 관리 지원을 제공한다.
- 트랜잭션 경계 내에서 테스트를 실행할 수 있으므로 테스트 중에 변경된 사항이 롤백되므로 후속 테스트를 위해 깨끗한 상태를 유지할 수 있다.

5. 모킹 및 테스트 더블
- Mockito 및 EasyMock과 같은 인기 있는 모킹 프레임워크와 통합되어 모의 객체 생성과 외부 종속성을 위한 테스트 더블을 가능하게 한다.

6. 웹 테스트 지원
- RESTful API 테스트 및 HTTP 요청 및 응답 처리 지원과 같이 Spring으로 구축된 웹 애플리케이션 테스트에 특정한 기능을 제공한다.
- HTTP 요청 및 응답 모의, 컨트롤러 테스트 및 웹 끝점의 동작 확인을 위한 도구를 제공한다.

#### 추가 이야기
기존에는 의존 관계들을 다 생성자로 주입받음. 
- 그런데 이를 필드로 받아올 것이다.
- 테스트 같은경우 스프링 IoC에서 관리하는게 아니기 떄문에 필드로 바로 가능 
- Value (값 가져다 쓸때)
- Autowired 의존성 주입할때 

￼
#### 테스트 실습
- Post에 대한 피처 테스트
- restTemplate 실제로 뭔가 요청하는 걸 할 수  있음 .
```java
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
```
> 나온값하고 내가 원하는 값 비교할 수 있다.


- 방금 진행한 코드는  인수 테스트와 비슷 
- 진짜 서버 실행 후 값 가져온다 생각하면 됨 

유닛텟트는 가능한 전부다 
두 가지를 같이 써야함 
스프링 테스트가 어려움 ..
- !!유닛테스트가 더 중요함 

### 각 어노테이션에 대한 설명
1. @Autowired
    - 빈을 다른 빈으로 자동 종속성 주입하는 데 사용된다.
    - 필드, 생성자 또는 설정자 메서드에 적용할 때 Spring은 자동으로 필요한 빈의 인스턴스를 해당 구성 요소로 해결하고 주입한다.
    - 수동 빈 인스턴스화 및 배선이 필요하지 않아 Spring 애플리케이션에서 종속성을 관리하는 데 편리하다.

2. @SpyBean
    - 통합 테스트에서 Spring 빈의 부분 모의 또는 스파이를 생성하는 데 사용된다.
    - bean의 메소드에 대한 호출을 가로채고 확인할 수 있는 스파이 객체로 실제 bean을 대체할 수 있다.
    - `@SpyBean`을 사용하면 테스트 중에 특정 상호 작용을 주장하고 확인하면서 빈의 원래 동작을 유지할 수 있다.

3. MockMvc
    - 전체 서버를 시작하지 않고 Spring MVC 애플리케이션의 컨트롤러 및 엔드포인트에 대해 HTTP 요청을 시뮬레이트하고 수행하는 방법을 제공한다.
    - MockMvc를 사용하면 웹 레이어에 대한 통합 테스트를 작성하고 요청을 보내고 컨트롤러의 응답 및 동작을 확인할 수 있다.

4. MockBean
    - 통합 테스트를 위해 Spring bean의 모의 구현을 만드는 데 사용된다.
    - 실제 빈을 모의 개체로 대체하여 테스트 목적으로 모의 동작을 정의할 수 있다.
    - `MockBean`은 종속성에서 특정 빈을 분리하고 특정 구성 요소 또는 시나리오 테스트에 집중하려는 경우에 유용하다.

5. @WebMvcTest
    - Spring MVC 애플리케이션의 웹 레이어를 테스트하는 데 사용된다.
    - 테스트 클래스에 적용될 때 컨트롤러, 요청 매핑 및 웹 관련 기능을 테스트하기 위해 특별히 맞춤화된 제한된 Spring 애플리케이션 컨텍스트를 설정한다.
    - `@WebMvcTest`를 사용하면 다른 구성 요소를 제외하고 웹 레이어에 테스트 노력을 집중할 수 있으므로 테스트 속도와 효율성이 향상된다.



## 학습 키워드
- 통합 테스트(Integration Test)
- @Autowired
- @SpyBean
- MockMvc
- MockBean
- @WebMvcTest


[![Hits](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction.svg)](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction/)