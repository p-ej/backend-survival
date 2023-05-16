# Relational Algerbra


### 관계 대수
- **DBMS 분야의 이론적 틀이자 기본 개념**
- 관계형 데이터베이스에서 데이터를 설명하고 조작하기 위한 수학적 기반을 제공한다.
- 수학에서 산술 연산을 생각하자 1 + 2를 통해 3이라는 결과를 얻듯, Relation도 여러 연산을 통해 새로운 Relation을 얻을 수 있다.
   
#### 특징
- 기본적으로 관계형 데이터베이스의 테이블인 관계에서 동작한다.
- 새 관계를 도출하기 위해 관계에서 수행할 수 있는 일련의 작업으로 구성된다.
- 이 작업은 수학적 집합 이론 및 논리에서 영감을 얻었다고 한다.
- 데이터를 쿼리, 결합 및 변환할 수 있다.
- 즉, 하나 이상의 Relation(테이블)으로 새로운 Relation을 만들 수 있다.
   
#### 기본 연산
1. Selection 선택(σ)
  - 지정된 조건을 만족하는 관계에서 행의 하위 집합을 선택한다. 
  - SQL에선 WHERE 절 사용
   
2. Projection 투영(π)
  - 관계에서 열의 하위 집합을 선택하여 지정된 열만 포함하는 새 관계를 만든다.
  - SQL에서 SELECT 다음으로 지정할 속성들을 표기한다.
    - ex: SELECT name, age FROM user;
   
3. Union 합집합(∪): 두 관계를 수직으로 결합하여 두 입력 관계의 모든 행을 포함하는 새 관계를 생성합니다. 중복은 자동으로 제거됩니다.
   
4. Intersection 교집합(∩)
  - 두 입력 관계에 공통인 행만 포함하는 새 관계를 생성.
  - 공통 행만 뽑아낸다고 생각하면 된다.
   
5. Difference 차이(−)
  - 한 관계에는 있지만 다른 관계에는 없는 행을 포함하는 새 관계를 만든다.
   
6. Cartesian Product 데카르트 곱(×)
  - 첫 번째 관계의 모든 행을 두 번째 관계의 모든 행과 결합하여 결합된 행이 있는 새 관계를 생성한다.
  - 두 개 이상의 테이블을 합쳐 모든 요소들을 뽑아낸다.
  - SQL에선 SELECT * FROM t1, t2;
  - 보통 데카르트 곱을 그대로 사용하지 않고 Selection과 함께 사용한다 그것이 아래의 Join이다.

> 견우, 직녀 등 사람을 다루는 관계 people이 있고, 각 사람이 소유한 물건을 다루는 관계 items가 있을 때, 사람의 이름(name)을 키로 사용한다면 다음과 같이 표현할 수 있다
```
σpeople.name = items.person_name(people × items)

σ = Selection
× = Cartesian Product
```

> SQL은 이렇게 표현한다
```
SELECT people.name, age, gender, items.name AS item_name, usage
FROM people, items
WHERE people.name = items.person_name;
```
   
7. Join 조인(⨝)
  - 공통 속성 또는 조건을 기반으로 두 관계의 행을 결합한다. 
  - 내부(inner) 조인, 왼쪽 조인(left) 및 오른쪽 조인(right)과 같은 다양한 유형의 조인은 서로 다른 기준이 있다.

> SQL
```
SELETE * FROM user A JOIN post B ON A.id = B.user_id
```
- 이렇게 사용할 수도 있다.
- 조인을 사용하면 첫번째 테이블이 기준이 된다. (왼쪽으로부터 ~~한 것들을 합쳐서 생성)
  - 위를 해석하면 유저가 작성한 게시글이 된다.

> - 나는 보통 LEFT JOIN을 사용한다. (혹은 INNER JOIN) 왜냐면 RIGHT JOIN은 Typeorm에선 지원하지 않기 때문이다. (사실 잘 사용하지도 않는다.)
> - 결국 Relation에서 원하는 데이터를 가져오려면 어떤 기준으로 가져올 것인지 생각해야한다. 