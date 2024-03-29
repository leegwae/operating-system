# Process and Thread

## 프로세스

**프로세스(process)**는 RAM에 적재되어 실행 중인 프로그램을 의미하며 CPU의 작업 단위이다. 운영체제로부터 시스템 자원(CPU 시간, 주소 공간, 메모리 등)을 할당받는 주체이다.

## 프로세스 주소 공간

<img src="https://user-images.githubusercontent.com/57662010/209113874-1fef1053-383d-4403-9005-d9e994d550ad.png" alt="process address space" style="zoom: 67%;" />

**주소 공간(process address space)**은 프로세스가 사용할 수 있는 주소의 범위를 정의한 것으로 텍스트 세그먼트(text segment), 데이터 세그먼트(data segment), 스택 세그먼트(stack segment)로 나뉜다.

텍스트 세그먼트는 실행 가능한 코드나 상수, 변수와 같은 읽기 전용 데이터를 저장한다. 데이터 세그먼트는 전역 변수를 저장하며, 초기화되지 않은 전역 변수를 저장하는 GVAR 섹션과 초기화된 전역 변수를 저장하는 BSS 섹션으로 나뉜다. 스택 세그먼트는 함수와 관련한 데이터-매개변수, 리턴 주소, 지역 변수를 저장한다. 이들의 크기는 모두 컴파일에 결정된다. 스택과 데이터 사이의 빈 공간은 힙(heap)이라고 하며 동적 메모리를 할당할 때 사용한다. 힙의 크기는 런타임에 결정된다.

## 멀티 프로세스

다중 프로그래밍에서 하나의 CPU는 아주 짧은 주기로 프로세스를 번갈아 실행한다. 이때 다음에 실행할 프로세스를 선택하는 작업을 스케줄링이라고 하며, 실행 중인 프로세스를 선택된 프로세스로 바꾸는 작업을 문맥 교환이라고 한다.

### 문맥 교환

<img src="https://user-images.githubusercontent.com/57662010/209119490-2823a4e7-bcc2-4e91-b4dd-33fb9a41cbcb.png" alt="image" style="zoom:67%;" />

운영체제는 프로세스를 관리하기 위하여 각 프로세스를 실행하는데 필요한 정보를 담은 **PCB(process control block; 프로세스 제어 블록)**를 메인 메모리의 커널 영역에 유지한다.

**문맥 교환(context switching)** 혹은 **프로세스 교환(process switching)**은 실행하던 프로세스의 정보를 PCB에 저장하고 실행하려는 프로세스의 정보를 PCB에서 읽어 레지스터에 적재하는 것이다. 캐시를 플러시, 재적재하고 메모리 사상을 교체하는 등의 작업을 수행하므로 오버헤드가 발생한다.

### CPU-바운드 프로세스와 I/O-바운드 프로세스

버스트는 일정 시간 동안 계산이나 I/O를 수행하는 행위를 말한다. 전자를 CPU 버스트, 후자를 I/O 버스트라고 한다.

CPU-바운드 프로세스(computed-bound process)는 긴 CPU 버스트를 가지며 드물게 I/O를 기다리는 프로세스이다. I/O-바운드 프로세스는 짧은 CPU 버스트를 가지며 빈번하게 I/O를 기다리는 프로세스이다. 디스크에 비해 CPU가 빨라진 현대에서 프로세스는 대부분 I/O-바운드 프로세스로, CPU를 효율적으로 사용하기 위해서 멀티 프로그래밍을 적용하는 것이 적합하다. I/O-바운드 프로세스의 우선순위를 더 높게 주어 이들이 I/O를 기다리는 동안 CPU-바운드 프로세스를 실행하면 된다.

### 프로세스 상태

<img src="https://user-images.githubusercontent.com/57662010/209124816-fae17edf-882e-43e0-8dd9-a882840f4e62.jpg" alt="Process-state" style="zoom:67%;" />

- new: 프로세스를 생성 중이다.
- ready: 프로세스가 swap in되어 실행 가능하며, 실행되기를 기다린다.
- running: 프로세스가 현재 실행 중이다. 스케줄러에 의해 ready 상태의 프로세스가 선택되어 실행된다.
- waiting(blocked): 프로세스가 특정 이벤트가 발생하기 전까지 실행할 수 없다. 특정 이벤트가 발생하기를 기다린다. 예를 들어 프로세스는 사용자의 입력을 기다리거나 디스크에서 파일을 읽어오는 동안 멈출 수 있다.
- terminated: 프로세스가 모든 자원을 반납하고 종료되었다.
- suspended: 프로세스가 swap out되어 수행이 정지되었다.

### 멀티 프로세스의 장점과 단점

각각의 프로세스는 독립적으로 자원을 할당받으므로 하나의 프로세스가 종료되어도 다른 프로세스에게 영향을 끼치지 않는다는 장점이 있다. 그러나 각각 자원을 할당해야한다는 점에서 자원 소모가 크며, 문맥 교환이 잦다면 오버헤드가 발생한다. 또한 프로세스는 서로의 자원에 접근하기 위하여 복잡하고 어려운 **IPC(inter-process communication; 프로세스 간 통신)** 기법을 사용해야 한다.

## 프로세스 스케줄링

다중 프로그래밍 환경에서 CPU를 효율적으로 사용하기 위해 스케줄링이 필요하다.

### 스케줄링 큐의 종류

각각의 프로세스(PCB)는 상태에 따라 큐에 입력된다. Job Queue는 시스템에 존재하는 모든 프로세스를 저장한다. Ready Queue는 메모리 내에 적재되어 실행되기를 기다리는 프로세스를 저장한다. Device Queue는 I/O 장치의 입출력을 기다리는 프로세스를 저장한다.

### 장기, 중기, 단기 스케줄러

**Long-term scheduler(job scheduler)**는 어떤 프로세스를 ready queue로 보낼지 결정한다. **Short-term scheduler(CPU scheduler)**는 어떤 프로세스를 다음에 실행할지 결정한다. **Medium-term scheduler(swapper)**는 메모리에 적재된 프로세스를 조절하여 여유 공간을 확보한다. swapper는 메모리에 적재된 프로세스의 수행 이미지를 디스크의 swap 영역에 기록(swap out)하거나 swap 영역으로부터 읽어들여 메모리에 적재(swap in)한다.

### 선점, 비선점 스케줄링 알고리즘

**선점형(preemptive) 스케줄링 알고리즘**의 경우 운영체제는 현재 실행 중인 프로세스의 CPU 자원을 빼앗아 다른 프로세스에게 할당할 수 있다. 문맥 교환으로 인한 오버헤드가 발생하거나, 우선순위가 높은 프로세스에게 CPU 자원이 계속 할당되어 어떤 프로세스가 기아 상태에 빠질 수 있다. 응답 시간이 빠르고 CPU가 효율적으로 사용된다는 장점이 있다. 대화형 시스템에 적합하다.

**비선점(nonpreemptive) 스케줄링 알고리즘**의 경우 현재 실행 중인 프로세스는 waiting이나 terminated 상태가 되기 전까지 CPU 자원을 독점한다. 버스트 시간이 긴 프로세스에게 CPU 자원이 할당되면 버스트 시간이 짧은 프로세스는 기아 상태에 빠질 수 있다. 또한 응답 시간이 느리며, CPU가 효율적으로 사용되지 못한다는 단점이 있다. 배치 시스템에 적합하다.

### 배치, 대화식, 실시간 시스템

**배치 시스템**은 프로그램을 요청 순서에 따라 순차적으로 처리한다. 빠른 응답을 필요로 하지 않으므로 비선점이나 주기가 긴 선점형 알고리즘이 적절하다. 문맥교환을 줄여 성능을 향상시키는 기법이다. **대화식 시스템**은 여러 개의 프로세스를 번갈아 실행한다. 다수의 사용자에게 빠르게 응답하는 것이 중요하므로 한 프로세스가 CPU를 독점하지 않도록 선점형 알고리즘을 사용한다. **실시간 시스템**은 데드라인 안에 응답하는 것이 중요하다.

### 비선점 스케줄링

#### FCFS

**FCFS(First-come First-served)**에서는 프로세스들이 요청한 순서대로 큐에 삽입되고 CPU를 할당받는다. 수행 시간이 짧은 프로세스가 뒤로 갈수록 평균 반환 시간(대기 시간 + 실행시간)이 증가한다. 이를 해결하기 위해 SJF를 사용할 수 있다.

#### SJF

**SJF(Shortest Job First)**에서는 수행 시간이 짧은 프로세스가 먼저 실행된다. 평균 대기 시간이 가장 짧지만, 프로세스의 수행 시간은 미리 알기 어렵다. 또한 수행 시간이 긴 프로세스의 수행이 계속 밀리는 단점이 있다.

#### HRRN

**HRRN(Highest Response Ratio Next)**에서는 response ratio가 높은 순서대로 프로세스를 실행하여 기아 문제를 해결한다.

```
response ratio = (대기 시간 + 수행 시간) / (수행 시간)
```

#### Priority Scheduling

**우선순위 스케줄링(Priority Scheduling)**에서는 우선순위가 높은 프로세스부터 실행된다. 우선순위가 낮은 프로세스가 기아 상태에 빠질 수 있다.

### 선점 스케줄링

#### SRTN

**SRTN(Shrotest Remaning Time Next)**는 SJF의 선점 구현으로 새로운 프로세스가 현재 실행 중인 프로세스보다 수행 시간이 짧다면 먼저 실행된다. 수행 시간이 긴 프로세스가 기아 상태에 빠질 수 있다.

#### Round-Robin Scheduling

**라운드 로빈 스케줄링(Round-Robin Scheduling)**에서는 프로세스는 동일한 할당 시간(time quantum)만큼 번갈아가며 실행한다. 할당 시간을 모두 사용하면 프로세스는 ready 상태가 된다. 할당 시간을 크게 하면 뒤쪽 순서의 프로세스가 대기하는 시간이 늘어나며, 작게 하면 응답은 빠르지만 문맥 교환으로 인한 오버헤드가 증가한다.

#### Multi Level Queue Scheduling

**다단계 큐(Multi Level Queue Scheduling)**는 우선순위가 같은 프로세스를 하나의 큐에 넣고 각 큐는 라운드 로빈이나 FCFS 등 독자적인 스케줄링을 가진다. 우선순위가 높은 큐의 할당 시간은 짧게, 우선순위가 낮은 큐의 할당 시간은 길게 할당한다. 더 높은 우선순위의 큐가 비면 낮은 우선순위의 큐를 작업하므로 기아 문제가 발생할 수 있다.

#### Multi Level Feedback Queue Scheduling

**다단계 피드백 큐(Multi Level Feedback Queue Scheduling)**는 다단계 큐를 사용하는데 I/O 바운드 프로세스에게 더 높은 우선순위를 부여한다. 할당 시간을 모두 소요한 프로세스는 한 단계 낮은 우선순위를 부여받게 된다. 기다린 시간에 따라 우선순위가 높아지는 에이징 기법을 적용하여 기아 문제를 예방할 수 있다. 응답 시간이 빠르고 반환 시간이 짧다.

---

## 스레드

**스레드(thread)**는 프로세스 내에 있는 여러 개의 실행의 흐름(thread of execution)을 일컫는 단위이다. 프로세스는 한 개 이상의 스레드를 가지게 된다.

<img src="https://user-images.githubusercontent.com/57662010/209548063-9f1b8fdc-20a2-47c1-8fc1-1d3acf9e337d.png" alt="image" style="zoom: 50%;" />

같은 프로세스 내에 있는 스레드들은 프로세스가 할당받은 자원의 일부를 공유한다. 그러나 각각의 스레드는 독립적으로 함수를 호출하므로 스택, PC, 레지스터, 상태 등의 정보를 독립적으로 보유한다. 운영체제는 프로세스처럼 스레드를 실행하기 위해 필요한 정보를 담은 **스레드 제어 블록(thread control block; TCB)**를 커널 영역에 유지한다.

## 멀티 스레드

**멀티 스레드(multi thread)**는 하나의 프로세스가 여러 개의 스레드로 구성되어, 하나의 스레드가 하나의 작업을 처리하는 것을 말한다. 스레드의 문맥교환을 위하여 **스레드 테이블(thread table)**을 유지해야한다.

### 사용자 레벨 스레드

**사용자 레벨 스레드(user-level thread)**는 스레드 패키지를 사용자 공간에 구현한 것이다. 커널은 스레드에 대해 알지 못하여 단일 스레드 프로세스를 수행하고 있다고 생각한다. 프로세스는 스레드를 관리하기 위해 각각 스레드 테이블을 유지한다.

스레드를 지원하지 않는 운영체제에서 구현할 수 있고, 문맥 교환시 시스템 콜을 하지 않아 빠르다. 각각의 프로세스는 적절한 스케줄링 알고리즘을 적용할 수도 있다. 그러나 스레드 하나가 블로킹 시스템 콜을 하면 프로세스가 블록되어 결과적으로 모든 스레드가 블록되는 문제가 있다. 또한 스레드 하나에서 페이지 폴트가 발생하면 해당 페이지를 적재할 때까지 프로세스가 블록된다. 프로세스 내에 클록 인터럽트 같은 것이 없어 라운드 로빈 스케줄링이 불가능하다.

### 커널 레벨 스레드

**커널 레벨 스레드(kernel-level thread)**는 스레드 패키지를 커널 내부에 구현한 것이다. 커널이 프로세스 테이블과 스레드 테이블을 가지고 스레드 단위로 스케줄링한다. 스레드 연산이 시스템 콜로 제공되므로 스레드 연산이 빈번하다면 오버헤드가 증가한다.

### 멀티 스레드의 장점과 단점

스레드는 자원을 공유하므로 자원 소모가 적으며 데이터 영역 혹은 힙 영역을 사용하여 데이터를 주고받을 수 있어 스레드 간의 통신이 간단하다. 스레드의 문맥 교환은 캐시를 초기화하지 않아도 되므로 비용이 작다. 프로세스에 비교하여 생성과 삭제가 쉽고 빠르다.

그러나 하나의 스레드에 이상이 있으면 프로세스 자체가 죽어버릴 수도 있다(다른 스레드 모두 종료될 수 있다). 또한 자원을 공유하므로 **임계 영역(critical section)**에 대해 **동기화 작업**을 필요로 한다.

### thread-safe

**스레드 안전(thread-safe)**은 두 개 이상의 스레드가 공유 자원에 동시에 접근해도 프로그램 실행에 문제가 생기지 않는 것이다. thread-safe는 다음 방법으로 구현한다. 첫번째, 세마포어 등 락을 사용하여 공유 자원에는 한 번에 하나의 스레드만 접근할 수 있도록 한다. 두번째, 공유 자원에는 원자적 연산을 사용한다. 세번째, 자원을 최대한 공유하지 않도록 한다. 네번째, 스레드가 호출하는 함수가 다른 스레드에게 영향을 끼치지 않도록 한다.

### 스레드 풀

![image](https://user-images.githubusercontent.com/57662010/209802323-a71b0923-c418-41b2-8c1d-fecd5c04f666.png)

**스레드 풀(thread pool)**은 미리 여러 개의 스레드를 만들어놓고 작업 큐에 들어오는 작업을 하나의 스레드가 하나씩 맡아 처리하는 방식이다. 매 작업마다 새로운 스레드를 생성하고 삭제한다면 작업 처리 요청이 폭증할 때 오버헤드가 증가한다. 스레드의 전체 개수를 제한하여 이것을 막는다.

---

## 스터디

### 프로세스란 무엇인가

RAM에 적재되어 실행 중인 프로그램으로 CPU로부터 자원을 할당받는다.

### 프로세스는 RAM에 어떤 구조로 적재되어있는지 말해보세요(프로세스 주소 공간에 대해 설명해주세요)

프로세스는 RAM에 코드 세그먼트, 데이터 세그먼트, 스택 세그먼트로 나뉘어 저장된다. 코드 세그먼트는 실행 가능한 코드와 읽기 전용 상수들을 저장한다. 데이터 세그먼트는 전역 변수를 저장한다. 스택은 함수와 관련한 데이터를 저장한다. 이때 데이터 세그먼트와 스택 사이에 빈 공간(힙)이 있는데 동적 메모리를 할당할 때 사용한다.

### 멀티 프로세스란 무엇인가

하나의 CPU가 여러 개의 프로세스를 번갈아가면서 실행하는 것이다.

### 왜 멀티 프로세스를 사용하는가?

사용자에게 여러 개의 프로세스가 블로킹되지 않고 실행되는 것처럼 보이기 위해서이다. 또한 CPU를 효율적으로 사용하기 위해서이다.

### CPU를 효율적으로 사용하기에 멀티 프로세스가 왜 좋은가?

오늘날 CPU는 디스크에 비해 속도가 매우 빠르며 프로세스는 대부분 I/O를 기다리는 I/O 바운드 프로세스이다. 블록된 동안 CPU 작업을 필요로 하는 다른 프로세스를 실행한다면 CPU 자원을 낭비하지 않을 수 있다.

### 문맥 교환에 대해 설명해주세요

문맥 교환은 실행 중인 프로세스의 상태를 PCB에 적재하고, 실행하려는 프로세스의 정보를 PCB에서 읽어 레지스트리에 적재하는 것이다.

### PCB에 대해 설명해주세요

PCB(Process Control Block)은 프로그램을 실행하기 위해 필요한 정보를 저장한다. 프로세스의 id, PC, 상태 비트, 레지스터 등을 가진다.

### 프로세스 상태에 대해 설명해주세요

프로세스가 메모리에 적재되어 실행을 대기하는 `ready` 상태. CPU 자원을 할당받아 실행 중인 `running` 상태. 이벤트(예: I/O)의 발생을 대기 중인 `waiting` 상태. 특정 이유로(예: 메모리 부족) 디스크의 swap 영역으로 swap out된 `suspended` 상태. 모든 자원을 반납하고 실행이 종료된 `terminated` 상태.

### 선점 스케줄링과 비선점 스케줄링의 차이는 무엇인가?

선점 스케줄링에서는 프로세스가 도중에 CPU 자원을 다른 프로세스에게 할당 가능하다. 비선점 스케줄링에서는 프로세스는 종료 혹은 대기 상태가 되기 전까지 CPU 자원을 독점한다.

### 선점 스케줄링을 사용하기에 적합한 환경의 예시를 들어주세요

다수의 사용자에게 빠르게 응답하는 것이 중요할 때 사용하기 좋다. 하나의 프로세스가 CPU를 독점하면 안되므로 뺏어야된다.

### 비선점 스케줄링의 종류를 설명해주세요

FCFS(First-come First-served), SJF(Shortest Job First), HRRN, 우선순위 스케줄링.

### 선점 스케줄링의 종류를 설명해주세요

SRTN(Shrotest Remaning Time Next), RR(Round Robin), 다단계 큐, 다단계 피드백 큐

### 잘 알고 있는 스케줄링 중 하나를 설명해주세요

라운드 로빈 스케줄링은 선점 스케줄링으로, 프로세스는 동일한 time quantum만큼 번갈아가며 실행한다. 할당 시간을 크게 주면 뒤쪽 순서의 프로세스의 대기 시간이 증가하고 작게 주면 문맥 교환이 잦을 수 있다.

### 멀티 스레드가 무엇인가요

하나의 프로세스가 여러 개의 독립적인 실행 흐름인 스레드로 구성되는 것이다. 이때 운영체제는 프로세스가 아니라 스레드 단위로 스케줄링한다.

### 멀티 프로세스가 있는데 왜 멀티 스레드를 사용하나요

하나의 프로세스 내에서 스레드는 프로세스 자원을 일부 공유한다. 따라서 프로세스보다 자원을 덜 소모하며 스레드 간 통신도 쉽고 문맥 교환 비용도 작다.

### 멀티 스레드의 단점이 있나요?

첫번째, 커널 영역을 공유하고 있으므로 하나의 스레드에 문제가 생기면 다른 스레드에도 문제가 생길 수 있다. 두번째, 자원을 공유하므로 임계구역에 대한 동기화가 필요하다.

### thread-safe에 대해 알고 있나요?

여러 개의 스레드가 공유 자원에 동시에 접근해도 프로그램 실행에 문제가 생기지 않는 것이다.

### thread-safe는 어떻게 구현할 수 있을까요?

공유 자원에 접근할 때 세마포어 등의 락을 사용해 상호 배제를 달성할 수 있다.

### 프로세스와 스레드의 차이는 무엇인가

첫번째, 각각의 프로세스는 독립적으로 자원을 할당받지만 스레드는 스택을 제외하면 프로세스가 할당받은 자원을 공유한다. 두번째, 프로세스는 자원을 공유하기 위해 IPC를 사용하지만 스택은 데이터나 힙 영역으로 데이터를 주고받을 수 있다. 세번째, 스레드가 프로세스의 경량 버전이므로 생성과 삭제가 쉽고 싸다.

### 멀티 스레드를 사용하기 적합하거나 적합하지 않은 환경은?

프로세스의 경량 버전이므로 생성과 삭제가 쉬워 스레드 개수가 동적으로 빠르게 변하는 경우 적절하다.

모든 스레드가 CPU 바운드인 경우는 적합하지 않다.

### 웹 서버에서 멀티 스레드 사용해보자

dispatcher 스레드가 네트워크 요청을 받는다. disaptcher가 대기 상태의 worker 스레드에게 요청을 전달한다. 깨어난 worker 스레드는 캐시와 디스크에서 페이지를 찾아 클라이언트에게 전달하고 새로운 요청을 대기하며 잠든다.
