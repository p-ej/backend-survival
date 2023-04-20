# Spring Web MVC로 구현

### 어노테이션 정리
#### RequestMapping
- 요청을 특정 핸들러 메서드에 매핑하는 데에 사용되는 어노테이션이다.
- 핵심 어노테이션 중 하나라고 한다.
- URL 패턴이나 요청을 처리해야 하는 해당 핸들러 메서드를 정의하는 데 사용한다. (/users, /users/:id, ... 이런것들에 대한 메서드를 만들때)
- 인자에는 URL PATH와 HTTP 메소드를 전달해주면 된다.
- 클래스와 메서드 수준에서 사용가능

```java
@RestController
@RequestMapping("/posts")
public class PostController {
}
```

#### Get, Post, Patch, DeleteMapping은 무엇일까 ?
- RequestMapping이 클래스와 메서드 둘 다 사용 가능하다면, 해당 것들은 메소드 수준에서만 사용이 가능하다고 한다.
- 보다보면 RequestMapping이랑 큰 차이는 없는것 같다. 그냥 메서드별로 구분할라고 나눠놓은 것 같다. 
    - 여기 안에도 RequestMapping이 들어있네 ..?
    - 확실히 어노테이션 별로 GET, POST를 구분해 놓으면 가독성에 좋을것도 같다. 또 PATH만 적으면되니...

```java
@PostMapping
메서드 {}

@PatchMapping("/{id}")
메서드 {}
```

#### PathVariable
- URL에 파라미터로 들어오는 값들을 위한 것.
- PATH에 {} 중괄호로 된 변수를 가져올 수 있다. 
- "localhost/users/{id}"

``` java
@DeleteMapping("/{id}")
public String delete(@PathVariable("id") String id) {
    return "게시물 삭제: " + id;
}
```

#### RequestBody
- HTTP 메세지에서 Body에 담기는 데이터를 받을 수 있는 어노테이션이다. 

```java
@PostMapping
@ResponseStatus(HttpStatus.CREATED)
public String create(@RequestBody String body) {
    return "게시물 생성: " + body;
}
```


#### ExceptionHandler
- Controller Layer에서 발생하는 에러를 잡아서 메서드로 처리해주는 기능이라고 한다.
- Service, Repository에서 발생하는 에러는 제외한다고 하는데 나중에 알아봐야겠다.
- value라고 어떤 예외를 처리할 것인지 설정해 줄 수 있는데, 설정하지 않으면 모든 예외를 잡는다고 한다.
> @ControllerAdvice를 사용해서 처리하는 방법도 있던데 알아봐야겠다.

```java
@GetMapping("/{id}")
public String detail(@PathVariable String id) {
    if (id.equals("404")) {
        throw new PostNotFound();
    }
    return "게시물 상세: " + id + "\n";
}

@ExceptionHandler(PostNotFound.class)
@ResponseStatus(HttpStatus.NOT_FOUND)
public String postNotFound() {
    return "게시물을 찾을 수 없다.\n";
}
```



#### ResponseStatus
- 서버에서 Response를 할 때 HTTP 상태 코드를 설정할 수 있다.
```java
@GetMapping("/{id}")
@ResponseStatus(HttpStatus.NOT_FOUND) // 예시
public String detail(@PathVariable String id) {
    if (id.equals("404")) {
        throw new PostNotFound();
    }
    return "게시물 상세: " + id + "\n";
    }
```


#### [중간]
> - 코드로 구현하고 코드로 이해하는건 뭔가 더 편하다 
> - 개념적으로 헷갈렸던 부분들도 뭔가 코드로 구성하다보면 이해해될때도 많다.
> - 어노테이션이 많아서 그렇지 계속 하나씩 뜯어봐야겠다.


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