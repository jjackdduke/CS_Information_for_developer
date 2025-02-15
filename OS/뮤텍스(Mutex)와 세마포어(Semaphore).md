# 뮤텍스(Mutex)와 세마포어(Semaphore)

## 뮤텍스와 세마포어의 차이

### Toilet problem

> 동시성 프로그래밍의 가장 큰 숙제는 '공유자원 관리'이다. 공유자원을 안전하게 관리하기 위해서는 상호배제(Mutual exclusion)를 달성하는 기법이 필요하다.
> 뮤텍스와 세마포어는 이를 위해 고안된 기법으로 서로 다른 방식으로 상호배제를 달성한다.

### Mutex

뮤텍스는 화장실이 하나 뿐이 없는 식당과 비슷하다. 화장실을 가기 위해서는 카운터에서 열쇠를 받아 가야 한다.
당신이 화장실을 가려고 하는데 카운터에 키가 있으면 화장실에 사람이 없다는 뜻이고 당신은 그열쇠를 이용해 화장실에 들어갈 수 있다.
당신이 화장실에서 행복한 시간을 보내고 있는데 다른 테이블에 있는 어떤 남자가 화장실에 가고 싶어졌다. 이 남자는 아무리 용무가 급하더라도 열쇠가 없기 때문에 화장실에 들어갈 수 없다. 결국 남자는 당신이 용무를 마친 후 나올 때까지 카운터에서 기다려야 한다.
콛이어 옆테이블에 있는 남자도 화장실에 가고 싶어졌고 이 남자 또한 화장실에 들어가기 위해서는 카운터에서 대기해야한다.
이제 당신은 화장실에서 나와 카운터에 키를 돌려놓았다. 이제 기다리던 사람들 중 맨앞에있던 사람은 키를 받을 수 있고 이를 이용해 화장실에 들어갈 수 있다.
이것이 뮤텍스가 동작하는 방식이다. 화장실을 이용하는 사람은 **프로세스 혹은 쓰레드**이며 화장실은 **공유자원**, 화장실 키는 공유자원에 접근하기 위해 필요한 어떤 오브젝트이다.
즉 뮤텍스는 Key에 해당하는 어떤 오브젝트가 있으며 이 오브젝트를 소유한 프로세스 or 스레드 만이 공유자원에 접근할 수 있다.

### Semaphore

세마포어는 손님이 화장실을 좀 더 쉽게 이용할 수 있는 레스토랑이다. 세마포어를 이용하는 레스토랑의 화장실에는 여러 개의 칸이 있다. 그리고 화장실 입구에는 현재 화장실의 빈 칸 개수를 보여주는 전광판이 있다.
만약 당신이 화장실에 가고 싶다면 입구에서 빈 칸의 개수를 확인하고 빈 칸이 1개 이상이라면 빈칸의 개수를 하나 뺀 다음에 화장실로 입장해야한다. 그리고 나올 때 빈 칸의 개수를 하나 더해준다.
모든 칸에 사람이 들어갔을 경우 빈 칸의 개수는 0이 되며, 이때 화장실에 들어가고자 하는 사람이 있다면 빈 칸의 개수가 1로 바뀔 때까지 기다려야 한다.
사람들은 나오면서 빈 칸의 개수를 1씩 더한다. 그리고 기다리던 사람은 이 숫자에서 다시 1을 뺀 다음 화장실로 들어간다.
이처럼 세마포어는 **공통으로 관리하는 하나의 값**을 이용해 상호배제를 달성한다.

세마포어도 뮤텍스와 같고 화장실 빈칸의 개수는 현재 공유자원에 접근할 수 있는 쓰레드, 프로세스의 개수를 나타낸다.

두 기법 모두 완벽한 기법은 아니라 데이터 무결성을 보장할 수 없으며, 데드락이 발생할 수도 있다. 상호배제를 위한 기본적인 기법이며 여기에 좀 더 복잡한 매커니즘을 적용해 꽤나 우아하게 동작하는 프로그램을 짤 수 있다.
