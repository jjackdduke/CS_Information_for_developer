# Hash

> 다양한 데이터를 고정된 길이를 가진 데이터로 매핑(Mapping)한 값
>
> 해시 함수를 구현하여 데이터 값을 해시 값으로 매핑한다.



## 1. Hash의 동작 원리

![img](https://velog.velcdn.com/images%2Fedie_ko%2Fpost%2F733c9c0f-afc9-4557-acf5-972bd1895993%2Fhashtable.jpeg)

- Key를 Hash Function을 통해 Hash value로 매핑하고, 이 Hash Value를 index로 삼아 데이터의 value를 데이터에 저장

### 

## 2. Hash Function

> 임의의 길이를 갖는 메시지를 입력받아서 고정된 길이의 해시값을 출력하는 함수

- 특징
  1. 어떤 입력 값에도 항상 고정된 길이(해시함수에 따라 비슷한 길이까지 포함)의 해시값을 출력
  2. '눈사태 효과' : 입력 값의 일부가 변경되면 전혀 다른 값을 출력
     - 눈사태 효과로 인하여 결과값으로는 입력값을 유추할 수 없다. => 단방향으로만 진행

- **Hash Function**의 예시
  - SHA(Secure Hash Algorithm)



## 3. Hash Collision (해시 충돌)

> 해시 함수는 입력값의 길이가 어떻든 고정된 길이의 값을 출력하기 때문에, 입력값이 다르더라도 같은 결과값을 출력하는 경우가 발생한다.

![img](https://miro.medium.com/max/1400/1*i5JV9RiF17ftnGDvuqVFSA.png)

- 해시 충돌이 1도 없는 Hash Function을 만드는 것은 불가능
  - 해시 테이블의 충돌을 완화하는 방향으로 문제를 보완



## 4. 해시 충돌 완화

![img](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FCoiPf%2Fbtq2qyoJVrN%2FERiH4UbKnKHQyF4R0HGjOk%2Fimg.png)

### (1) 개방 주소법 (Open Addess)

- 한 버킷당 들어갈 수 있는 엔트리는 하나이지만 해시 함수로 얻은 주소가 아닌, **다른 주소에 데이터를 저장할 수 있도록** 허용하는 방법
- 부하율(Load Factor)이 높을 수록(= 테이블에 저장된 데이터의 밀도가 높을수록) 성능이 급격하게 저하

1. **선형 탐사법 Linear Probing**

   ![img](https://miro.medium.com/max/614/1*xN0omiiMDelgCQmmg7zv9Q.png)

   - 선형으로 순차적 검색을 하는 방법
   - 해시 함수로 나온 해시 index에 이미 다른 값이 저장되어 있다면, 해당 해시값에서 고정 폭을 얾겨 다음 해시값에 해당하는 버킷에 액세스
   - 특정 해시 값의 주변이 모두 채워져 있는 일차 군집화(primary clustering) 문제에 취약
     (해시 충돌이 해시 값 전체에 균등하게 발생할 때 유용한 방법)

2. **제곱 탐사법 Quadratic Probing**

   ![img](https://miro.medium.com/max/1400/1*K-C6o_R4caYMrCNLkyWazw.jpeg)

   - 선형 탐사법과 동일하지만, 고정폭이 아니라 제곱으로 증가
   - 데이터의 밀집도가 선형 탐사법보다 낮기 때문에 연쇄적으로 충돌이 발생할 가능성이 적다
   - 캐시의 성능이 떨어져 속도의 문제가 발생

3. **이중 해시법(double probing)**

   ![img](C:\Users\dongi\OneDrive\문서\SSAFY\dongind_oct\CS스터디\CS_Information_for_developer\Data Structure\assets\img.png)

   - 항목을 저장할 다음 위치를 결정할 때, 원래 해시 함수와 다른 별개의 해시 함수를 같이 이용하는 방법



### (2) 분리 연결법 Seperate Chaining

- 개방 주소법과는 달리 한 버킷(슬롯) 당 들어갈 수 잇는 엔드리의 수에 제한을 두지 않는다.
- 버킷은 링크드 리스트(Linked List) 혹은 트리(Tree)를 사용
- 장점
  - index가 변하지 않고 데이터 개수의 제약이 없다
- 단점
  - 메모리 문제를 야기
  - 테이블의 부하율이 따라 선형적으로 성능이 저하 => 부하율이 작을 경우 open addressinbg 방식이 더 빠름