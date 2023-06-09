# Repository


### 전술적 설계의 Repository
- 도메인 개체에 액세스하고 유지하기 위한 추상화를 제공하기 위해 도메인 계층의 전술적 디자인에 사용되는 패턴이다.
- 도메인 계층과 데이터 액세스 계층 간의 중재자 역할을 하여 기본 데이터 스토리지 메커니즘의 세부 정보를 노출하지 않고도 도메인 개체를 검색, 저장 및 쿼리할 수 있다.
- Aggregate를 관리하는 Collection처럼 작동한다. 이는 두 가지 의미를 갖는다.
1. 오직 Aggregate만 Repository를 갖는다.
2. Repository는 영속화 방법 및 기술을 감춘다.

#### 특징
- 데이터 액세스 논리를 캡슐화하고 도메인 개체 작업을 위한 깨끗하고 도메인 중심적인 인터페이스를 제공하는 것.
- 도메인 계층은 데이터 스토리지 기술의 특정 구현 세부 사항에 밀접하게 연결되지 않고 도메인 개체에서만 동작할 수 있다.

#### 유의할 점
- 데이터베이스 또는 ORM(Object-Relational Mapping) 프레임워크의 선택과 같은 데이터 액세스 계층의 특정 구현 세부 정보를 지시하지 않는다는 점에 유의하자.
  - Spring Data JPA는 이 둘을 만족시키기 위한 기능을 갖추고 있다. 
- 리포지토리 인터페이스는 도메인 개체에 액세스하고 유지하기 위한 계약을 정의한다. 
  - Persistence Context를 통해 Collection처럼 사용하는 게 가능하며, 아예 인터페이스만 만들면 나머지는 크게 신경 쓰지 않아도 되는 기능까지 제공함.
- 하지만 실제 구현은 애플리케이션의 특정 요구 사항 및 기술에 따라 다를 수 있다.

> 리포지토리 패턴을 사용하여 도메인 계층은 비즈니스 개념 및 동작 모델링에 집중할 수 있는 반면, 리포지토리는 데이터 액세스 및 지속성의 복잡성을 처리하여 데이터 저장소 계층과 상호 작용하기 위한 깨끗하고 도메인 중심적인 인터페이스를 제공한다.



`그렇다면 Aggregate에서 각 객체에 접근할 때 여러 Repository를 받아와야 할 것 같은데 Spring을 사용하면 간단히 DI를 통해 접근 가능하게 된다. `
   
> 이말인 즉슨 static으로 만들거나 직접 객체를 생성해서 가져올 필요가 없다는 뜻
   

#### 마무리 요약
- 우리는 적절한 Aggregate를 발견 및 생성한다
- 그리고 적절히 책임을 나눌 수 있도록 Entity와 Value Object로 구성(또는 분해)한다.
- 이를 위한 Repository를 만들과 동시에, 여러 기술 문제와 무관한 **비즈니스 도메인에 집중**할 수 있게 된다. 

> Aggregate -> Aggregate Entity, Aggregate VO -> Aggregate Repository

- 이렇게 비즈니스 도메인에 집중한 코드를 모아둔 곳을 Domain Layer라고 부른다. (도메인 계층)


- Repository와 Aggregate를 사용하는 코드가 모인 곳을 Application Layer(여기선 애플리케이션의 기능, Use Case가 직접적으로 드러나게 된다) : 서비스(비즈니스) 계층 ?
> 여기선 도메인 계층에 어떤 역할을 위임할 것인지 데이터를 받아 각각 정해주는 통로 역할을 한다고 생각하면 될 것 같다.

- Web 등 구체적인 기술로 사용자와 소통하는 코드가 모인 곳을 UI Layer라고 부른다. (컨트롤러)
- 이게 바로 Layered Architecture가 DDD와 함께 다뤄지는 이유다. 


> Layered Architecture의 도움 없이는 도메인 모델을 Domain Layer에 격리하는 게 불가능하다.


## 느낀점
- 어렵다 너무 어렵다.
- 단순히 3계층 아키텍처로 한다면 어떻게든 만들겠지만 도메인을 정의하고 이것을 나누는 과정에서 막힐거 같고 무엇보다 용어와 뜻, 어떻게 녹여내는지가 감이 안온다.
- 물론 처음에는 당연 막막할거라는걸 인지하지만 그렇게 되기 싫은 욕심도 난다.
- Aggregate Root를 정하고 이를 적절히 나누면서 외부에서 어떻게 접근하게 할것인지 잘 정해봐야겠다.
- 또... 테스트코드는 어떻게 작성하지 ..?



## 학습 키워드
- 레포지토리 (Repository)
- Application Layer와 UI Layer