# JDBC

### JDBC
- Java 프로그램이 데이터베이스와 상호 작용할 수 있는 표준 방법을 제공하는 API(Application Programming Interface)이다.
- Java 응용 프로그램은 데이터베이스 연결, SQL 쿼리 실행, 데이터 검색 및 업데이트, 데이터베이스 트랜잭션 관리와 같은 다양한 데이터베이스 작업을 수행할 수 있ek.
- Java와 MySQL, Oracle, PostgreSQL, SQL Server 등과 같은 다양한 데이터베이스 관리 시스템(DBMS) 간의 다리 역할을 합니다.
    - 쉽게 이해하자면 DBMS 연결할 수 있는 API?
- 개발자가 데이터베이스 독립적인 코드를 작성할 수 있도록 하는 일련의 클래스 및 인터페이스를 제공한다. 
    - 즉, 동일한 코드가 수정 없이 다른 데이터베이스에서 동작할 수 있다.

#### 특징
1. 연결 
  - 데이터베이스와의 연결을 설정하기 위한 클래스와 메소드를 제공한다. 
  - 이는 SQL 문을 실행하고 결과를 검색하여 데이터베이스와 상호 작용하는 데 사용된다.

2. Statement 및 PreparedStatement
  - SQL 쿼리 및 업데이트를 실행하기 위한 Statement 및 PreparedStatement라는 인터페이스를 제공한다. 
  - Statement는 정적 SQL 문 실행을 허용하는 반면 PreparedStatement는 매개 변수화된 SQL 문 실행을 허용하여 더 나은 성능과 보안을 제공한다.

3. ResultSet 
  - 데이터베이스 쿼리의 결과를 검색하고 조작하기 위한 ResultSet 인터페이스를 제공한다. 
  - 결과 집합을 탐색하고, 개별 열에 액세스하고, 열 유형을 기반으로 데이터를 검색할 수 있다.

4. 트랜잭션 관리
  - 트랜잭션 관리를 지원하여 개발자가 데이터베이스 작업을 원자(Atomic) 단위로 그룹화할 수 있도록 한다. 
  - 트랜잭션을 커밋하거나 롤백하는 방법을 제공하여 데이터 일관성과 무결성을 보장한다.

> 트랜잭션은 매우 중요한것.. 막 걸다간 아주 큰 일이 발생한다.

5. 예외 처리
  - 일반적인 데이터베이스 오류 및 예외를 나타내는 확인된 예외 집합을 정의한다. 
  - 우리는 DB로부터 발생되는 예외를 핸들링하여 오류를 정상 처리하고 적절한 오류 처리 혹은 복구 작업을 수행할 수 있다.


> Java SE(Standard Edition) 플랫폼의 핵심 부분으로 데스크탑, 웹 및 엔터프라이즈 애플리케이션을 포함한 Java 애플리케이션의 데이터베이스 연결에 널리 사용된다고 한다.


### 실습 및 테스트

## 학습 키워드
- JDBC(Java Database Connectivity)



[![Hits](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction.svg)](https://hits.sh/p-ej.gitbook.io/devroad-backend/megatera-backend/introduction/)