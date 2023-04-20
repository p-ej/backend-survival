# Spring Web MVC로 구현

### 코드 구현 실습




### 어노테이션 정리
#### RequestMapping
- 요청을 특정 핸들러 메서드에 매핑하는 데에 사용되는 어노테이션이다.
- 핵심 어노테이션 중 하나라고 한다.
- URL 패턴이나 요청을 처리해야 하는 해당 핸들러 메서드를 정의하는 데 사용한다. (/users, /users/:id, ... 이런것들에 대한 메서드를 만들때)
- 인자에는 URL PATH와 HTTP 메소드를 전달해주면 된다.
- 클래스와 메서드 수준에서 사용가능

#### Get, Post, Patch, DeleteMapping은 무엇일까 ?
- RequestMapping이 클래스와 메서드 둘 다 사용 가능하다면, 해당 것들은 메소드 수준에서만 사용이 가능하다고 한다.
- 보다보면 RequestMapping이랑 큰 차이는 없는것 같다. 그냥 메서드별로 구분할라고 나눠놓은 것 같다. 
    - 여기 안에도 RequestMapping이 들어있네 ..?
    - 확실히 어노테이션 별로 GET, POST를 구분해 놓으면 가독성에 좋을것도 같다. 또 PATH만 적으면되니...

#### PathVariable
- URL에 파라미터로 들어오는 값들을 위한 것.
- "localhost/users/{id}"

## 학습 키워드
- @RequestMapping
    - @GetMapping
    - @PostMapping
    - @PatchMapping
    - @DeleteMapping
    - @PathVariable
- @RequestBody
- @ExceptionHandler
- @ResponseStatus