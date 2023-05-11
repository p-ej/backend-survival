# Dependency Injection


스프링의 활용법

스프링의 주요
DI
	⁃	맞닿아 있는 부분 많음 
AOF 
	⁃	거의 직접 구현하진 않음


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


학습 키워드
	•	Spring AOP(Aspect Oriented Programming)
	•	Dependency Injection
	•	싱글턴 패턴
	•	싱글턴이 생겨난 이유, 어떻게 발견 ?
	•	IoC(Inversion of Control)
	•	Spring Bean
	•	BeanFactory
