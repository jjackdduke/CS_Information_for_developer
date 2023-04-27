# String vs StringBuffer/StringBuilder

- String과 StringBuffer/StringBuilder 클래스의 가장 큰 차이점은 String은 불변(immutable)의 속성을 갖는다는 점이다.
  ![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F99948B355E2F13350F)

- 즉 String은 값이 바뀌면 이전 값이 잠시 남아있는다.
- 위와 같이 String은 불변성을 가지기 때문에 변하지 않는 문자열을 자주 읽어들이는 경우 String을 사용해 주시면 좋은 성능을 기대할 수 있다. 그러나 문자열 추가,수정,삭제 등의 연산이 빈번하게 발생하는 알고리즘에 String 클래스를 사용하면 힙 메모리(Heap)에 많은 임시 가비지(Garbage)가 생성되어 힙메모리가 부족으로 어플리케이션 성능에 치명적인 영향을 끼치게 된다.
- String 과는 반대로 StringBuffer/StringBuilder 는 가변성 가지기 때문에 .append() .delete() 등의 API를 이용하여 동일 객체내에서 문자열을 변경하는 것이 가능하다.
  ![image](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Ft1.daumcdn.net%2Fcfile%2Ftistory%2F9923A9505E2F133608)

# StringBuffer vs StringBuilder

- 가장 큰 차이점은 동기화의 유무로써 StringBuffer는 동기화 키워드를 지원하여 멀티쓰레드 환경에서 안전하다는 점(thread-safe)이다. 참고로 String도 불변성을 가지기때문에 마찬가지로 멀티쓰레드 환경에서의 안정성(thread-safe)을 가지고 있다.
- 반대로 StringBuilder는 동기화를 지원하지 않기때문에 멀티쓰레드 환경에서 사용하는 것은 적합하지 않지만 동기화를 고려하지 않는 만큼 단일쓰레드에서의 성능은 StringBuffer 보다 뛰어나다.

## 3줄요약

String : 문자열 연산이 적고 멀티쓰레드 환경일 경우
StringBuffer : 문자열 연산이 많고 멀티쓰레드 환경일 경우
StringBuilder : 문자열 연산이 많고 단일쓰레드이거나 동기화를 고려하지 않아도 되는 경우
