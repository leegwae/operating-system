# Interrupt and System Call

## 인터럽트

- **인터럽트(interrupt)**: 예외상황이 발생했을 때 이것을 CPU에게 알려 하던 것을 멈추고 예외를 처리하도록 하는 것



## 인터럽트의 종류

인터럽트는 인터럽트를 발생하는 주체를 기준으로 **하드웨어 인터럽트**와 **소프트웨어 인터럽트**로 나눌 수 있다.



### 하드웨어 인터럽트

- **하드웨어 인터럽트(hardware interrupt)**는 I/O 디바이스, 타이머 등 하드웨어에 의해 발생하는 인터럽트이다.
- <u>일반적으로 인터럽트는 하드웨어 인터럽트를 가리킨다.</u>
- **비동기식 인터럽트(asynchronous Interrupt)**라고도 한다. CPU는 하드웨어 인터럽트가 도착하기 전까지 다른 일을 하기 때문이다.



### 소프트웨어 인터럽트

- **소프트웨어 인터럽트(software interrupt)**는 소프트웨어에 의해 발생하는 인터럽트이다.
- **동기식 인터럽트(synchronous Interrupt)**라고도 한다.
- **트랩(trap)**은 소프트웨어 인터럽트의 종류로, 사용자 프로그램은 시스템 서비스를 실행하기 위해 트랩 명령을 실행한다(시스템 호출한다). CPU는 트랩 명령에 커널 모드로 전환하여 시스템 호출을 처리한다.



## 인터럽트의 발생과 처리 과정

### 인터럽트의 발생과 처리 과정

하드웨어가 인터럽트 `A`를 발생시키면, 사용자 모드에 있는 CPU는 현재 상태를 저장하고 커널 모드로 전환한다. **인터럽트 벡터 테이블(interrupt vector table)**에서 인터럽트 `A`를 처리하는 **인터럽트 핸들러(interrupt handler)**를 찾아 실행하여 인터럽트를 처리한다. 인터럽트 핸들러는 **인터럽트 서비스 루틴(Interrupt Service Routine; ISR)**이라고도 한다. 



### 시스템 호출의 발생과 처리 과정

사용자 프로그램이 `A` 시스템 호출을 하면, 사용자 모드에 있는 CPU는 현재 상태를 저장하고 커널 모드로 전환한다. 인터럽트 벡터 테이블에서 `A` 시스템 호출을 처리하는 인터럽트 핸들러를 찾아 실행한다. 이때 핸들러를 **시스템 호출 핸들러(system call handler)** 혹은 시스템 호출 서비스 루틴(system call service routine)이라고도 한다. 핸들러는 시스템 호출 테이블(system call table)에서 `A` 시스템 호출이 구현된 함수를 찾아 `A` 시스템 호출을 실행한다.



## 참고

[chowisely - Interrupt & System Call](https://velog.io/@chowisely/Operating-Systems-Interrupt-System-Call)

[joona - Interrupt](https://jooona.tistory.com/3)
