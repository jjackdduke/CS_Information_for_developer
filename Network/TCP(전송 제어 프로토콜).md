# TCP(전송 제어 프로토콜)

> Transmission Control Protocol : 두개의 호스트를 연결하고 데이터 스트림을 교환하게 해주는 중요한 네트워크 프로토콜

TCP는 에러가 없이 패킷이 신뢰할 수 있게 전달 되었는지 보증하는 역할을 담당한다.

**TCP의 특징**

- 연결 지향 방식
  - 패킷을 전송하기 위한 논리적 경로를 배정
- 3-way handshaking과정을 통해 연결을 설정하고 4-way handshaking을 통해 해제
  - 3-way handshaking : 목적지와 수신지를 확실히하여 정확한 전송을 보장하기 위해서 세션을 수립하는 과정
- 흐름 제어 및 혼잡 제어
- 높은 신뢰성을 보장
- UDP보다 속도가 느리다
- 전이중(Full-Duplex), 점대점(Point to Point) 방식

=> TCP는 연속성보다 신뢰성 있는 전송이 중요할 때에 사용하는 프로토콜 / 예시 : 파일 전송



# TCP 3-way Handshake

> 연결하고자 하는 두 장치 간의 논리적 접속을 성립하기 위해 사용하는 연결 확인 방식으로, 3번의 확인 과정을 수행

![스크린샷 2021-05-08 오후 10.49.13](https://goodgid.github.io/assets/img/network/tcp_ip_3way_4way_1.png)

### 3-way handshake 기본 매커니즘

- TCP 통신은 PAR(Positive Acknowledgement with Re-transmission)을 통해 신뢰적인 통신을 제공

### 작동 방식

- client는 server와 연결하기 위해 3-way handshake를 통해 연결 요청

- 연결 요청을 먼저 시도한 요청자 : Client / 연결 요청을 받은 수신자 : Server

  - 양측 모두 먼저 연결 요청을 시도할 수 있으므로, 먼저 시도 여부에 따라 client / server를 나눔

- **SYN(Synchronization)** : 연결 요청, 세션을 설정하는데 사용되면 초기에 시퀀스 번호를 보냄

- **ACK(Acknowledgement)** : 보낸 시퀀스 번호에 TCP 계층에서의 길이 또는 양을 더한 것과 같은 값을 ACK에 포함하여 전송

  - 동기화 요청에 대한 답변 : `Client의 Sequence Number + 1`을 하여 ACK로 반환

- **Step1(SYN)**

  **클라이언트는 서버와 커넥션을 연결하기 위해 SYN을 보낸다.(seq : x)**

  - 송신자가 최초로 데이터를 전송할 때 Sequence Number를 임의의 랜덤 숫자로 지정하고, SYN 플래그 비트를 1로 설정한 세그먼트를 전송
  - PORT 상태
    - Client : `CLOSED - SYS_SENT로 변함`
    - Server : `LISTEN`

- **Step2(SYN + ACK)**

  **서버가 SYN(x)를 받고, 클라이언트로 받았다는 신호인 ACK와 SYN 패킷을 보냄(seq : y, ACK : x + 1)**

  - 접속 요청을 받은 Server가 요청을 수락했으며, 접속 요청 프로세스인 Client도 포트를 열어달라는 메세지를 전송(SYN-ACK signal bits set)
  - ACK Number필드를 Sequence Number + 1로 지정하고 SYN과 ACK 플래그 비트를 1로 설정한 새그먼트 전송(`Seq = y, Ack = x + 1, SYN, ACK`)
  - Port 상태
    - Client : `CLOSED`
    - Server : `SYN_RCV`

- **Step3(ACK)**

  **클라이언트는 서버의 응답인 ACK(x+1)와 SYN(y)를 서버로 보냄**

  - 마지막으로 접속 요청 프로세스인 서버가 수락 확인을 보내 연결을 맺음(ACK)
  - 이떄, 전송할 데이터가 있으면 이 단계에서 데이터를 전송할 수 있다.
  - PORT 상태
    - Client : `ESTABLISED`
    - Server : `SYN_RCV` => ACK => `ESTABLISED`

### full-duplex 통신의 구성

- Step 1, 2에서는 Client -> Server 방향에 대한 연결 파라미터(시퀀스 번호)를 설정하고 이를 승인
- Step 2, 3에서는 Server -> Client 방향에 대한 연결 파라미터(시퀀스 번호)를 설정하고 이를 승인
  - 이를 통해 full-duplex 통신이 구축



# TCP 4-Way Handshake

> TCP 연결을 해제(Connection Termination)하는 과정

**FIN(finish)** : 세션을 종료시키는데 사용되며, 더이상 보낸 데이터가 없음을 표현

### Termination의 종류

1. **Graceful connection release(정상적인 연결 해제)**
   - 정상적인 연결 해제에는 양쪽의 커넥션이 서로 모두 커넥션을 닫을 때까지 연결되어 잇다.
2. **Abrupt connection release(갑작스런 연결 해제)**
   1. 갑자기 한 TCP 엔티티가 연결을 강제로 닫는 경우
   2. 한 사용자가 두 데이터 전송 방향을 모두 닫는 경우

### 작동 방식(Abrupt)

`RST(TCP reset)` 세그먼트가 전송되면 갑작스러운 연결 해제가 수행

**RST 세그먼트 전송 Case**

1. 존재하지 않는 TCP 연결에 대해 비SYN 세그먼트가 수신된 경우
2. 열린 커넥션에서 일부 TCP 구현이 잘못된 헤더가 있는 세그먼트가 수신된 경우
   - RST 세그먼트를보내, 해당 커넥션을 닫아 공격을 방지한다.
3. 일부 구현에서 기존 TCP 연결을 종료해야 하는 경우

**기타**

- 연결을지원하는 리소스가 부족할 때
- 원격 호스트에 연결할 수 없고 응답이 중지 되었을 때

### 작동 방식(Graceful)

연결 요청을 먼저 시도한 요청자를 Client로, 연결 요청을 받은 수신자를 Server 쪽으로 고려

![img](https://blog.kakaocdn.net/dn/qUXSw/btqDWsFNWJw/hVdKIneSYb7UK3wc0pj6Z0/img.png)

- **STEP1 (Client -> Server : FIN(+ACK))**
  - 서버와 클라이언트가 연결된 상태에서 클라이언트가 close를 호출하여 접속을 끊거나 끊는 시도를 한다.
  - 이때, 클라이언트는 서버에게 연결을 종료한다는 FIN 플래그를 보낸다.
    - 이 때, 실질적으로 ACK도 포함되어 있다.
- **STEP2 (Server -> Client : ACK)**
  - 서버는 FIN을 받고, 확인했다는 ACK를 클라이언트에게 보내고 자신의 통신이 끝날 때까지 기다린다(`TIME_WAIT` 상태)
    - Server(수신자)는 ACK Number 필드를 (Sequence Number + 1)로 지정하고, ACK 플래그 비트를 1로 설정한 세그먼트를 전송
  - 서버는 클라이언트에게 응답을 보내고 `CLOSE_WAIT` 상태에 들어간다. 그리고 아직 남은 데이터가 있다면 마저 전송을 마친 후에 close()를 호출
  - 클라이언트에서는 서버에게 ACK를 받은 후에 서버가 남은 데이터 처리를 끝내고 FIN 패킷을 보낼 때까지 기다린다.(`FIN_WAIT2`)
- **STEP3 (Server -> Client : FIN)**
  - 데이터를 모두 보냈다면, 서버는 연결 종료에 합의 한다는 의미의 **FIN** 패킷을 클라이언트에게 보낸 후에, 승인 번호를 보내줄 때 까지 기다리는 `LAST_ACK` 상태로 들어간다
- **STEP4 (Client -> Server : ACK)**
  - 클라이언트는 FIN을 받고, 확인했다는 ACK를 서버로 보낸다
  - 아직 서버로부터 받지 못한 데이터가 있을 수 있으므로 `TIME_WAIT`를 통해 기다린다(실질적인 종료과정 `CLOSED`에 들어가게 된다)
    - 이때 `TIME_WAIT` 상태는 의도치 않은 에러로 인해 연결이 데드락으로 빠지는 것을 방지
      - 데드락(Deadlock) : 교착상태, 운영체제 또는 소프트웨어의 잘못된 자원관리로 인하여 둘 이상의 프로세스가 함께 퍼지는 현상
    - 만약 에러로 인해 종료가 지연되다가 타임이 초과되면 `CLOSED`로 들어간다
- 서버는 ACK를 받은 이후 소켓을 닫는다(Closed)
- `TIME_WAIT` 시간이 끝나면 클라이언트도 닫는다(Closed)



## 참조

**TCP**

https://mangkyu.tistory.com/15

**3-way & 4-way handshake** 

https://bangu4.tistory.com/74

https://velog.io/@averycode/%EB%84%A4%ED%8A%B8%EC%9B%8C%ED%81%AC-TCPUDP%EC%99%80-3-Way-Handshake4-Way-Handshake

https://seongonion.tistory.com/74