# Process Management

> CPU가 프로세스가 여러 개일 때, CPU 스케줄링을 통해 관리하는 것을 말한다. 이때, CPU는 각 프로세스들이 누군지 알아야 관리가 가능하다. 이러한 프로세스들의 특징을 갖고 있는 것이 바로 Process Metadata다.


- Process ID : PID(Process Identification Number)
    - 프로세스 고유 식별 번호
- Process State(프로세스 상태)
    - 프로세스의 현재 상태(준비, 실행, 대기 상태)를 기억시킨다.

- Process Priority(스케줄링 정보)
    - 프로세스 우선순위 등과 같은 스케줄링 관련 정보를 기억시킨다.

- CPU Registers
    - 프로세스의 레지스터 상태를 저장하는 공간 등. CPU 내 범용 레지스터(AX, BX, CX, DX), 데이터 레지스터(SP, BP, SI, DI), 세그먼트 레지스터(CS, DS, ES, SS) 등이 갖고 있는 값을 기억시킨다.

- Owner(계정 정보)
    - CPU 사용시간의 정보(Quantum), 각종 스케줄러에 필요한 정보를 기억시킨다.

- 기억장치 관리 정보
    - 프로그램이 적재될 기억 장치의 상한치, 하한치, 페이지 테이블 등의 정보를 기억시킨다.

- 입출력 정보
    - 프로세스 수행 시 필요한 주변 장치, 파일들의 정보를 기억시킨다.

- 프로그램 카운터(계수기)
    - 다음에 실행되는 명령어의 주소를 기억시킨다.

​
이러한 정보들이 담긴 메타데이터는 프로세스가 생성되면 PCB(Process Control Block)이라는 곳에 저장이 된다.

## PCB(Process Controll Block)
> 프로세스 메타데이터들을 저장해 놓는 곳으로 하나의 PCB 안에는 하나의 프로세스의 정보가 담겨있다.
![image](https://mblogthumb-phinf.pstatic.net/MjAyMDA3MDNfMzYg/MDAxNTkzNzQ2NDQwNTYw.f62otrPY3iNE4edLaxgkM1F3YRBJAVAzJkc0z1FJR8Eg.tr73Ug8AtaJViK2yoGYAlOrZcDQBxYWyYQRwtbsLTJgg.PNG.adamdoha/image.png?type=w800)

**프로그램 실행 -> 프로세스 생성 -> 프로세스 주소 공간에 (코드, 데이터, 스택) 생성 -> 이 프로세스의 메타데이터들이 PCB에 저장**

 - CPU에서는 프로세스의 상태에 따라 교체 작업이 이루어지는데(인터럽트가 발생해서 할당받은 프로세스가 Block 상태가 되고 다른 프로세스를 running으로 바꿀 때), 이때 앞으로 다시 수행할 Block 상태의 프로세스의 상태값을 PCB에 저장해두기 위해 필요하다.
 - Linked list 방식으로 관리됨. 프로세스가 생성되면 해당 PCB가 생성되고 프로세스 완료 시 제거된다. 이렇게 수행 중인 프로세스를 변경할 때, CPU의 레지스터 정보가 변경되는 것을 **ContextSwitching**이라고 한다.

## Context Switching
> CPU가 현재 실행하고 있는 Task(Process, Thread)의 상태를 저장하고, 다음 진행할 Task의 상태 및 Register 값들에 대한 정보(Context)를 읽어 새로운 Task의 Context 정보로 교체하는 과정을 말한다. 다르게 말하면, CPU가 이전의 프로세스 상태를 PCB에 보관하고, 또 다른 프로세스의 정보를 PCB에서 읽어서 레지스터에 적재하는 과정을 말하는데, 다르게 말해보면 다중 프로그래밍 시스템에서 CPU가 할당되는 프로세스를 변경하기 위해 현재 CPU를 사용하여 실행되고 있는 프로세서의 상태 정보를 저장하고 제어권을 인터럽트 서비스 루틴(ISR)에게 넘기는 작업을 말한다.

### context Switching 수행 과정
1. Task의 대부분 정보는 Register에 저장되고 PCB(Process Control Block)로 관리됨

2. 현재 실행하고 있는 Task의 PCB 정보를 저장(Process Stack, Ready Queue)

3. 다음 실행할 Task의 PCB 정보를 읽어 Register에 적재하고 CPU가 이전에 진행했던 과정을 연속적으로 수행