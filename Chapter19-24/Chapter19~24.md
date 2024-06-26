# 19. 리듬
- 코드 정리를 관리하는 기술 중에는 정리의 리듬을 관리하는 일도 있음 -> 코드 정리할 때 한번에 처리하는 규모를 작게 처리
- 코드 정리를 선행할 때, 탁월한 장점 중 하나는 코드 정리 내용도 뭉쳐진다는 것 -> 동작 변경은 코드 안에 뭉쳐서 나타나는 경향이 존재
- 계속 코드 정리를 한다면 대부분의 변경 작업은 이미 정리된 코드 안에서 이루어짐 -> 미래에는 정리되지 않은 코드를 만나는 일이 급격히 줄어듬
- 
# 20. 얽힘 풀기
- 코드 정리 후 정리한 코드와 변경할 동작이 함께 얽혀 버리는 상황 발생
- 아래와 같은 세가지 선택지가 있지만 전부 이슈가 존재
  - 그대로 배포 -> 검토하는 사람도 무례하다고 느끼며, 오류가 쉽게 발생
  - 코드 정리와 변경 사항을 하나 이상의 PR로 나누거나 하나의 PR을 여러 번의 커밋으로 나눔 -> 무례함은 줄이지만 작업 횟수는 늘어남
  - 진행 중인 작업을 버리고 코드 정리를 선행하는 순서로 다시 시작 -> 작업은 많아지지만, 이어지는 커밋과 일관성은 분명해짐
- 코드는 의도를 사람들에게 설명하는 것 -> 빠른 수행이 최종 목표가 되는 것은 아님
- 실타래를 풀려면 실이 엉켜 있다는 사실을 알아차려야 함 -> 더 빨리 알수록 작업량은 적어짐
# 21. 코드 정리 시점
- 시스템의 동작 변경과 연관된 코드 정리 시점 
### 아예 안한다면
- 미래에도 코드 동작이 변경되지 않을꺼면 코드 정리할 하지 않아도 된다는 말이 합리적일수도 있음
### 나중에 정리하기
- 날던 새가 물리 법칙에 의문을 품고, 갑자기 하늘에서 떨어지는 것과 같은 이야기 -> 언제나 일은 많기에 나중에 정리하기는 힘듬
- 아래와 같은 다른 방식으로 가치 창출
  - 지저분함 보유세를 줄여주는 것
  - 학습 도구로 활용하는 것 -> 설계한 세부 결과를 깨달을 수 있는 좋은 방법
  - 자신이 원할 때하면, 더 의욕적이고 기분도 좋음
### 동작 변경 후에 코드 정리
- 아래와 같은 경우 동작 변경 후 코드 정리
  - 방금 고친 코드를 다시 변경할 예정일 때
  - 지금 정리하는 것이 더 저렴할 때
  - 코드 정리하는 데 드는 시간이 동작 변경에 드는 시간과 거의 비슷할 때
### 코드 정리 후에 동작 변경
- 코드 정리는 선행해야하는 건가? 상황에 따라 다르기에 아래와 같은 질문을 해봐야함
  - 지저분한 상태로 코드 변경하면 일이 더 어려운지 -> 코드를 정리한다고 쉬워지지 않으면 먼저 정리 필요 없음
  - 코드 정리의 이점을 바로 얻을 수 있는지? -> 코드 정리한다면 더 빨리 이해가 되면 먼저 코드 정리
  - 코드 정리에 드는 비용을 보상 받기 좋을때 -> 이 코드를 변경하면 몇년동안 매주 보상 받음
  - 코드 정리에 대해 얼마나 확신이 드는지? -> 추측할 때는 편견에서 벗어나야함
- 코드를 먼저 정리하는 것도 좋지만, 정리 자체가 목적이 되지 않도록 경계 필요
### 요약
- 코드 정리 X
  - 미래 코드 변경 없을 때
  - 설계를 개선하더라도 배울 것이 없을 때
- 나중으로 미루자
  - 정리할 코드 분량이 많은데, 보상이 바로 보이지 않을 때
  - 코드 정리에 대한 보상이 잠재적일 때
  - 작은 묶음으로 여러 번에 나눠서 코드 정리를 할 수 있을 때
- 동작 변경 후에 정리
  - 다음 코드 정리까지 기다릴수록 비용이 더 불어날 때
  - 코드 정리를 하지 않으면 일을 끝냈다는 느낌이 들지 않을 때
- 코드 정리 후 동작 변경
  - 코드 정리가 동작변경에 즉각적인 효과가 있을 때
  - 어떤 코드를 어떻게 정리해야하는지 알고 있을 때

# 22. 요소들을 유익하게 관계 맺는 일
- 소프트웨어 설계 -> 요소들을 유익하게 관계를 맺는 일
### 요소
- 토큰 -> 식 -> 문 -> 함수 -> 객체/모듈 -> 시스템
- 요소는 경계가 있으며 하위 요소를 포함
### 관계 맺기
- 요소들은 서로 관계를 지닌 존재 -> 하나의 함수가 다른 함수를 호출(호출 하고/호출 받는 관계)
- 소프트웨어 설꼐에서 관계는 다음 것들이 존재
  - 호출
  - 발행(Publish)
  - 대기(Listen)
  - 참조(변수의 값을 가져오기)
### 유익하게
- 하나의 설계 작업은 작은 하위 요소로 만든 거대한 스프를 만드는 일과 같음
- 요소들 사이는 분명 존재하지만 잘 드러나지 않은 관계 <> 중간 요소들을 통해 서로에게 도움이 되게 만들면 복잡한 부분을 덜어가면서 더 간단

### 요소들을 유익하게 관계 맺는 일
- 설계란? 설계를 구성하는 요소들과 그들의 관계, 그리고 그 관계에서 파생되는 이점
- 설계자란? 요소들을 유익하게 관계 맺는 일을 하는 사람
- 소프트웨어 설계자는 다음과 같은 일만할 수 있음
  - 요소를 만들고 삭제
  - 관계를 만들고 삭제
  - 관계의 이점을 높임
- 아래와 같은 예시 존재
```
caller()
    return box.width() + box.height()
```
- 윗 예시를 아래와 같이 바꿀수 있음
```
caller()
    return box.area()
    
Box>>area()
    return width() + height()
```
- 두 요소는 하나의 함수 호출로 연결됨 -> caller() 함수가 단순해진 유익해진 대가로 Box가 하나의 더 커진 함수를 갖게됨
- 시스템의 구조
  - 요소 계층 구조
  - 요소 사이의 관계
  - 이러한 관계가 만들어 내는 이점

# 23. 구조와 동작
- 소프트웨어는 두 가지 방식으로 가치를 만듬
  - 현재 소프트웨어가 하는 일
  - 미래에 새로운 일을 시킬 수 있는 가능성
- 동작을 규정하는 방식은 두 가지
  - 입출력 쌍: 입력 값을 통해 출력 값
  - 불변 조건: 지정한 동작에 따른 출력 값은 변하지 않음
- 동작은 가치를 만듬 -> 1달러 소비하여 10달러짜리의 가치적인 역할을 하여 비즈니스를 생성할 수 있음
- 시스템의 동작보단 선택의 가능성(미래에 대한 기능적 확장)을 통해 더 높은 가치를 얻을 수 있음
- 선택의 가능성을 제거하는 몇가지 시나이로
  - 핵심 인력 퇴사 -> 며칠 걸리던 일이 몇달 소요
  - 고객과 거리 멀어짐 -> 요구적 사항 변동 횟수 줄어듬
  - 변경에 따른 비용 치솟음 -> 선택 가능성이 줄어들면 소프트웨어가 만드는 가치도 줄어듬
- 시스템 구조는 동작에 영향을 미치지 않으며, 미래의 기회를 만듬
- 문제는 구조는 동작처럼 또렷하게 드러나지 않음 -> 코드 변경하기 쉽게 구조를 변경했다? 어떻게 알지?
- 구조 변경과 동작 변경은 가치를 만들지만, 근본적으로 다름 -> 되돌릴수 있는 능력인 가역성이 다름

# 24. 경제 이론: 시간 가치와 선택 가능성
- 소프트웨어 설계는 '먼저 벌고 나중에 써야 한다'와 '물건이 아닌 옵션을 만들어야 한다'는 두 가지 속성을 조화시켜야함
  - 오늘의 1달러가 내일의 1달러보다 더 가치가 있기 때문에 버는 것은 빨리하고, 쓰는 것은 가능한 뒤로 미루자
  - 혼란스러운 상황에서는 어떤 물건에 대한 옵션이 물건 자체보다 낫기 때문에 불확실성에 맞서는 옵션을 만듬