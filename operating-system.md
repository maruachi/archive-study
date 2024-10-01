*아래 글은 인프런 운영체제 공룡책 강의([운영체제 공룡책 강의](https://www.inflearn.com/course/lecture?courseSlug=%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-%EA%B3%B5%EB%A3%A1%EC%B1%85-%EC%A0%84%EA%B3%B5%EA%B0%95%EC%9D%98))를 보고 작성됐습니다.

# 컴퓨터와 프로그램이란?
- 컴퓨터는 정보를 처리하기 위한 도구이다.
- 컴퓨터의 기본 구조는 폰 노이만 구조이다.
  - Stored-program(내장형 프로그램) 방식. 램에서 프로그램(명령어의 집합)을 올려서 논리처리하도록 하는 방식.
  - SW에 따라 HW의 기능을 결정할 수 있는 Reversity를 가진다.
- 프로그램이란 명령어의 집합이다.
  
# 운영체제란 무엇인가?
- 운영체제란 컴퓨터에 항상 실행되며 프로세스, 자원, 유저 인터페이스(마우스, 키보드 등) 제어하기 위한 프로그램이다.
- 응용 프로그램에 시스템 서비스를 제공한다.

# 프로세스
- 하드 디스크에 있는 프로그램을 실행을 위해서 메모리 올라간 것을 프로세스라 불린다.

## 프로세스 메모리 레이아웃
  - text section: program의 코드들이 기계어로 저장된 영역
  - data section: global 변수, statci 변수가 저장된 영역
  - heap section: 런타임 동적 할당 영역
  - stack section: 함수의 local 변수, 파라미터, return address 저장 영역

![image](https://github.com/user-attachments/assets/6338c241-30a3-47fe-8551-eae997c0e0e7)

## 프로세스의 상태와 생명주기
- new: 프로세스가 생성
- running: instructions이 실행 중
- waiting: I/O 작업을 위해 대기와 같이 실행을 잠시 멈추는 단계
- ready: waiting이 끝나고 시작하기 전 준비 단계 (ready -> runing scheduler dispatch하여 runing 상태로 전환)
- terminated: 프로세스 실행 종료

![image](https://github.com/user-attachments/assets/cb2f2151-5976-4f87-a54d-439a77c5631a)

## PCB(Process Control Block) or TCB (Task Control Block)
- Process State
- Program Counter + CPU registers => context
- CPU-scheduling information
- Memoryu-management information
- Accounting information
- I/O status information

## 프로세스와 스레드
- 프로그램은 기본적으로 single thread of execution을 수행한다.
- 하지만 현대의 OS는 multiple threads of execution을 수행한다.
  - thread는 lightweight process이다.
- Scheduling Queue
  - ready queue: 실행 준비 중인 PCB
  - wait queue: 실행 대기 중인 PCB. 실행을 위해서 ready queue로 이동 필요
- Context Switch
  - 프로세스의 context를 저장해됬다가 복구하여 재실행하는 과정
- fork vs wait
  - 프로세스가 fork() 시점을 기준으로 자기 자신과 동일한 프로세스를 생성하는 행위
  - fork로 인해 프로세스의 tree 관계를 만들 수 있다.
  - fork() 시스템 콜 시에 자식 pid는 0을 부모 프로세스에서는 자식의 실제 pid 값으로 return 받는다. -> 이를 통해 부모와 자식을 구분 가능하다.
  - wait() 시스템 콜로 부모 프로세스를 대기 시켜 자식 프로세스에 대한 책임을 다하게 할 수 있다.
- 프로세스 종료
  - exit() 시스템 콜로 종료
  - 프로세스 종료시에 자원을 OS에게 반환
- zombie 프로세스: 부모 프로세스는 wait하지 않고 자식이 계속 남아서 돌고 있는 경우
- orphan 프로세스: 부모 프로세스가 종료됐지만 자식이 게속 남아서 돌고 있은 경우

## 프로세스 소통
- cooperating process의 소통은 inter-process communication (IPC)라고 불른다.
- IPC를 구현하는 방법는 공유 메모리와 메세지 패싱 두 가지가 존재한다.
- Producer-Consumer Problem
  - Producer-Consumer Problem은 소프트웨어 설계에서 자주 등장하는 패턴이다.
    - Producer는 작업을 생성하고 Consumer는 작업을 처리한다.
    - Producer, Consumer 패턴은 프로세스 소통이 필요한 전형적인 예이다.
- IPC 구현
  - shared memory -> 공유 buffer가 작업 대기열과 같은 역할을 한다.
    - 단점: shared memory를 application 단에서 관리해야 함
  - message passing -> send(), receive() system call로 작업대기열을 OS가 처리
    - direct vs indirect
      - direct: communication links -> send(P, message) receive(Q, message)
        - 하나의 링크만 가능
      - indirect: mailboxes(ports) -> send(message), receive(message)
        - 하나의 프로세스가 여러개의 링크 가능
        - OS에 필요한 API: new mailbox, send/receive through mailbox, delete mailbox
  - blocking send blocking receive vs non-blocking send non-blocking receive
    - send, receive 시 응답을 무한정 기달려야 하나 아니면 동시처리가 되냐 그 차이
