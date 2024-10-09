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

## IPC 실 예시
- Shared Memory: POSIX Shared Memory
  - Portable Operating System Interface -> 운영체제 표준화하려는 시도
  - memory-mapped files: 메모리에 할당된 파일
- Message Passing: Pipes
  - pipe(), fork()와 같이 사용하여 구현
  - 프로세스를 연결하난 pipe를 통해서 read, write로 데이터 통신이 가능하다
- Sockets
  - IP address(컴퓨터), port number(프로세스)를 특정하여 다른 컴퓨터의 프로세스끼리 통신하는 방법
  - Socket class: connection-oriented (TCP)
  - DatagramSocket class: connectionless (UDP)
  - MulticastSocket class: multiple recipients
- RPCs(Remote Procedure Calls)
  - 원격 프로세스의 함수를 호출하는 기능
 
# 쓰레드와 Concurrency

## Thread
- a lightweight process
- a basic unit of cpu utilization
- 프로세스는 기본적으로 하나의 스레드 실행을 담당한다.
- 하나의 프로세스가 multi-process 실행 시에 thread 별로 메모리 할당이 필요하다.
- Thread ID 별로 메모리에 Program counter / Register set / Stack가 별도로 할당된다.
  - code, data, files 영역은 process 할당된 영역에 그대로 남아 있는다.

## 멀티 스레딩의 장점
- Responsiveness: 소켓 리스너 구현 예시처럼 반응성이 좋아진다.
- Resource Sharing: IPC와 달리 thread 끼리는 메모리를 공유하기에 shared-memory, message-passing이 용이하다
- Economy: process 생성보다 비용이 저렴하다
- Scalability: 확장 시에 multiprocessor 설계의 이점을 취할 수 있다

## Java에서 Thread 구현
- 구현 방법
  - Thread 상속
    - Thread class 상속 시 다른 객체의 상속을 주지 못하기 때문에 상속 기능을 제한하는 구현 방식이다.
  - Runable 인터페이스 구현을 통해서 Thread 객체 DI
  - Runable 타입으로 Lambda 표현식(익명 객체)을 Thread 객체에 DI
 
## Programming Challenges in Multicore systems
- Identifying tasks: 분리가능한 task 찾는 것에 어려움
- Balance: 각 task 별로 업무 부담을 분배의 어려움
- Data splitting: 데이터가 각 독립 코어에 분배의 어려움
- Data dependency: 각 코어 별로 데이터 의존성을 동기화 필요
- Testing and debugging: 테스트와 디버깅의 어려움. breakpoint가 특정 thread에서만 동작하기에 나머지 thread의 동작을 예측하기 어렵다

## Amdahl's Law
- 코어는 무조건 많을수록 좋은 것이 아니다.
- 예상하기로는 코어의 개수에 비례하여 성능이 빨라질 것으로 기대하지만, 실제로는 성능이 코어 개수 계속해서 증가하면 성능이 수렴하는 것을 보인다
- speedup <= 1/(S+(1-S)/N)
  - S: the portion that must be performed serially on a system
  - N: the number of processing cores

## User threads and Kernel threads
- User thread -> JVM에서 관리되는 thread (user space)
- Kernel thread -> OS kernel에서 하드웨어와 직접적으로 연관된 thread (kernel space)
- many-to-many, one-to-many, many-to-one

## Implicit Threading
- Four alternative approaches using implicit threading
  - implicit threading은 스레드의 생성과 관리의 책임을 개발자가 직접하지 않고 라이브러리나 외부로 넘기는 것
  - Thread Pools: Thread Pool에서 thread를 획득하여 사용하는 방식
  - Fork & Join
  - OpenMP: 특정 코드 블락을 병렬처리. 컴파일러에게 지시하여 병렬처리하도록 만든다. 
  - Grand Central Dispatch

# CPU scheduling

## CPU scheduling의 기본 개념
- 멀티프로그래밍 시에 여러개의 프로세스를 동시에 돌릴 수 있고 이를 통해 CPU 활용을 최대화 할 수 있다.
- I/O burst vs CPU burst
  - 일반적으로 I/O burst duration이 높은 프로그램(I/O burst bound)이 많기 때문에, cpu scheduling이 이점을 많이 챙길 수 있다.
![image](https://github.com/user-attachments/assets/58b980e9-65d0-4733-85a2-bc14aecca468)

## CPU scheduler
- 실행할 프로세스를 선택하는 기준
  - FIFO queue vs priority queue
- Non-preemptive scheduling vs Preemptive scheduling
  - Non-preemptive scheduling: CPU를 종료 또는 wait 상태로 변경될 때까지 프로세스가 cpu를 점령
  - Preemptive scheduling: 스케쥴러가 언제든지 프로세스를 언제든지 변경 가능
- Decision Making for CPU-scheduling
  - 1. running -> waiting state
  - 2. running -> ready state
  - 3. waiting -> ready state
  - 4. process terminates
  - 1 & 4 no chice - non-preemptive scheduling
  - 2 & 3 preemptive or non-preemptive 선택 가능
- Dispatcher
  - Context switching 시에 scheduler에게 어떤 process를 실행시킬지 알려주는 역할
  - Dispatcher latency: PCB 블록을 전환 시에 걸리는 delay 시간
  - `vmstat 1 3` 명령어 입력 시에 context switching이 얼마나 많이 일어나는 확인 가능
- CPU scheduling algorithms
  - CPU scheduling problems: ready 상태의 process 중 어떤 process를 먼저 실행시킬 것인가?
  - FCFS: 순서대로 처리
  - SJF: CPU-burst가 가장 짧은 process부터 처리
  - FCFS vs SJF
    - SJF 작업 시에 waiting time과 turnaround time을 줄일 수 있다.
