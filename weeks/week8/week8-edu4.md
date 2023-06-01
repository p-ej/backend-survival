# Aggregate

### Aggregate 집합체
- 전략적 설계에서 우리가 실제로 집중하게 될 부분이다.
- Entity와 VO는 도메인 모델, 도메인 객체가 맞지만 이를 바로 사용하진 않는다.
- 자동차라는 집합체를 사용하고 타이어나 엔진을 사용하지 않는다는 것과 같다.
- 불변식을 유지하고 여러 도메인 객체를 사용하기 좋게 만들어준다.
- 한 도메인에 대한 여러 객체가 있다고 가정했을때 여러 방면에서 각 객체에 접근함으로써 불변식(무결성)을 깨뜨리고 일관성을 파괴하는 일이 없도록 지켜준다. 
  - 그래서 우리가 다루는 도메인 객체의 개수를 적절하게 조절해주어야 한다.
  - `즉, 자동차의 부품을 모두 직접 접근하는 것과 자동차라는 자체의 단위로 접근하는 것은 다르다`

> 불변식이란것은 우리가 지켜야 할 제약 조건, 규칙이라고 볼 수 있다.

#### 특징
- 도메인 개체를 응집력 있는 단위로 구조화하고 관리하는 데 사용되는 패턴
- 하나 이상의 엔터티 및 값 개체로 구성되며 엔터티 중 하나는 집계 루트 역할을 한다.
- 일관성의 단일 단위로 취급되는 도메인 개체의 모음
- 트랜잭션 일관성 경계를 정의하고 소프트웨어 애플리케이션의 도메인 계층 내에서 비즈니스 규칙을 시행하기 위한 중요한 개념
- Aggregate Root는 Aggregate에 대한 기본 액세스 지점 역할을 하며 Aggregate 내 개체의 일관성과 무결성을 유지 관리하는 역할을 한다.

> 아래의 예시를 보자
- Order(주문) Entity와 Line Item Entity로 주문을 표현한다.
- 하나의 주문(Order)은 여러 Line Item을 포함한다. 
- 총 주문액이 100만원을 넘지않는 불변식을 적용한다.

```
Order
- Identifier: order-123
- List<LineItem>
    - LineItem
        - Identifier: item-234
        - Product: Guitar
        - Quantity: 1
        - Unit Price: 30만 원
        - Total Price: 30만 원
    - LineItem
        - Identifier: item-345
        - Product: Microphone
        - Quantity: 1
        - Unit Price: 20만 원
        - Total Price: 20만 원

// 총 주문액은 50만원
```

- 만약 개별 Line Item에 직접적으로 접근이 다능하고 시스템이 동시에 여러 처리를 할 수 있다면 2개의 요청이 들어올 경우 다음과 같이 될 수 있다.

> 50만원이 남은것을 확인 후 기타 한대와 마이크 2개를 더 구입한다. 이를 동시 처리한다고 가정

```
Order
- Identifier: order-123
- List<LineItem>
    - LineItem
        - Identifier: item-234
        - Product: Guitar
        - Quantity: 2
        - Unit Price: 30만 원
        - Total Price: 60만 원
    - LineItem
        - Identifier: item-345
        - Product: Microphone
        - Quantity: 3
        - Unit Price: 20만 원
        - Total Price: 60만 원
// 총 주문액 120만원
```

- 이런 경우 총 금액 100만원의 불변식이 깨져버린다.
- 해결법은 여기서 Order Entity를 Order라는 Aggregate의 Root로 삼는다.
- 또 하나의 객체에 동시 접근할 수 없게하는 제약을 통해(일반적으로 트랜잭션이 있다.) 불변식을 지킬 수 있게한다.
- 추가로 불변식뿐만 아니라 Aggregate가 제공하고는 적절한 인터페이스를 통해 더 나은 표현력을 가질 수 있게 된다.
   
   
> 아래 코드에서 여러 3가지 가정을 살펴보자.
   
- Line Item에 개별접근한 경우
```java
    LineItem lineItem = lineItemRepository.findById("item-234").get();
    lineItem.changeQuantity(2);

    LineItem lineItem = lineItemRepository.findById("item-345").get();
    lineItem.changeQuantity(3);

// 이 경우는 자동차로 접근하는것이 아닌 부품에 접근하는것과 같음
// 직접 아이템의 수량을 조작한다.
```
   
- Aggregate를 사용하는 경우
```java
    Order order = orderRepository.findById("order-123").get();
    order.changeItemQuantity("item-234", 2);
    order.changeItemQuantity("item-345", 3);

// 이 경우는 숫자를 직접 바꿔주는 것
```
   
- Aggregate의 동작에 집중하는 경우
```java
    Order order = orderRepository.findById("order-123").get();
    order.addItem("Guitar", 1);
    order.addItem("Microphone", 2);

// 이 경우 Order Entity의 함수를 통해 구매할 수량만 넘겨줌으로써 수량을 늘려달라는 작업까지 위임한다.
```

> - 마지막 코드는 Aggregate를 통해 OOP에서 말하는 협력(위임의 관계라고해야하나)하는 객체를 더 잘 구성할 수 있게 된다.
> - 책임, 위임, 관심사의 분리 등의 주제를 이를 통해 고민하고 적용할 수 있다.


#### 4가지 경험 법칙
- IDDD의 저자인 Vaughn Vernon은 4가지 경험 법칙이다. 잘 숙지하도록 하자
1. 불변식을 통해 일관성 경계를 찾아서 모델링한다. Aggregate은 트랜잭션적 일관성 경계와 동의어다.
2. 작은 Aggregate으로 설계한다.
3. ID로 다른 Aggregate을 참조한다. JPA 등을 사용하면 관련된 객체를 모두 직접 참조할 수 있게 되는데, 그렇게 하지 않는다.
4. 경계 밖에선 결과적 일관성을 사용한다. 이를 위해 도메인 이벤트 등을 사용할 수 있다.
  - `만약 다른 사용자가 시스템이 해야 할 일이라면 결과적 일관성을 선택하자`
  - 나 여기 담았어 같은 도메인 이벤트를 날린다.
  - 결과적 일관성은 1초 지나고나서 아까담은거 반영한다.

> - 4번에 대해서는 무슨 뜻인지 잘 몰라서 간략하게 찾아봤는데 MSA 패턴에서 적용할만한 사항이라는 것 같다. 우선 거대해지는 서비스 그리고 분산환경에 대해 피할 수 없으니 자연스레 DBMS에 대한 트랜잭션만으로 데이터의 일관성을 유지하는 것은 한계가 있다는 것 같다. 

https://www.popit.kr/%EA%B2%B0%EA%B3%BC%EC%A0%81-%EC%9D%BC%EA%B4%80%EC%84%B1%EC%9D%B8%EA%B0%80-%EC%B5%9C%EC%A2%85%EC%A0%81-%EC%9D%BC%EA%B4%80%EC%84%B1%EC%9D%B8%EA%B0%80/




#### 주요 원칙들
1. 경계
  - 관련 도메인 개체의 클러스터 주위에 경계를 정의한다. 
  - Aggregate 내의 Entity 및 VO를 캡슐화하고 Aggregate에 대한 모든 수정 사항이 Aggregate Root를 통과하도록 한다. (위에서 본 Order)

2. 일관성
  - Aggregate는 경계 내에서 불변성과 비즈니스 규칙을 적용하여 일관성을 보장한다. 
  - 개체 생성, 업데이트 또는 삭제와 같이 Aggregate 상태에 영향을 미치는 모든 작업은 Aggregate Root를 통해 수행된다. 
  - 이렇게 하면 Aggregate가 변경 사항을 제어하고 유효성을 검사하여 Aggregate가 유효하고 일관된 상태로 유지되도록 할 수 있다. 
  - 트랜잭션 일관성, 결과적 일관성

3. 트랜잭션 경계
  - Aggregate는 트랜잭션 경계를 정의한다. 
  - 즉, Aggregate 내의 변경 사항은 일반적으로 **단일 작업 단위**로 함께 유지되거나 롤백된다. 
  - 이는 Aggregate 내에서 데이터 무결성과 일관성을 유지하는 데 도움이 되고 데이터베이스 트랜잭션 관리를 단순화한다.

4. Aggregate Root
  - 각 Aggregate에는 지정된 Aggregate Root가 있으며 이는 Aggregate 내의 개체에 액세스하고 수정하기 위한 진입점 역할을 하는 Aggregate 내의 엔터티이다. 
  - Aggregate Root는 Aggregate 불변성을 적용하고 구성 객체 간의 상호 작용을 조정하는 역할을 한다.

5. 관계 및 탐색
  - Aggregate 간의 관계는 일반적으로 Aggregate Roots에 대한 참조로 표현된다. 
  - 즉, 외부 세계는 Aggregate Roots를 통해서만 다른 Aggregate에 액세스하고 상호 작용할 수 있으므로 캡슐화를 유지하고 각 Aggregate의 일관성을 보호할 수 있다.

6. 수명 주기 관리
  - 종종 잘 정의된 수명 주기와 생성 및 삭제 규칙이 있다. 
  - Aggregate 내에서 새 개체의 생성을 제어하고 사용하기 전에 일관된 상태인지 확인한다.

> - 관련 개체를 함께 그룹화하고 명확한 경계와 일관성 규칙을 정의하여 복잡한 도메인 모델을 구성하고 구조화하는 데 도움이 된다. 
> - 모듈화, 캡슐화를 가능하게 하고 도메인 개체가 애플리케이션의 도메인 계층 내에서 무결성을 유지하고 비즈니스 규칙을 시행하면서 효과적으로 협업하도록 돕는다.


## 학습 키워드
- 집합체 (Aggregate)
- 불변식
- 트랜잭션적 일관성과 결과적 일관성