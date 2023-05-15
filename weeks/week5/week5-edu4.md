# TDD



### 테스트 주도 개발 
- 단위, 통합 
- V모델에서 테스트를 먼저 준비
- 테스트 주도 개발(TDD)은 실제 구현 코드를 작성하기 전에 테스트 작성을 강조하는 소프트웨어 개발 접근 방식이다. 
- 실패한 테스트를 작성하고 테스트를 통과하도록 코드를 구현한 다음 코드를 리팩토링하는 주기를 따른다. 
- TDD는 코드베이스에서 지속적으로 테스트하고 반복하여 코드 품질, 유지 관리성 및 디자인을 개선하는 것을 목표로 한다.
#### 간단 설명
- 대부분은 구현을 해놓고 그것을 검증하는 코드를 작성한다.
- TDD는 테스트 코드를 먼저 작성한다.
- TDD 접근 방식은 개발자가 코드를 작성하기 전에 원하는 동작과 요구 사항에 대해 생각하도록 권장한다. 
- 테스트를 먼저 작성함으로써 TDD는 문제에 대한 더 나은 이해를 촉진하고 작고 집중적인 반복을 장려하며 개발 프로세스 초기에 버그를 잡는 데 도움이 된다. 
- 또한 더 모듈화되고 테스트 가능한 코드로 이어지고 레거시 가능성이 줄어들며 향후 변경 및 개선을 위한 안전망을 제공한다.

### TDD Cycle
- Red-Green-Refactor 주기라고도 하는 TDD 주기는 세 가지 주요 단계로 구성된다.
1. red 실패 테스트 코드 먼저 작성
    - 테스트를 만족시키는 구현이 없기 때문에 처음에는 테스트가 실패할 것으로 예상됩니다. 이 단계는 테스트가 코드의 부재 또는 잘못된 동작을 감지할 수 있도록 한다.
2. green 최대한 빨리 테스트를 통과
    - 테스트 요구 사항을 충족하는 가장 간단한 코드를 작성하는 데 중점을 둔다.
3. refactor 리팩터링, 가장 중요하지만 많은 사람들이 간과하고 있는 부분이다. 
    - 리팩토링은 코드 동작을 변경하지 않고 중복을 제거하고 코드 구조를 개선하며 성능을 최적화하는 것을 목표로 한다.

> 리팩토링 단계가 끝나면 새로운 테스트로 사이클이 다시 시작되어 실패한 테스트 작성, 코드 구현 및 리팩토링 프로세스를 반복한다. 코드의 정확성을 확인하는 일련의 자동화된 테스트를 유지하면서 소프트웨어를 점진적으로 빌드하기 위해 TDD 주기가 반복적으로 반복된다.

### 추가 이야기
- 계속 해서 어떤것을 만들건지 뚜렷한 목표를 갖는다. 
- 그 목표를 표현 (테스트코드로) 이거를 어케 빨리 통과를 시킬까 이걸 보면서 요렇게 고치면되겠다라고 인지하면서 개발을 진행한다. 

- 어떤거를 깔끔하게 풀고싶다. 테스트를 어떻게든 통과시킴 그것을 리팩토링을 통해 더 나은 코드를 한다. (중점은 리팩토링)

- 또한 클린코드는 유지보수가 좋다. 장기적으로 더 나은 생산성을 위함 (내부 품질연관)

- 테스트 코드를 작성해서 잘 작동하는 믿을 수 있는 근거를 확보한다.


> - 중요한건 연습이다. 사고방식에 익숙해지자.
> - 백엔드 개발자는 적어도 테스트 코드는 작성해야함. 





## 학습 키워드
- TDD 란
    - TDD Cycle