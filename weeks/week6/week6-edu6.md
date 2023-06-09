# JdbcTemplate


### JdbcTemplate
- **JDBC(Java Database Connectivity)의 사용을 단순화하고 Java 애플리케이션에서 데이터베이스 작업의 생산성을 향상시키는 Spring Framework에서 제공하는 클래스이다.**
- Spring JDBC 모듈의 일부이며 JDBC를 사용하여 관계형 데이터베이스와 상호 작용하기 위한 중앙 구성 요소 역할을 한다.

#### 특징
- JdbcTemplate 클래스는 JDBC API의 복잡성을 추상화하고 SQL 쿼리 실행, 결과 처리, 예외 처리 및 데이터베이스 트랜잭션 관리를 위한 더 높은 수준의 편리한 API를 제공한다.
    - 일반적으로 JDBC와 관련된 상용구 코드를 제거하여 데이터베이스 액세스 코드를 더 깨끗하고 간결하게 만든다.
    - 코드의 단순화 혹은 사용하기 편리하게 만듦, 기존의 단점을 개선

#### 이녀석은 크게 아래와 같이 나눌 수 있다.
1. 간소화된 데이터베이스 작업
  - 연결 관리, 명령문 생성 및 실행, 결과 집합 처리 및 예외 변환과 같은 데이터베이스 작업의 하위 수준 세부 정보를 처리한다. 
  - 우리는 기본 JDBC 인프라에 대한 걱정 없이 SQL 쿼리를 작성하고 결과를 처리하는 데 집중할 수 있다.
  - 기존 JDBC보다 더 쉽게 사용할 수 있다는 것 같다. Spring Framework -> Spring Boot를 사용하는 느낌인가 ?

2. 매개변수화된 쿼리
  - 성능을 개선하고 SQL 주입 공격(SQL 인젝션인가보다)을 방지하는 PreparedStatement를 사용하여 매개변수화된 쿼리 실행을 지원한다. 
  - 매개변수 바인딩 및 변환을 수동으로 처리하지 않고도 SQL 문에 매개변수를 안전하게 전달할 수 있습니다.

3. 결과 집합 처리
  - ResultSet 개체에서 데이터를 추출하고 이를 Java 개체에 매핑하는 메서드를 제공한다. 
  - 행 매핑, 열 매핑, 컬렉션으로의 결과 집합 추출 등 다양한 데이터 추출 기술을 지원한다.
  - 이 부분은 좀 더 봐야겠다.

4. 예외 변환
  - JDBC SQLException을 보다 의미 있고 이식 가능한 DataAccessException으로 변환하여 다양한 데이터베이스 공급업체에서 일관된 예외 처리를 허용한다. 
  - 오류 처리를 단순화하고 데이터베이스 관련 오류에 대한 통합 예외 계층 구조를 제공한다
  - `내가 느끼기엔 뭐든 에러 처리가 까다롭다고 생각했는데 이런걸 지원하면 편리하다.`

5. 트랜잭션 지원
  - Spring의 트랜잭션 관리 기능과 통합되어 우리가 데이터베이스 트랜잭션을 선언적으로 관리할 수 있도록 한다. 
  - 프로그래밍 및 선언적 트랜잭션 관리를 모두 지원하여 다단계 데이터베이스 작업에서 데이터 무결성과 일관성을 보장한다.
  - `Spring은 여러 상황에 대한 트랜잭션 기능들이 많은 것으로 알고있다. 그래도 역시나 트랜잭션은 적절하게 사용하는것이 제일 좋다는 말...`


> 전반적으로 JdbcTemplate은 데이터베이스로 작업할 때 기존의 JDBC보다 편리함을 제공하는 것 같다.


### 실습 및 테스트

## 학습 키워드
- JdbcTemplate



[![Hits](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction.svg)](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction/)