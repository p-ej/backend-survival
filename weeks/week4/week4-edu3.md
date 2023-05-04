# Domain Model

4계층을 알아보면서
기본이 되었던 가운데 도메인 

사용자 인터 UI 
응용 (애플리케이션
도메인 
인프라스트럭처 (웹과 DB포괄) - 상속 구현 (참조)

예전에는 컨트롤러를 상속 지금은 어노테이션 지원

지금까진 응용 + 도메인을 묶어서 처리하고있었음 

DDD는 도메인 레이어, 객체를 따로 관리(혹은 분리)를 해줌

[사진]


도메인 모델만 (모델 계층) 따로 구성

이걸 왜 만듦 ?
애플리케이션 레이어 특징을 알아보자 
- 얇게 유지 (거의 있는게 없어야함)
- 업뮤 규칙이나 지식이 포함되지 않음 : 여기서 뭔가 작업을 안함 
    - 이런 순서로만 해줘정도
- 작업을 조정
- 도메인 객체의 협력자에게 작업 위임
    - 도메인 객체들한테 너네가 이거 이렇게 알아서해줘

행위랑 데이터
도메인 모델은 행위를 가장 중요시 함. 유닛 테스팅하기 적합한 존재 

행위 없이 코딩 : 내가 너를 다 파악해서 조작하겠음 - 협력이 아님 - 복잡성 증가)
Long amount = account.getAmount(); // 계좌 남은 금액 얻기
account/setAmount(amount + 10_000);

행위 있다면 :
account.increateAmout(10_000);
내부에서 뭘하든 내가 알게뭐야 다른애한테 위임해줌 


DAO 데이터관리
레포지토리 도메인 모델 관리
JPA 레포지토리는 DAO를 의미

다오는 DB중심
레포지토리 도메인 중심

데이터 매퍼라는게 있음 
JPA 레포도 도메인 데이터 매퍼의 특수형태 
- DDD를 실용적으로 하는 곳. 얘를 잘쓰면 잘 버무림 (DDD entity와)

ERD를 먼저 그림, DAO와 VO(DTO)를 사용하는 방식 (데이터베이스 주도 개발)
- 애플리케이션 레이어에서 뭘 안하지만 이런 방식을 사용하면, 도메인 모델이 없기 때문에 사실상 DTO만 존재 그래서 set, get을 사용함 
- 스키마에 맞춰서 뭔가를 조작

DDD
- 도메인 모델 만들기
- JPA 모델 만들기 
- 둘이 완전히 달라도 됨

ireum 가져오기 스필릿

모델에선 first last 
name

전자는 DB가 바뀌면 프로그램이 바뀜
후자는 프로그램을 먼저 고치고 DB를 변경 

어디서 무게를 두느냐가 중요

DB 조정을 위해 CQRS, Event Sourcing 사용 (모델 분리)
솔루션 자체를 변경하기 쉬움(상대적으로)
- MySQL -> NoSQL

도메인 모델에 대해 무기력하거나 풍부
- 행위 없는 데이터를 다루지마라 (안티패턴)
- 근데 코드양은 적긴함 
    - 못써먹을 정도는 아님

당장은 무기력 그 후 점차 전환

[사진]

￼

- DDD나 풍부한 도메인 모델을 다루는 걸 추천함

책 추천 오브젝트 : 객체지향 설계
DB에 많이 끌려다닌다.


도메인 모델 실습
[사진]
DAO 대신 models 패키지 폴더 생성


기본 구성은 이렇다 그런데 그냥 이대로 하면 도메인 모델로써는 너무 취약하고 DTO와 다를바가 없다.
public class Post {
    private Long id;

    private String title;

    private String content;

    public Post(Long id, String title, String content) {
        this.id = id;
        this.title = title;
        this.content = content;
    }
}

유용하게 사용해보자 
!객체지향

public class PostId {
    private String id;

    public PostId(String id) {
        this.id = id;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        PostId postId = (PostId) o;
        return Objects.equals(id, postId.id);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id);
    }
}

VO 값이 같은지 안같은지 확인 

[사진]
￼


Content 
public class MultilineText {
    private final List<String> lines;

    public MultilineText(String text) {
        this.lines = Arrays.asList(text.split("\n"));
    }

    @Override
    public String toString() {
        return lines.stream().collect(Collectors.joining("\n"));
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (o == null || getClass() != o.getClass()) return false;
        MultilineText that = (MultilineText) o;
        return Objects.equals(lines, that.lines);
    }

    @Override
    public int hashCode() {
        return Objects.hash(lines);
    }
}


Post를 좀 더 풍성하게 만들어주자 


도메인 레이어랑 애플리케이션 레이어를 구분을 해주자 
service 레이어에선 find를 할떄 도메인 모델을 얻게 한다. 
- DAO는 도메인 모델이 아니기때문 

Repository 생성하기 

new 보단 정적 팩토리 메소드로 

최근 도메인 모델은 getter라는 것을 안붙인다.


타입을 잘 잡아놓기만 하면 헷갈릴것이 없다.

주의할 점은 getter는 절대 비즈니스 로직을 위해 사용하지 말것.

service에서 로직을 구현할 필요없음 

레이어를 나누는것 (초보적 수준)
- 응용 , 도메인

좀 더 깔끔한 방법이 있긴함 
스프링자체에서 