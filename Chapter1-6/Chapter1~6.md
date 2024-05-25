# 1. 보호 구문 (guard clause)
- 아래와 같이 중첩 코드가 생기면 가독성이 매우 떨어짐
```
if(조건)
    if(다른 조건 부정)
        .... 코드.....
```
- 이럴땐 아래와 같이 정리하자
```
if(조건 부정) return 
else(다른 조건) return 
.... 코드.....
```
- 다만 보호 구문을 남용하지 말자 -> 읽기 매우 까다로워짐

<img src="img.png" alt="drawing" width="700px"/><br>


# 2. 안 쓰는 코드
- 실행되지 않는 코드 지워라 -> 코드가 가치가 있으려면 호출되야 함
- 안쓰는 코드는 찾기 어려울 경우도 있음 -> 예로 리플렉션을 여러번 사용하는 코드
  - 코드 지울때 로그 활용해 지우고 실행해보기
- 해당 코드가 나중에 필요하면? 형상 관리를 통해 재사용
- 코드를 정리할 때 '조금'만 삭제 -> 잘못 고친 걸 쉽게 복구 가능
  - '조금'은 주관적

# 3. 대칭으로 맞추기
- 코드는 마치 유기체처럼 성장 -> 같은 문제라도 상황에 따라 다양하게 변할 수 있음
- 다양한 변화들은 코드를 읽게 어렵게 만듬 -> 읽는 입장에선 일관성이 좋아야 읽기 쉬움
- 아래와 같이 같은 문제 해결에도 여러 가지 표현이 가능함
```
foo()
  return foo if foo not nil
  foo := ...
  return foo

# 기교를 가한 코드
foo()
  return foo not nil
    ? foo
    : foo := ...

# 할당문이 수식이면 더욱 까다로움
foo()
  return foo := foo not nil
      ? foo
      : ...

```
- 코드를 읽을 때, 기존과 다르면 '다른 동직의 코드'로 예단하게 되는 이슈 발생 가능
- 이를 해결하기 위해서는 한 가지 방식을 선택해서 정하고 다른 방식으로 작성된 코드를 해당 방식으로 변경

# 4. 새로운 인터페이스로 기존 루틴 부르기
- 루틴을 호출해야하는데, 기존 인터페이스 때문에 어렵거나, 복잡하거나, 혼란스러울땐 호출하고 싶은 새로운 인터페이스를 새롭게 구현
- 새로 만든 인터페이스는 기존 인터페이스를 호출하는 것으로 구현 가능
- 새롭게 구현한 통로 인터페이스는 소프트웨어 설계에서 작은 중추적인 역할 -> 어떤 동작 변경 시, 통로 인터페이스만 변경 
### 이해 포인트
- 기존 인터페이스와 구현
```
interface OldService {
    void performOldTask(String data, int code);
}

class OldServiceImpl implements OldService {
    @Override
    public void performOldTask(String data, int code) {
        System.out.println("Old Service Implementation: " + data + ", " + code);
    }
}
```
- 새로운 인터페이스와 이를 통해 기존 인터페이스 호출하기
```
interface NewService {
    void performTask(String data);
}

class NewServiceImpl implements NewService {
    private final OldService oldService;

    public NewServiceImpl(OldService oldService) {
        this.oldService = oldService;
    }

    @Override
    public void performTask(String data) {
        // 새로운 인터페이스를 통해 기존 인터페이스 호출
        int defaultCode = 100;  // 예를 들어, 기본값 설정
        oldService.performOldTask(data, defaultCode);
    }
}
```
- 활용 예시
```
public class Main {
    public static void main(String[] args) {
        OldService oldService = new OldServiceImpl();
        NewService newService = new NewServiceImpl(oldService);

        // 새로운 인터페이스 사용
        newService.performTask("Hello, World!");
    }
}
```
- 인터페이스를 쓰지 않으면 main 함수에 oldService에 적용되어야할 로직(default 값)이 포함<br>
  -> 추후 다른 호출 부분에도 해당 로직(default 값)에 복사/붙여 넣기 또는 로직 변경 시 수정을 해야하는 좋지 않은 상황 발생(SRP 위반)

# 5. 읽는 순서
- 코드를 여러 사람이 보기 때문에 읽기 좋은 순서로 정렬하자
- 완벽한 순서는 없음 -> 때로는 기본 요소를 먼저 이해한 다음 구성 방법을 이해하고 싶을때도 있고, API를 먼저 이해한 다음 세부 구현을 이해하고 싶을때도 있음
- 본인의 경험을 살려 순서를 판단하자 -> 어떤 순서의 코드가 제일 좋은지
### 이해 포인트
- 요구사항에 대한 구현도 좋지만 그 코드 가독성에 중요성을 개발자가 인지하고 코드를 짜자 라는 것으로 이해함

# 6. 응집도 높은 배치
- 코드를 읽다가 변경해야 할 동작이 여러 곳에 흩어진 부분도 같이 변경해야한다는 것은 매우 불편한 상황
- 이를 해결하는 좋은 방법은 코드의 순서를 바꿔서 변경할 요소들을 가까이 두는 것
- 두 루틴에 결한도가 있으면 서로 옆에 두는게 좋음(같은 디렉토리에 넣을 수도 있음)
- 결합도 제거도 좋지만 아래와 같은 어려울수도 있음
  - 당장 어떻게 해야할지 모를 때
  - 지금 당장 시간이 없을 정도로 급할 때
  - 팀이 충분한 변경을 수행하고 있을때 -> 중간에 변경하면 작업한 팀원과의 마찰 발생
- 동작 변경에만 매달리지 말고, 응집도를 높이는 순서로 정리하면 코드 변경이 더 쉬움
- 응집도가 좋아지면 결합도 역시 덩달아 좋아짐