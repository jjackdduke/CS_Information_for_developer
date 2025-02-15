# IPC(Inter Process Communication)
  - 실행 프로세스 간에 통신을 가능케하는 메커니즘
     - 프로세스 간에 서로 통신하는 규칙에 관한 문제

  - 호스트의 기종, 운영체제 등이 무엇인지 관계없이 일정 규칙으로 통신하는 것이 매우 중요함
     - 통상, 두 프로세스 간에 일정 포멧과 순서를 따르는 메세지를 주고 받으며 통신을 하게됨

  - 결국, 멀티태스킹 운영체제 내 또는 네트워크화(Networked)/분산된(Distributed) 컴퓨터들 사이에 실행되는 프로세스 간에 정보의 교환을 가능케 하는 기법
  

## IPC 구현 기법들 구분

  - 전통적인 주요 기법 둘
     - 공유 메모리 기법 (Shared Memory)
        - 협력적 프로세스들이 공유 메모리 영역을 통해 데이터 교환
           - 주로, 전역 변수, 공유 변수, 공유 파일을 통해 통신을 이룸
        - 충돌 가능성은 있으나, 일반적으로 더 빠르고 커널 도움이 거의 필요 없음
           - 즉, 고속 통신이 가능하나, 통신 책임이 응용 프로세스에 있고, 
           - 운영체제는 단지 공유 메모리 만 제공 함
     - 메세지 전달 기법 (Message Passing)
        - 협력적 프로세스 간에 메세지 교환
        - 충돌하지 않으나, 적은 양의 데이터 교환 위주
        - 통신 책임 및 수단이 운영체제에 의해 제공됨

  -  유닉스 초기 IPC 구현 3가지 기법 
     - 공유 메모리 (Shared Memory)
        - 다중 프로세스들이 가상 메모리(Virtual Memory)를 공유
        - 협력 프로세스들 간에 공유되는 메모리 영역이 운영체제에 의해 구축 제공됨
     - 세마포어 (Semaphore) : 공유 자료에 대한 접근을 통제하는 일종의 카운터
     - 메세지 큐 (Message Queue)

  -  메세지 전달 (Message Passing)에 기반을 둔 기법들
     - 시그널 (Signal)
        - 실시간으로 프로세스에 이벤트 발생을 알리기 위함 
     - 세마포어 (Semaphore)
        - 공유 자료에 대한 접근을 통제하는 일종의 카운터
     - 파이프 (Pipe)
        - 2개의 프로세스 입출력을 직렬 연결하여 하나의 공유 페이지를 제공
           - ls | pr (ls를 수행한 표준출력이 pr의 표준입력으로 전달 됨)
        - 이름 없는 파이프 : 상호 관련 있는 프로세스 간 통신에 사용
        - 이름 있는 파이프(Named Pipe) : 장치 파일을 통해, 상호 관련 없는 프로세스 간 통신에 사용
     - 소켓 (Socket)
        - 네트워크에 기반을 둔 IPC 메카니즘
        - 파이프 개념을 네트워크로 확장시킨 것
     - RPC (Remote Procedure Call) : 클라이언트-서버 모델을 이용하는 기법
        - 원격지 프로세스 간에 함수의 호출에 기반을 둔 기법
        - 한편, LPC (Local Procedure Call)는 동일 시스템내에서 호출 기법을 말함


## 통신계층의 관점
IPC는 트랜스포트 계층(Layer 4) 또는 세션계층(Layer 5)에서 이루어짐