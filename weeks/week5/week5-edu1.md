# Dependency Injection


### 스프링의 주요 개념을 알아보자

#### DI (Dependency Injection)
- 스프링에서 상당히 맞닿아 있는 부분이 많다.
- 보통 이것을 IoC랑 엮어서 얘기하는 부분이 많은데 엄밀히 말하면 서로 밀접하지만 둘은 차이점이 존재한다.
- 우선 DI는 `의존성 주입`이다. 스프링의 핵심 기능이며, 소프트웨어 애플리케이션에서 객체 간의 종속성을 관리하는 방법이다.
	- 의존하는 책임을 제거하여 클래스들간의 결합도를 낮추는 것을 목표로 하는 디자인 패턴이라고 한다.
- 우리는 보통 객체 인스턴스를 생성할때 `new` 키워드를 사용한다. 즉, 객체 자체에 의해 생성되거나 획득하는게 아니라 객체의 종속성이 객체에 주입된다. 
	- 내가 이해한바는 한 클래스에서 다른 클래스를 생성한 다음에 생성한 클래스를 이용해서 뭔가를 한다는 느낌보다. 다른 클래스를 조립해서 만든다 ? 라는 의미로 생각하고 있다. (이게 무슨 차이가 있을까는 알아봐야겠다.)
- 스프링 프레임워크는 DI를 쉽고 편하게 해준다. 이것이 프레임워크의 힘이다. 

##### 장점
- 이또한 객체를 분리하고 모듈식 및 재사용 가능한 코드를 촉진한다는 것
- 종속 클래스를 수정하지 않고도 구현을 변경하거나 교체하는 것이 더 쉬워진다.


##### 의존성 주입 방법
- 여러가지 주입 방법이 있는데 우리는 생성자 주입으로 진행할 것이다. (이게 좋다고 한다.)
1. 생성자 주입
	- 종속성은 클래스 생성자를 통해 제공된다.
	- 생성자 매개변수는 필요한 종속성을 나타내며 프레임워크는 객체를 생성할 때 이를 확인하고 주입한다.
2. Setter 주입
3. 필드 주입
4. 인터페이스 주입 

##### Spring DI 활성방법 
- 이또한 2가지가 있는데 XML 파일 구성과 Java 클래스로 구성을 한다.
- 애플리케이션 컨텍스를 구성하고 Bean(객체)과 해당 종속성을 정의하면 프레임워크가 얘네들을 인스턴스화하고 연결한다.


##### 왜 생겨났을까 
- 느슨한 결합과 객체 생성 관리 문제를 해결하기 위해 개발되었다고 한다. 
- DI라는 것이 나오기 이전에는 객체가 종속성을 생성하고 관리하는 역할을 했기에 코드가 밀접하게 결합(결합도 상승)되어 유지 관리나 테스트 및 재사용이 어려웠다. 
	- 결국 유지 보수와 클린코드를 위해 발전해 나간것이다. (효율업업)

#### AOP (Aspect Oriented Programming : 관점 지향 프로그래밍)
- 개발자가 개발한 어떠한 로직을 기준으로 핵심, 부가적 관점으로 나누어 보고 이 관점을 기준으로 모듈화한다는 의미
- 한마디로 개발자의 관점에서 프로그래밍을 한다? 라고 이해하면 될 것 같다. 
	- `어떤 로직에서 실행시간이 얼마나 걸리는가에 대해서는 개발자만 알면되니까.`
- 스프링에서는 AOP를 위한 어노테이션을 지원한다.
- 거의 직접 구현하진 않는다고 한다.
- 아래의 사진은 이해하기 편할 것 같다.
	- AOP를 적용하면 어떤 로직의 실행 전과 실행 후 그리고 그 사이에 메인 로직이 들어감.
   
![AOP 예시](./../../resources/images/AOP.png)
   
##### 왜 생겼을까 ?
- 소프트웨어 개발에서 교차 절단 문제를 해결하기 위해 만들어졌다고한다. 
- 교차 절단 문제는 응용 프로그램의 여러 부분에 영향을 미치고 기본 논리 모듈 또는 구성 요소를 가로지르는 경향이 있는 기능 또는 특징을 지칭한다. 
- 예로는 로깅, 보안, 트랜잭션 관리, 오류 처리 및 성능 모니터링이 있다.


##### Aspect 
- 핵심 개념은 aspect이라고 한다.
- 특정 조인 포인트(Aspect 동작을 적용해야 하는 애플리케이션의 특정 지점) 및 어드바이스가 적용되어야 하는 조인 포인트를 지정하는 포인트컷에 대해 수행할 작업을 나타내는 일련의 어드바이스를 정의한다.
	- `개발자가 적용할 관점의 포인트를 지정하는 부분같다.`

> 이것또한 코드 중복을 줄이고 관심사 분리를 개선하여 더 깨끗하고 유지 관리하기 쉬운 코드를 위함.
   
----

### 싱글턴 패턴
- 적용을 할 수도 안할수도 있는 패턴이다.
- 클래스의 인스턴스화를 단일 인스턴스로 제한한다.
	- 객체 인스턴스 생성을 한번만 하고 이것을 전역으로 재사용할 수 있게 공유한다는 것 같다.
- 애플리케이션이 살아있는 동안(라이프 사이클) 생성한 클래스의 인스턴스가 하나만 존재하도록 한다.
	- 스프링은 Bean 안에서 관리되는 객체를 모두 싱글턴으로 생성해준다고 한다. (!!!!)



Factory 
	⁃	정적 팩토리 메소드 
	⁃	객체를 만드는 것들에 대한 것을 이야기함
	⁃	메서드 하나 , 메소드를 모아놓은 오브젝트일 수 있음
	⁃	팩토리 오브젝트

객첼 직접 만들지 않고 객체를 생성하는 책임만 가진 객체를 만들어서 쓴다. (SRP 단일책임원칙
	⁃	보통 new를 사용해서 인스턴스 생성함. 
PostDAO 또는 PostRepository의 구현이 여럿이라 유용하게 사용 가능
객체를 모두 팩토리에서 얻는다면, 객체를 관리함으로써 해당 객체를 싱글턴처럼 쓸 수도 있다. 


구현체가 뭔지 안다.
	⁃	PostListDAO()

PostDAO 
this.postDAO = factory.getPostDAO(“map, hash, list 등”);
factory 의존성은 있긴한데 그 안에서 뭐하는지 모름 

이런 것들에 대한 책임을 모르게 하고 싶다. 



factory.getPostRepository 



Factory 클래스 생성 (전역으로 접근하기위함 )
	⁃	서로 다른 클래스 이지만 팩토리를 사용하여 전역객체로 돌려쓰게 한다 ? 
	⁃	같은 팩토리에서 얻으니까 객체 공유 가능 

싱글톤 패턴을 사용하면 instanc of 메서드 사용

postRepository 에서 싱글톤인지는 정해져 있지 않다. 

다만 팩토리를 쓸때 싱글톤처럼 되는 것이다. 

!! 서로 모르게 분리한다. 

팩토리안에 postRepository를 postDao로도 가능하다. (hash, map, list , … ) 이렇게 번갈아가며 사용가능 
이런거를 플러그라고한다. 



이렇게 인터페이스를 만든다.
도메인 옵젝 : 서비스에 해당 
	⁃	서비스 객체가 다오 인터페이스를 보는데 그거를 구현한 무언가 ? 
	⁃	그 무언가를 도메인 옵젝은 모른다는 것이다. 



DI를 얘끼하기 전
IoC에 대해 알아보자 
inversion of control (제어 역전)
	⁃	원래는 순서대로 진행이 되고 제어권을 개발자가 갖고있어야함.

	•	원래 


UI를 만들떈 대게 그렇게 안함. 
입력을하고 포커스가 아웃이 될수 있다. 이름에 대한 처리
포커스 아웃에 대한 처리
내 프로그램에선 처리를 못함
tk가 해줌
얘가 루프돌림 제어권이 tk로 넘어감


제어의 역전은 프레임워크의 정의하는 특징 


헐리우드 원칙
배우가 나 영화 나갈래 이게아님 
근본적으론 영화사에서 나를 부름


입력 처리할거야 이것이 아님 입력했나안했나 가 아니라 
입력했어 나가면 이렇게 처리해를 tk에게 위임한것. 

IoC와 DI
IoC는 프레임워크의 공통적인 특징 
<J2EE development without EJB>
POJO를 사용해야한다. 


Spring 등이 EJB를 비판하면서 POJO, IoC Container를 이야기 했고 이전쟁을 끝내기 위해 마틴파울러가 새로운 용어를 제안함 
	⁃	나는 UI에서 했던건데 오해가 크네 ….
	⁃	스프링이라던가 여러가지가 IoC를 사용하고 있어서 스페셜해 -> 내차는 특별하다 바퀴가 있기 때문에
	⁃	프레임웤이면 당연히 IoC해야하는거다 ..
	⁃	그래서 DI가 나옴

실제로는 IoC랑 DI가 거의 차이 없이 사용되긴 함
URI랑 URL이랑 비슷함.
	다만 URN을 많이 사용하지 않으니 
깐깐한 사람들은 그렇게 사용하면 안됨, 심지어 공식문서도 틀렸다는 사람이 있음 .

대부분은 동의어처럼 사용함. 



예전에는 스프링 개발자들이 Property 주입(Setter Injection)을 많이 썼지만 (Java Bean의 흔적)
최근에는 생성자 주입을 선호, 특히 final 필드로 만들어서 사용하는 걸 강력히 권장함. 



스프링은 BeanFactory 라는것을 사용
팩토리는 만드는것
스프링이 관리하는 객체를 Bean 이라 함. 객체를 관리함
	⁃	대게는 스프링 빈 (자바빈을 구분하기 위함)

스프링 이니셜라이저로 만들었다면 기본으로 TEst가 그래들빌드에 있음 

실수 사례 
테스트는 랜덤으로 진행됨. 



독립적으로 잡기위해 


각각 독립적으로 격리



스프링의 빈 팩토리를 텟트할 것. 



컨트롤러 테스트
기존 postcontroller는 PostService에 의존하고 있었다. (new를 통해)

이것을 생성자 주입을 통해서 구성한다.

무엇인지 모르겠지만 얘네둘 너네가 넣어줘라


new 고 뭐고 아무것도 없다. 
얘네 의존관계가 있는데 너네가 주입을 해줘

이 방법이 스프링에선 지원이 되도록 구성을 해놓음 




@Test
@DisplayName("Spring IoC Container를 통해 PostController 객체 얻기 테스트")
void getPostController() {
    DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();

    GenericBeanDefinition beanDefinition = new GenericBeanDefinition();

    beanDefinition.setBeanClass(PostController.class);

    beanFactory.registerBeanDefinition("postController", beanDefinition);

    PostController postController = beanFactory.getBean("postController", PostController.class); // 이름으로 얻기
}

위 코드로 테스트를 진행하면 

Error creating bean with name 'postController': Failed to instantiate [com.study.api.rest.demo.controllers.PostController]: No default constructor found
라는 에러가 나타난다. 
	⁃	이것을 만들 수 없다. 왜  ? 기본 생성자가 없기 때문에 

이전 컨트롤러에서 생성자 주입을 통했기때문에 
다른 접근 방법이 필요함 


	•	코드추가
@Test
@DisplayName("Spring IoC Container를 통해 PostController 객체 얻기 테스트")
void getPostController() {
    DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();

    GenericBeanDefinition beanDefinition = new GenericBeanDefinition();

    beanDefinition.setBeanClass(PostController.class);

    ConstructorArgumentValues constructorArgs = new ConstructorArgumentValues();
    constructorArgs.addIndexedArgumentValue(0, "getPostDtoListService");
    
    beanDefinition.setConstructorArgumentValues(constructorArgs);
    beanFactory.registerBeanDefinition("postController", beanDefinition);

    PostController postController = beanFactory.getBean("postController", PostController.class); // 이름으로 얻기
}

	•	테스트 실행
	•	에러
Error creating bean with name 'postController': Unsatisfied dependency expressed through constructor parameter 0: Could not convert argument value of type [java.lang.String] to required type [com.study.api.rest.demo.services.GetPostDtoListService]: Failed to convert value of type 'java.lang.String' to required type 'com.study.api.rest.demo.services.GetPostDtoListService'; Cannot convert value of type 'java.lang.String' to required type 'com.study.api.rest.demo.services.GetPostDtoListService': no matching editors or conversion strategy found
	⁃	0번 봤는데 타입이 이상하다. (원래 레퍼런스를 잡아줘야함.)
	⁃	생성자의 타입 밸류가 문자열이기 때문 ( constructorArgs.addIndexedArgumentValue(0, "getPostDtoListService"); )
	⁃	실제 PostController의 생성자 첫번째 매개변수 타입은 GetPostDtoListService이다.


	•	코드 추가
@Test
@DisplayName("Spring IoC Container를 통해 PostController 객체 얻기 테스트")
void getPostController() {
    GetPostDtoListService getPostDtoListService =
            new GetPostDtoListService();

    DefaultListableBeanFactory beanFactory =
            new DefaultListableBeanFactory();

    GenericBeanDefinition beanDefinition =
            new GenericBeanDefinition();

    beanDefinition.setBeanClass(PostController.class);

    ConstructorArgumentValues constructorArgs =
            new ConstructorArgumentValues();
    constructorArgs.addIndexedArgumentValue(0, getPostDtoListService);

    beanDefinition.setConstructorArgumentValues(constructorArgs);
    beanFactory.registerBeanDefinition("postController", beanDefinition);

    PostController postController = beanFactory.getBean("postController", PostController.class); // 이름으로 얻기
}
	⁃	얘가 알아서 조립을 해줌 
	⁃	

예전에는 XML로 함.

스프링 앱의 대부분은 최소한의 IoC 컨테이너인 BeanFactory를 넘어서 ApplicationContext를 사용할 때가 많음. 
	⁃	BeanFactory -> ApplicationContext 확장된 인터페이스 
예전에는 Bean에 대한 정보를 xml 파일로 썼는데 최근 Java의 Annotation으로 처리함.
전자를 진정한 POJO라고 말하는 사람도 있지만 후자가 너무 편함. 
여러 구현 중 선택하거나 값을 주입하는 건 외부(XML, 환경변수 등)를 활용하는게 좋다.

Bean은 Java Config에서 @Bean 어노테이션을 써서 정의하거나, 해당 클래스에 @Compoenet 어노테이션을 붙여주고 Scan함 (@ComponentScan 사용)



PostController의 RestController 어노테이션을 확인해보면 이미 @Component가 있다. 이미 얘는 스프링 빈이라는 것

Service에도 Compenet 어노테이션을 사용하는데 Service라는 것으로 대체한다. 



만약 PostController의 스프링 생성자 주입 방식을 사용하고 Service의 Service 어노테이션을 생략한다면 아래와 같은 에러가 난다. 


Action:

Consider defining a bean of type 'com.study.api.rest.demo.services.GetPostDtoListService' in your configuration.
	⁃	컨트롤러에서 주입한 스프링 빈을 못찾는다. 


@Bean에서 얻어옴 싱글톤처럼 사용 (스프링 자체가 그렇게 관리해줌) 
	⁃	객체를 여러개 생성하지 않는다는 것. 


기본적으로 빈 팩토리가 만들어주고 관리도함
그 이상의것을 하기위해 애플리케이션 컨텍스트라는것을 잡아서 해줌


자바 컨피그 방식도 존재
@Configuration 어노테이션 


내부에서 방금 진행했던 행동을 함. 자세히는 더 복잡함 
	⁃	느낌이 이정도 (이런것들이 돌아간다는 정도로 이해하기)

중복으로 막 만들지 않음



?? 
왜 단순히 new로 생성하는건 의존한다고 하는것일까 ?
DI를 하면 의존한다고 안보는건가 ? 
의존의 의미가 무엇일까 

1차적인 내생각은 new로 생성하나 생성자 주입으로 하나 그 객체를 사용하는건 같은데  정확히 객체 하나를 재사용하기 위해서만 일것같진 않고.. 


도구를 위한 도구
스프링코어 


레이어 나누는게 힘들면안됨


## 학습 키워드
• Spring AOP(Aspect Oriented Programming)
• Dependency Injection
• 싱글턴 패턴
• 싱글턴이 생겨난 이유, 어떻게 발견 ?
• IoC(Inversion of Control)
• Spring Bean
• BeanFactory
