# operating-system

내가 공부한 운영체제

(2022/06/02~2022/07/04)

(2022/09/28~)



- [운영체제란 무엇인가](https://github.com/leegwae/operating-system/blob/main/Operating%20System%20Basics.md)
  - 소프트웨어로서의 운영체제
  - CPU의 이중 모드
- [인터럽트와 시스템 호출](https://github.com/leegwae/operating-system/blob/main/Interrupt%20and%20System%20Call.md)
  - 인터럽트의 정의
  - 인터럽트의 종류
  - 인터럽트의 발생과 처리 과정
- [프로세스와 스레드](https://github.com/leegwae/operating-system/blob/main/Process%20and%20Thread.md)
  - 프로세스와 스레드의 정의
  - 프로세스와 스레드의 차이
  - 문맥 교환의 정의
  - 멀티 프로세스와 멀티 스레드의 정의와 장단점
  - 멀티 스레드를 사용하는 이유
- [프로세스 간 통신(IPC)](https://github.com/leegwae/operating-system/blob/main/IPC.md)
  - 공유 메모리 방식
  - 메시지 패싱 방식
- [프로세스 동기화 문제(수정중)](https://github.com/leegwae/operating-system/blob/main/Process%20Synchronization.md)
  - 경쟁 상태와 임계구역
  - 임계구역 문제를 해결하는 세 가지 조건
  - 상호 배제 구현하기
  - 생산자-소비자 문제
- [메모리 관리](https://github.com/leegwae/operating-system/blob/main/Memory%20Basics.md)
  - 메모리 계층 구조
    - 메인 메모리
    - 캐시 메모리

  - 메모리 추상화: 주소 공간
  - MMU와 메모리 관리
  - 멀티 프로그래밍에서 메모리 관리
- [메모리 분할](https://github.com/leegwae/operating-system/blob/main/Memory%20Partitioning.md)
  - 단편화
  - 메모리 분할 방식
- [가상 메모리](https://github.com/leegwae/operating-system/blob/main/Virtual%20Memory.md)
  - 페이징
  - 페이지 폴트와 페이지 교체 알고리즘
  - 세그멘테이션
  - 세그멘테이션-페이징




## TODO

- [ ] **스터디 내용으로 갱신하기**
- [x] 인터럽트
- [x] 시스템 콜
- [x] 프로세스와 스레드
- [ ] 스케줄링
- [ ] IPC: 프로세스 간 데이터 교환을 목적으로 함
  - [x] 공유 메모리 - 동기화 문제
  - [x] 메세지 패싱
  - [ ] 메시지 패싱 종류(파이프, 네임드 파이프FIFO, 메시지 큐, 소켓)
- [x] 동기화 문제: 프로세스 간 데이터를 동기화, 보호를 목적으로 함
  - [x] 경쟁 상황
  - [x] 임계 구역과 해결 조건
  - [x] 해결책
    - [x] 뮤텍스
    - [x] 세마포어
    - [x] 모니터




## 참고

- 운영체제 강의 필기
- 운영체제론(노상혁, 이동희, 천홍석, 최동우 공역)
- [Technical Interview Guidelines for Beginners](https://github.com/JaeYeopHan/Interview_Question_for_Beginner)
- [Ready For Tech Interview](https://github.com/WooVictory/Ready-For-Tech-Interview)
- [Tech Interview For Developers](https://github.com/gyoogle/tech-interview-for-developer)
- [tech interview](https://github.com/WeareSoft/tech-interview)
- [geeksforgeeks - operating system](https://www.geeksforgeeks.org/operating-systems/?ref=lbp)

