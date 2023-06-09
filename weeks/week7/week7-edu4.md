# Relationship Mapping

### Relationship Mapping이란 
- Hibernate 또는 JPA(Java Persistence API)와 같은 ORM(Object-Relational Mapping) 프레임워크의 맥락에서 관계 매핑은 관계형 데이터베이스의 엔터티 간 관계를 정의하고 애플리케이션의 해당 객체 지향 관계에 매핑하는 프로세스를 말한다
- 도메인 모델.
- 관계형 데이터베이스에서 엔터티는 일반적으로 여러 테이블에 저장되며 엔터티 간의 관계는 외래 키 제약 조건을 사용하여 표현된다. 
- ORM 프레임워크는 관계 유형, 카디널리티 및 관계가 데이터베이스 수준에서 구현되는 방식을 지정하는 주석 또는 구성 옵션을 제공하여 이러한 관계의 매핑을 처리한다.


#### 특징
1. 일대일(1:1)
  - 일대일 관계에서 한쪽의 각 엔터티 인스턴스는 다른 쪽의 정확히 하나의 엔터티 인스턴스와 연결된다. 
  - 이 관계는 각 엔터티가 다른 엔터티에 대한 참조를 갖는 양방향이거나 하나의 엔터티만 다른 엔터티에 대한 참조를 갖는 단방향일 수 있다.

2. 일대다(1:N)
  - 일대다 관계에서 한쪽의 각 엔터티 인스턴스는 다른 쪽의 여러 엔터티 인스턴스와 연결된다. 
  - 예를 들어 고객 엔터티에는 연결된 여러 주문 엔터티가 있을 수 있다. 
  - 이 경우 주문 엔터티에는 고객 엔터티를 참조하는 외래 키가 있다.

3. 다대일(N:1)
  - 다대일 관계에서 한 쪽의 여러 엔터티 인스턴스는 다른 쪽의 단일 엔터티 인스턴스와 연결된다. 
  - 이것은 일대다 관계의 역관계다. 
  - 예를 들어 여러 주문 엔터티를 단일 고객 엔터티와 연결할 수 있다.

4. 다대다(N:N)
  - 다대다 관계에서 한 쪽의 여러 엔터티 인스턴스는 다른 쪽의 여러 엔터티 인스턴스와 연결될 수 있다. 
  - 이러한 유형의 관계에는 일반적으로 연결을 나타내는 중간 조인 테이블이 필요하다. 
  - 예를 들어 직원 엔터티에는 여러 프로젝트가 연결될 수 있으며 각 프로젝트에는 여러 직원이 할당될 수 있다.
    - 좋아요 같은경우도 

> 실무에서는 다대다를 표현하기 어렵다. 그래서 중간에 매핑? 관계 테이블을 두는것으로 배웠다. ORM으로는 중간에 관계 테이블을 하나 두고 연결할 테이블에 관계 테이블로 OneToMany를 걸었던 경험이 있다. ManyToMany도 가능하다

#### 어노테이션 구성
- `@OneToOne`: 엔터티 간의 일대일 관계
- `@OneToMany`: 하나의 엔터티에서 여러 엔터티로의 일대다 관계
- `@ManyToOne`: 여러 엔터티에서 하나의 엔터티로의 다대일 관계
- `@ManyToMany`: 엔터티 간의 다대다 관계


#### 주석을 사용한 매핑 예
```java
@Entity
public class Author {
    @Id
    private Long id;

    private String name;

    @OneToMany(mappedBy = "author")
    private List<Book> books;

    // Getters, setters, constructors, etc.
}

@Entity
public class Book {
    @Id
    private Long id;

    private String title;

    @ManyToOne
    private Author author;

    // Getters, setters, constructors, etc.
}
```
- `저자`와 `책` 사이에 일대다 관계다. 
- `저자` 엔터티에는 `books` 필드로 표시되는 `도서` 엔터티 모음이 있고 `책` 엔터티는 `저자` 필드로 표시되는 `저자` 엔터티와 다대일 관계가 있다.

> 엔터티 간의 관계를 매핑함으로써 ORM 프레임워크는 이러한 관계를 투명하게 로드, 유지 및 탐색하는 복잡성을 처리하여 개발자가 관계형 데이터베이스 기능을 활용하면서 객체 지향 관계로 작업할 수 있도록 한다.


### N + 1 문제
- N+1 문제는 Hibernate 또는 JPA(Java Persistence API)와 같은 ORM(Object-Relational Mapping) 프레임워크를 사용하여 데이터를 가져올 때 발생할 수 있는 성능 문제이다. 
- 지연 로딩 방식으로 관련 엔터티 또는 컬렉션을 검색할 때 실행되는 과도한 데이터베이스 쿼리 수를 나타낸다.

#### 왜 일어날까 ?
- N+1 문제는 일반적으로 관련된 엔터티 모음("N" 쪽이라고 함)이 있는 엔터티("일" 쪽이라고 함)가 있는 시나리오에서 발생한다. 
- 지연 로딩은 이러한 관련 엔터티를 액세스할 때만 주문형으로 로드하는 데 자주 사용된다.

#### 무슨문제 ?
- 이 문제는 "하나" 엔터티를 가져온 다음 관련 엔터티 컬렉션을 반복할 때 발생한다. 
- 각 관련 엔터티에 대해 지연 로드가 활성화된 경우 데이터베이스에서 해당 엔터티를 가져오기 위해 별도의 쿼리가 실행된다. 
- 그 결과 N+1 쿼리가 실행된다. 여기서 N은 관련 엔터티의 수이다.

#### 예를 들어보자
- 각 블로그 게시물에 여러 개의 댓글이 있을 수 있는 블로그를 모델링하는 애플리케이션을 생각해 보자. 
- 지연 로드가 활성화되어 있고 블로그 게시물 목록을 검색한 다음 각 게시물을 반복하여 댓글에 액세스하면 댓글을 가져오기 위해 각 게시물에 대해 추가 쿼리가 실행된다. 
- 이것은 N+1 쿼리로 이어지며 N은 블로그 게시물의 수이다.

> N+1 문제는 특히 대규모 데이터 세트 또는 깊게 중첩된 관계를 처리할 때 성능에 상당한 영향을 미칠 수 있다. 이로 인해 과도한 데이터베이스 왕복절차, 네트워크 트래픽 증가 및 애플리케이션 응답 시간 저하가 발생할 수 있다.

   
   
#### 해결하려면 어떻게 해야할까
1. 즉시 가져오기
  - 지연 로딩 대신 즉시 가져오기를 사용하여 단일 쿼리에서 기본 엔터티와 함께 관련 엔터티 또는 컬렉션을 가져올 수 있다. 
  - 이는 엔터티 매핑에서 적절한 가져오기 유형을 지정하여 달성할 수 있다.

2. 일괄 가져오기
  - 일괄 가져오기는 IN 절 또는 조인 가져오기를 사용하여 단일 쿼리에서 여러 관련 엔터티를 가져오는 방법이다. 
  - 이렇게 하면 실행되는 쿼리 수가 줄어들고 성능이 향상된다.

3. 조인 가져오기
  - 조인 가져오기를 사용하면 쿼리에서 명시적 조인을 지정하여 단일 쿼리에서 관련 엔터티를 검색할 수 있다. 
  - 이렇게 하면 데이터베이스에 대한 단일 왕복으로 필요한 모든 데이터를 가져온다.

4. Fetch Graphs 또는 Entity Graphs
  - JPA에서 제공하는 이러한 기능을 사용하면 엔티티 및 해당 연결에 대해 미리 정의된 가져오기 계획을 정의할 수 있다. 
  - 가져오기 계획을 미리 지정하면 단일 쿼리에서 필요한 모든 데이터를 가져와서 N+1 문제를 피할 수 있다.

5. DTO 프로젝션
  - DTO(Data Transfer Object) 프로젝션을 사용하면 전체 엔터티를 로드하는 대신 데이터베이스에서 필요한 데이터만 선택적으로 가져올 수 있다. 
  - 이렇게 하면 쿼리 수를 줄이고 성능을 향상시킬 수 있다.

> 애플리케이션의 데이터 액세스 패턴을 신중하게 분석하고, 성능과 메모리 사용량 사이의 균형을 고려하고, N+1 문제를 피하기 위해 적절한 가져오기 전략을 선택하는 것이 중요하다.


### DDD의 Aggregate
- DDD(Domain-Driven Design)에서 집계는 관련 개체의 클러스터를 일관성 및 트랜잭션 경계의 단일 단위로 나타내는 패턴이다. 
- 응용 프로그램 내에서 도메인 개체를 모델링하고 구성하는 데 중요한 개념이다.
- 집계는 하나 이상의 도메인 개체로 구성되며 그 중 하나는 집계 루트로 지정된다. 
- 집계 루트는 집계에 대한 진입점 역할을 하며 전체 집계의 일관성과 무결성을 보장하는 역할을 한다. 
- 개체 검색 및 수정을 포함하여 집계와의 모든 상호 작용은 집계 루트를 거쳐야 한다.

#### 특징
1. 일관성 경계
  - 집계는 모든 개체가 서로 일관성이 있는 경계를 정의한다. 
  - 집계 루트는 일관성 규칙을 적용하고 집계 상태의 무결성을 유지하는 역할을 한다. 
  - 집계 내의 개체에 대한 변경 사항은 집계 루트를 통해 조정되어 집계가 유효하고 일관된 상태로 유지되도록 한다.

2. 트랜잭션 경계
  - 집합체는 트랜잭션 경계 역할도 한다. 
  - 즉, 집합체 내의 개체에 대한 변경 사항은 일반적으로 단일 작업 단위로 유지되거나 롤백한다. 
  - 이렇게 하면 집계 컨텍스트 내에서 이루어진 변경 사항이 원자적이며 다른 집계와 격리된다.

3. 캡슐화
  - Aggregate는 복잡한 비즈니스 개념 작업을 위한 상위 수준의 추상화를 제공하여 도메인 개체와 해당 동작을 캡슐화한다. 
  - 객체의 내부 세부 정보를 숨기고 집계 루트를 통해 필요한 작업만 노출하여 집계 상태의 무결성을 보호한다.

4. 집계 루트
  - 집계 루트는 집계에 대한 진입점이며 집계의 개체와 상호 작용하기 위한 유일한 액세스 포인트 역할을 한다. 
  - 이는 전체 집계를 나타내며 일관성을 강화하고 집계 내에서 작업을 조정하는 일을 담당한다.

5. 관계 관리
  - 집계는 집계 내 개체 간의 관계 및 연결을 정의한다. 
  - 집계 내의 개체는 서로에 대한 직접 참조를 가지며 일반적으로 집계 외부의 개체에 대한 직접 참조는 허용되지 않는다.

6. 확장성 및 성능
  - 집계는 트랜잭션 경계의 세분성을 제어하여 성능 및 확장성을 최적화하도록 설계할 수 있다. 
  - 관련 개체를 집계로 그룹화하면 동시 액세스의 영향을 최소화하고 데이터베이스 업데이트 빈도를 줄일 수 있다.

`마무리`
- 집계를 설계할 때 도메인 내의 경계와 관계를 고려하여 집계가 최대한 응집력 있고 일관되며 독립적이 되도록 하는 것이 중요하다. 
- 집계는 순수하게 데이터 모델링 문제에 의해 구동되는 것이 아니라 도메인 개념 및 비즈니스 규칙을 기반으로 해야 한다.


> - 요약하면 DDD의 집계는 복잡한 비즈니스 개념을 명확한 경계와 트랜잭션 일관성이 있는 응집력 있는 단위로 모델링하고 캡슐화하는 방법을 제공함 
> - 도메인 모델의 무결성과 일관성을 유지하는 데 중요한 역할을 하며 대규모 애플리케이션의 복잡성을 구성하고 관리하는 데 도움을 줌

   
    
### CascadeType.ALL
- 엔터티 간의 특정 관계에 대한 계단식 동작을 지정하는 데 사용할 수 있는 옵션이다.
- cascade 작업은 한 엔터티에서 연결된 엔터티로 특정 작업을 전파하는 것을 말하낟. 
- 계단식 동작이 정의되면 한 엔터티에서 작업을 수행하면 연결된 엔터티에서 동일한 작업이 자동으로 트리거되어 데이터 일관성을 보장하고 개체 그래프의 무결성을 유지한다.

#### 특징
- `CascadeType.ALL` 옵션은 `PERSIST`, `MERGE`, `REMOVE` 및 `REFRESH`와 같은 모든 계단식 작업을 포함하는 편의 옵션이다.
- 관계에 대해 `CascadeType.ALL`이 지정되면 상위 엔터티에서 수행되는 모든 작업이 연결된 엔터티로 cascade된다.
   
1. `PERSIST`: 엔터티가 지속되면(`EntityManager.persist()`) `PERSIST` 캐스케이드 작업은 연결된 엔터티도 지속되도록 한다.

2. `MERGE`: 엔티티가 병합되면(`EntityManager.merge()`) `MERGE` 캐스케이드 작업을 통해 상위 엔티티에 대한 변경 사항이 연결된 엔티티로 전파된다.

3. `REMOVE`: 엔티티가 제거되면(`EntityManager.remove()`) `REMOVE` 캐스케이드 작업은 연관된 엔티티도 제거되도록 한다.

4. `REFRESH`: 엔티티가 새로 고쳐질 때(`EntityManager.refresh()`) `REFRESH` 캐스케이드 작업은 연관된 엔티티도 새로 고쳐져 데이터베이스의 최신 상태를 반영하도록 한다.

> - 'CascadeType.ALL'을 사용하여 지정된 계단식 동작이 지정된 모든 계단식 작업에 적용된다는 점에 유의하는 것이 중요하다. 
> - 특정 캐스케이드 작업만 활성화하려는 경우 'CascadeType.ALL'을 사용하는 대신 개별적으로 지정하도록 선택할 수 있다.

#### 사용예시
```java
@Entity
public class Author {
    @Id
    @GeneratedValue
    private Long id;

    private String name;

    @OneToMany(mappedBy = "author", cascade = CascadeType.ALL)
    private List<Book> books;

    // Getters, setters, constructors, etc.
}

@Entity
public class Book {
    @Id
    @GeneratedValue
    private Long id;

    private String title;

    @ManyToOne
    private Author author;

    // Getters, setters, constructors, etc.
}
```
- `Author` 엔터티는 `Book` 엔터티와 일대다 관계를 가지며 `Books` 컬렉션에 대해 `CascadeType.ALL`이 지정됨. 
- 즉, 지속, 병합, 제거 또는 새로 고침과 같이 `저자` 엔터티에서 수행되는 모든 작업이 연결된 `도서` 엔터티로 cascade된다.

> - 의도하지 않은 부작용이나 순환 종속성을 피하기 위해 계단식 작업을 사용할 때는 주의를 두어야한다. 
> - 계단식 작업의 동작과 의미를 이해하고 애플리케이션의 특정 요구 사항에 따라 적절하게 사용하는 것이 중요하다.
> - 남발금지

### orphanRemoval
- 관계의 "다" 쪽에 있는 엔티티가 관계의 동작을 지정하는 데 사용할 수 있는 속성이다.
- 관계가 제거되거나 "일" 쪽에 있는 엔터티에서 분리된다
- 기본적으로 엔터티가 관계에서 제거되거나 연결 해제되면 "다" 쪽의 연결된 엔터티는 상위 엔터티에 더 이상 연결되지 않더라도 데이터베이스에 남아 있다. 
- 하지만 `orphanRemoval` 특성을 사용하여 ORM 프레임워크에 고아(즉, 상위 엔터티에서 더 이상 참조하지 않는 연결된 엔터티)를 자동으로 제거하도록 지시할 수 있다.
- 'orphanRemoval'이 'true'로 설정되면 고아 제거 동작이 활성화된다. 
    - 즉, 엔터티가 관계에서 제거되거나 분리되면 고아로 간주되는 연결된 엔터티(상위 엔터티에서 더 이상 참조하지 않음)가 데이터베이스에서 자동으로 제거된다.
   
   
> 예시
```java
@Entity
public class Parent {
    @Id
    @GeneratedValue
    private Long id;

    @OneToMany(mappedBy = "parent", cascade = CascadeType.ALL, orphanRemoval = true)
    private List<Child> children;

    // Getters, setters, constructors, etc.
}

@Entity
public class Child {
    @Id
    @GeneratedValue
    private Long id;

    @ManyToOne
    private Parent parent;

    // Getters, setters, constructors, etc.
}
```
- `Parent` 엔티티는 `Child` 엔티티와 일대다 관계를 가지며 `orphanRemoval`은 `children` 컬렉션에 대해 `true`로 설정된다. 
- 즉, `Child` 엔티티가 `children` 컬렉션에서 제거되거나 상위 `Parent` 엔티티에서 분리되면 고아로 간주되어 데이터베이스에서 자동으로 제거된다.


#### 고려사항
1. `orphanRemoval`은 관계의 "일" 쪽에 있는 항목에만 적용된다. 위의 예에서는 `Parent`와의 관계에서 "다" 쪽에 있기 때문에 `Child` 엔터티에 적용됨.

2. `orphanRemoval` 속성은 일반적으로 `CascadeType.ALL` 또는 기타 캐스케이드 작업과 함께 사용되어 일관된 동작을 보장한다.

3. `orphanRemoval`을 사용할 때 관련 항목이 자동으로 제거될 수 있으므로 주의하고 고아 제거 동작이 애플리케이션의 비즈니스 규칙 및 요구 사항과 일치하는지 확인하자.

4. `orphanRemoval`을 사용할 때 특히 잠재적으로 고아가 될 수 있는 연결된 엔터티가 많은 경우 성능 영향을 고려해야하낟.

> `orphanRemoval`을 활용하면 연결된 엔터티의 관리를 간소화하고 고아가 자동으로 제거되도록 하여 데이터베이스에서 데이터 일관성을 유지할 수 있습니다.
   
   
### Event Sourcing
- 이벤트 소싱은 현재 상태 자체를 저장하는 대신 일련의 이벤트를 캡처하고 저장하여 응용 프로그램의 상태를 파생시키는 소프트웨어 개발 패턴 및 아키텍처 접근 방식이다. 
- 이벤트 소싱에서 시스템의 모든 변경 또는 작업은 변경 불가능한 이벤트로 표시되며 이벤트 로그 또는 이벤트 저장소에 저장된다.
- 객체의 현재 상태를 유지하는 대신 이벤트 소싱은 현재 상태로 이어진 일련의 이벤트를 캡처하는 데 중점을 둔다. 
- 이러한 이벤트는 발생한 순서대로 저장되며 언제든지 재생하여 시스템 상태를 재구성할 수 있다.


#### 특징
1. 이벤트
  - 이벤트는 시스템에서 발생한 불변의 사실 또는 동작을 나타낸다. 
  - 어떻게 발생했는지 또는 현재 상태가 무엇인지가 아니라 발생한 일을 캡처한다. 
  - 이벤트에는 일반적으로 이벤트 유형, 관련 엔터티, 관련 데이터 또는 페이로드와 같은 정보가 포함된다.

2. 이벤트 저장소
  - 이벤트 저장소는 시스템에서 생성된 모든 이벤트를 저장하는 지속 가능한 추가 전용 저장소 매커니즘이다. 
  - 이벤트의 시간 순서를 유지하고 이벤트를 효율적으로 검색하고 재생할 수 있다.

3. 상태 재구성
  - 이벤트 저장소에서 이벤트를 재생하여 시스템 상태를 재구성할 수 있다. 
  - 저장된 순서대로 이벤트를 적용함으로써 시스템은 주어진 시점에서 상태를 재구축할 수 있다. 
  - 이를 통해 시스템 동작에 대한 기록 보기를 제공하고 과거 상태 쿼리 기능을 사용할 수 있다.

4. 감사 추적 및 규정 준수
  - 이벤트 소싱은 본질적으로 시스템의 모든 작업 및 변경 사항에 대한 감사 추적을 제공한다. 
  - 이벤트는 변경할 수 없고 이벤트 저장소에 저장되므로 감사, 규정 준수 또는 규제 목적으로 이벤트 기록을 추적하고 분석하기가 더 쉬워진다

5. 확장성 및 성능
  - 이벤트 소싱은 이벤트 처리에서 이벤트 저장 및 쿼리를 분리하여 확장성 이점을 제공할 수 있다. 
  - 이를 통해 이벤트 처리 부하를 분산하고 이벤트를 병렬로 처리할 수 있다.

6. 이벤트 기반 아키텍처
  - 이벤트 소싱은 이벤트 기반 아키텍처 및 이벤트 기반 통신 패턴과 잘 맞는다. 
  - 시스템을 이벤트에 기반으로 함으로써 다른 시스템과 통합하고, 특정 이벤트를 기반으로 작업을 트리거하고, 느슨하게 결합된 이벤트 기반 아키텍처를 구축하는 것이 더 쉬워진다.

> - 이벤트 소싱은 이벤트 기록 기록 유지, 감사 가능성 또는 과거 상태 재구성이 중요한 시나리오에서 특히 유용하다. 
> - 금융, 의료, 공급망 또는 작업 기록을 추적하고 분석하는 기능이 중요한 영역과 같은 영역에서 유용할 수 있다.



### JPA Annotations
- Java 애플리케이션에서 엔티티 간의 관계를 정의하고 구성하는 데 사용되는 JPA 주석이다. 
- 관계형 데이터베이스의 테이블 간 관계를 Java 애플리케이션의 해당 개체에 매핑하는 데 도움이 된다.

1. `@OneToMany`
- 두 엔터티 간의 일대다 관계를 정의하는 데 사용된다. 
- 일반적으로 상위 엔터티(관계의 한쪽)에 있는 컬렉션 값 필드 또는 속성에 적용된다. 
- 상위 엔티티가 하위 유형의 연관된 엔티티를 여러 개 가질 수 있음을 지정한다.

> 사용 예
```java
@Entity
public class Parent {
    @Id
    private Long id;

    @OneToMany
    private List<Child> children;

    // Getters, setters, constructors, etc.
}
```
- `Parent` 엔터티는 `Child` 엔터티와 일대다 관계를 가진다. 
- `@OneToMany` 주석은 관계를 지정하고 `children` 필드는 연결된 자식 엔터티의 컬렉션을 나타낸다.

2. `@JoinColumn`
- 두 관련 엔터티 간의 조인 열을 정의하는 데 사용된다. 
- 일반적으로 상위 엔터티를 참조하는 하위 엔터티(관계의 여러 측면)의 필드 또는 속성에 적용한다. 
- 조인 열의 이름, 외래 키 제약 조건 및 기타 속성을 사용자 지정할 수 있다.

> 사용 예
```java
@Entity
public class Child {
    @Id
    private Long id;

    @ManyToOne
    @JoinColumn(name = "parent_id")
    private Parent parent;

    // Getters, setters, constructors, etc.
}
```
- 'Child' 엔터티는 'Parent' 엔터티와 다대일 관계를 가진다. 
- `@ManyToOne` 주석은 관계를 지정하고 `@JoinColumn` 주석은 조인 열을 지정한다. 
- `@JoinColumn`의 `name` 속성은 조인 열의 이름을 지정하며 이 경우 "parent_id"로 설정된다.
- `@JoinColumn` 주석은 조인 열의 추가 속성을 정의하기 위해 `referencedColumnName`, `nullable`, `unique`, `insertable`, `updatable` 등과 같은 속성으로 추가로 사용자 정의할 수 있다.

> - `@OneToMany` 및 `@JoinColumn`은 일반적으로 JPA의 엔터티 간에 일대다 관계를 설정하기 위해 함께 사용된다. 
> - `@OneToMany` 주석은 상위 엔티티의 컬렉션 필드에 적용되고 `@JoinColumn` 주석은 상위 엔티티를 참조하는 하위 엔티티의 필드에 적용된다.

## 학습 키워드
- N + 1 problem
- DDD의 Aggregate
- `CascadeType.ALL`
- `orphanRemoval`
- Event Sourcing
- JPA 어노테이션
    - @OneToMany
    - @JoinColumn