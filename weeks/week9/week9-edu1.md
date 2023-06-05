# SOLID


### SOLID 원칙
- 소프트웨어 설계를 보다 유지 관리 가능하고 유연하며 견고하게 만드는 것을 목표로 하는 5가지 객체지향 설계 원칙을 나타내는 약어이다. 
- 그리고 이 원칙을 바탕으로 어떠한 설계를 해나가야겠다가 바로 Hexagonal Architecture이다.

#### 종류
- SOLID는 아래와 같이 5가지로 구분되는데 하나씩 알아보도록 하자.
1. SRP 단일 책임
2. OCP 개방-폐쇄 원칙
3. LSP 리스코프 치환 원칙
4. ISP 인터페이스 분리 원칙
5. DIP 의존관계 역전 원칙


### SRP (Single Responsibility Principle, 단일 책임 원칙)
- 클래스가 변경되어야 하는 이유는 단 하나여야 한다. 
- 이는 클래스가 수행할 책임이나 작업이 하나만 있어야 함을 의미한다.
- 응집도를 다루는 원칙이다. 
- 여기서 말하는 `책임`이 하나라는 것은 무엇일까 ?
  - 클래스에 여러가지 책임을 부여하면서 여기 바뀌어서 다시 바꾸고, 저기 바뀌어서 다시 바꾸고가 없어야 한다라는 것을 뜻한다. 그러니까 하나당 한개의 책임만있으면 해당 클래스는 한개만 바꾸면 된다.

```
- 예를들어서 하나의 서비스에 1. 데이터를 검증하는 로직과 2. DB와 연결하여 데이터를 가져오는 SQL 로직도 같이 있다고 생각하자, 이 둘은 서로가 서로를 관여하게 묶여있다.  
  - 책임을 서로에게 위임하는것이 아닌 책임을 나눠갖는 상태이라고 생각하면 이해하기 편할것 같다.
- 이렇게 되었을때 처리해야하는 데이터가 변경되어 로직과 SQL을 수정해야한다면 둘 다 수정해야하는 번거로움이 있을 수 있다. 
- 그래서 1과 2를 분리하여 다른 클래스로 구성하면 SQL문이나 DBMS가 바뀌는 상황이 생겨도 서비스 내에서는 바꿀필요가 없다. 또한 1의 경우 도메인 모델에 분리하여 구성하고는 한다.

- 도메인 모델 (혹은 레이어) 그리고 Repository, DAO
```

#### 간단 예시
```java
  public class Order {
    public void calculateTotalPrice() {
      // Logic...
    }

    public void saveOrder() {
      // Logic...
    }

    public void sendConfirmationEmail() {
      // Logic...
    }
  }
```

- 위의 코드에선 `Order` 클래스는 총 가격 계산, 데이터베이스에 저장 및 이메일 전송과 같은 여러 가지 책임이 있기 때문에 SRP를 위반하는 사례이다. 
- 이렇게 책임을 다른 클래스로 분리하는 것이 좋다.


### OCP (Open-Closed Principle, 개방 폐쇄의 원칙)
- 소프트웨어 엔터티(클래스, 모듈, 함수)가 확장에는 열려 있지만 수정에는 닫혀 있어야 한다고한다. 
- 즉, 기존 코드를 수정하지 않고도 새로운 기능을 추가할 수 있어야 한다는 말이다.
  - 수정으로 인한 영향은 밖으로못나가게 닫혀있어야함. 
  - open : 모듈의 기능은 변경할 수 잇어야 함.
  - close : 변경이 다른 곳으로 퍼져나가지 않아야한다. 
- 이것을 해결하기 위해서 추상화를 통해 달성한다. (자바는 인터페이스를 활용)

```
- 어떤 Repository라는 인터페이스를 만들고, JdbcRepository란 클래스로 구현했다고 가정하자
- 어느날 DBMS를 몽고DB로 변경하기로 햇다면 어떻게 될까 ? 
- 여기서 mongoRepository라는 클래스를 새로 만들 수 있고 기존에 생성했던 인터페이스인 Repository를 사용하던 곳은 아무것도 수정하지 않아도 된다.
```

> - OCP는 객체지향 설계의 심장이라고 할 수 있다고한다.
> - 본질은 다형성애 대한 얘기 하지만 OCP란 이름을 붙여서 관리하는게 중요 (**다형성과 OCP의 느낌은 다름**)


#### 간단 예시
```java
public abstract class Shape {
    public abstract double calculateArea();
}

public class Rectangle extends Shape {
    private double length;
    private double width;

    public double calculateArea() {
        return length * width;
    }
}

public class Circle extends Shape {
    private double radius;

    public double calculateArea() {
        return Math.PI * radius * radius;
    }
}
```

- 위 코드에선 Shape 클래스를 수정하지 않고 확장하여 Rectangle, Circle 클래스를 추가할 수 있으므로 확장을 위해 열려있다고 볼 수 있다.
- 이렇게 기존 코드를 유지하면서 새로운 기능을 추가할 수 있다.


### LSP (Liskov Substitution Principle, 리스코프 치환 원칙)
- 프로그램의 정확성에 영향을 주지 않으면서 수퍼클래스의 객체를 서브클래스의 객체로 대체할 수 있어야 한다. 
- 이는 기본 클래스가 사용되는 모든 파생 클래스를 사용할 수 있어야 함을 의미합니다.
  - 즉, 서브타입은 그것의 기반타입(베이스, 슈퍼)으로 치환 가능해야한다.
- 또한 객체의 `행위`를 중점으로 보아야 한다.
- LSP를 지키고 있는지 파악하는 방법 중 하나는 상속이 행위 측면에서 Is-A관계인지 파악하는 것이 중요하다.

```
이런 내용이다.

정사각형은 직사각형인가 ? 
- 수학적인 직관은 그렇다고 이야기 하지만 객체의 행위 측면에서 보면 둘은 엄격히 분리해야한다.
- 올바른 Is-A 관계를 파악하기 위해 X는 Y인가요 > 라고 물을 때 기계적으로 답을 낼 수 앖다. 

```

#### 간단 예시
```java
public class Rectangle {
    private double width;
    private double height;

    public double calculateArea() {
        return width * height;
    }
}

public class Square extends Rectangle {
    public void setWidth(double width) {
        super.setWidth(width);
        super.setHeight(width);
    }

    public void setHeight(double height) {
        super.setWidth(height);
        super.setHeight(height);
    }
}
```


- `Square` 클래스는 상위 클래스 `Rectangle`과 다르게 동작하기 때문에 LSP를 위반한다. 
- 'Rectangle'은 너비와 높이를 독립적으로 설정할 수 있다.
- 하지만 'Square'는 동일한 너비와 높이를 적용한다. 
- 이는 `Rectangle`을 예상하는 코드가 `Square`가 주어질 때 잘못 작동할 수 있기 때문에 대체 가능성 원칙을 위반한다.

```
@Test
void area() {
    Rectangle rectangle = new Square();
    rectangle.setWidth(5);
    rectangle.setHeight(4);
    assertThat(rectangle.calculateArea()).toBe(5 * 4);
}
```

- 이 경우엔 Square를 생성하는 곳이 가까워서 문제의 원인을 추측하기 쉬워보인다. 
- 하지만 프로그램이 거대해지고 객체를 생성하는 곳과 사용하는 곳이 멀어진다면 원인 파악은 물론이오 버그와 싸우는 일이 많아질 것.
- Spring은 객체를 생성하는 곳과 멀어져도 가까운것처럼 가능하도록 적극 돕는다.


### ISP (Interface Segregation Principle, 인터페이스 분리 원칙)
- 클라이언트가 사용하지 않는 인터페이스에 의존하도록 강요해서는 안 된다고 말한다. 
- 클래스가 인터페이스에서 불필요한 메서드를 구현하도록 강요해서는 안 된다는 의미이다.
  - 결국은 응집력이 없는 커다란 인터페이스를 여러 개의 작은 인터페이스로 나누자.
  - SearchRepository, FindRepository, CommandRepository, ...

```
// 원래는 분리된 인터페이스를 구현하는 클래스가 있어야 함.
public class ProductRepository implements SearchProductRepo, CommandProductRepo {
}

// Spring Data JPA를 사용하니 이렇게 사용
public interface ProductRepository extends SearchProductRepository, CommandProductRepository, CrudRepository<Product, ProductId> {
}

```

- 하나의 구현이 여러 인터페이스를 구현 가능하다.
- Spring Data JPA라면 하나의 인터페이스가 여러 인터페이스를 상속하는 형태로 작성 가능하다. 

- 이제는 UseCase를 사용한다.
- 인터페이스로 UseCase를 만들고 구현체는 Service하나로 구현
1. UseCase 개수만큼 Public method 생김 + 생성자도 public으로 추가
2. private은 원하는 대로 

#### 간단 예시
- 구현부는 하나로 뭉치고 인터페이스만 분리한다. ISP
```java
public interface CreateProductUseCase {
	void createProduct(String name);
}

public interface SearchProductUseCase {
	List<Product> searchByName(String name);
}

public class ProductServcie implements CreateProductUseCase, SearchProductUseCase {

}
```

- 잘못된예시
```java
public interface Printer {
    void print();
    void scan();
    void fax();
}

public class LaserPrinter implements Printer {
    public void print() {
    }

    public void scan() {
    }

    public void fax() {
    }
}

public class InkjetPrinter implements Printer {
    public void print() {
    }

    public void scan() {
    }

    public void fax() {
      // 이 방법은 잉크젯 프린터에는 적용되지 않습니다.
      new UnsupportedOperationException()
    }
}
```

- `Printer` 인터페이스는 `InkjetPrinter` 클래스가 `fax()` 메서드를 적용할 수 없음에도 불구하고 강제로 구현하기 때문에 ISP를 위반한다. 
- 이로 인해 불필요한 종속성과 부풀려진 구현이 발생할 수 있다. 
- `프린터` 인터페이스를 더 작고 더 집중된 인터페이스로 분할하는 것이 좋다.



### DIP (Dependency Inversion Principle, 의존관계 역전 원칙)
- 상위 수준 모듈이 하위 수준 모듈에 의존해서는 안 된다. 
- 둘 다 추상화에 의존해야 한다. 
- 이는 종속성이 구체적인 구현이 아닌 인터페이스 또는 추상 클래스에 있어야 함을 의미한다.

```
- 여기서 상위 수준의 모듈은 비즈니스 로직을 다루는 부분 (상위 도메인 모델인 것들)
- 하위 수준의 모듈은 상위 수준의 모듈에서 호출하는 기술적인 부분을 의미 (디비 같은 것들)
```

- AddProductToCartServcie가 JdbcCartRepository에 의존한다면 비즈니스 로직에 가까운 Application Layer의 객체가 기술적인 문제를 다루는 Infrastructure Layer의 객체에 의존하는 꼴이 된다.
  - 이는 근본적으로 기술적인 문제가 바뀔 때 비즈니스 로직에 영향을 주게 된다. 
  - 그래서 의존성 방향을 Application Layer -> infrastructure Layer에서 역전하여 infrastructure Layer -> Application Layer로 만드는 설계가 필요하다


의존성방향
- 영향을 주는 쪽이 의존되는 쪽 
- 영향을 받는쪽이 의존하는 쪽


> - 주의할점은 역전이랑 주입이랑은 다르다 
> - 인터페이스에 의존하면서 구체적인 인스턴스를 활용하기 위해서는 DI가 필요하고 스프링을 사용하면 아주 쉽게 DI와 DIP를 활용할 수 있게 된다. 
> - DI를 쓰면 편함  (DI가 필요한건 아님)

#### 간단 예시
```java
public class OrderService {
    private final OrderRepository orderRepository;

    public OrderService(OrderRepository orderRepository) {
        this.orderRepository = orderRepository;
    }

    public void createOrder(Order order) {
        // Logic
        orderRepository.save(order);
    }
}

public interface OrderRepository {
    void save(Order order);
}

public class JdbcOrderRepository implements OrderRepository {
    public void save(Order order) {
        // Logic
    }
}

public class MongoOrderRepository implements OrderRepository {
    public void save(Order order) {
        // Logic
    }
}
```

- 'OrderService'는 'OrderRepository' 인터페이스에 의존한다. 
- 이를 통해 `OrderService`는 코드를 수정하지 않고도 `JdbcOrderRepository` 또는 `MongoOrderRepository`와 같은 `OrderRepository`의 모든 구현과 함께 작동할 수 있다. 
- 구현 선택을 외부에서 구성하여 유연성을 높이고, 테스트를 쉽게 할 수 있다.