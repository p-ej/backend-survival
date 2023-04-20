# Collection Pattern

### Collection Pattern
- 아키텍처 스타일인 REST 즉, REST API에 사용되는 패턴이다.
- URL 끝에 (엔드포인트?) 특정 유형의 리소스 element를 검색하도록 설계되었다고 한다. 
- 패턴 적용을 한 의미가 그룹화가 안된 리소스를 표현하기에는 번거로우니 그룹화를 시키려 복수형을 써서 이후 연관되는 알맞은 리소스를 표현하자? 이말인 것 같다.

#### 패턴 미적용시
> - /1-user
> - /2-user
> - /3-user
> - POST /users/new 

#### 패턴 적용시
- users
- users/{:id}
- users?user_id={:value}
- 뭔가 상당히 깔끔하고 URL 하나로 이해가 가는 것 같다. 
- GET users/:id
- DELETE users/:id (삭제)
- POST /users
> 오 신기하다

### 다시 본론
- 이렇게 그룹화를 시킬 리소스들을 하나의 URI로 표현한다고 한다. 그리고 일반적으로 복수형을 사용한다.
- 참고로 users/:id에서 리소스의 ID는 URI 그 자체라고 한다...
  - 그러니까 user_id가 리소스의 ID라는 의미가 아니고 리소스 ID를 구성하는 요소 중 하나일 뿐이다.
  - users라는 그룹 내의 식별자 ID라고 생각하고 다름을 확실히 해야할 것 같다. 
   
- 그룹화 시킬 리소스는 복수형으로 나타내고 그게 아니라면 단수형으로 한다. 
- app/notification
   
### 디렉터리 구성
- 게시글에 댓글이 달리는 상태 (실제 우리 회사에서도 URL을 이런 디렉터리 형태로 구성하고 있다. 난 URL 리소스 이름을 정하느라 몇시간을 고민했다.)
- 이런 형태면 조금 복잡할 것 같다.
  - boards/:id/comments/:id/sub_comments/:id
  - boards/:id/comments/:id/sub_comments
  - 안에 로직과 쿼리짜기에 .. 지금도 어렵다.
- comments/:id?sub_comments=:id
- 이렇게 디렉터리 형식으로 구성이 가능하다. 

#### [어려움]
> - **디렉터리 구성이 좀 어려운 것 같다. 각 게시판 좋아요 수 ?**
> - **URL에 쿼리 파라미터나 PATH 변수로 리소스 디렉터리를 구성하지 않으면 Request Body에다가 나머지 정보를 입력해서 요청받게 해도 되는것인가 ?**
> - **주문하기 같은 경우 /orders가 될텐데 /orders?user_id=:value 음...**

**학습 키워드**
- [웹 API 디자인 모범 사례](https://learn.microsoft.com/ko-kr/azure/architecture/best-practices/api-design#organize-the-api-design-around-resources)