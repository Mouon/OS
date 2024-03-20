# Multicore Architecture

## SMP
<img width="425" alt="스크린샷 2024-03-20 오후 1 43 48" src="https://github.com/Mouon/OS/assets/137624597/63ad91cc-a540-43bb-9b04-744dca21e0a5">

메모리 공유

## NUMA
<img width="535" alt="스크린샷 2024-03-20 오후 1 43 04" src="https://github.com/Mouon/OS/assets/137624597/dcb196a1-26b6-42d0-bae5-10fc9423e7d2">

접근하는 메모리에 따라 성능이 다름  
OS는 가까운 쪽으로 메모리를 할당하려고 애씁니다.

## Single-queue multiprocessor scheduling (SQMS)
- 단일 프로세서 스케줄링의 기본 프레임워크를 재사용합니다.
- 실행할 최상의 n개 작업을 선택합니다 (n은 CPU 수).
- 동기화 오버헤드로 인한 확장성 부족
- 캐시 친화성

## Multi-Queue Scheduling
- CPU 당 코어 하나씩
- 작업이 시스템에 들어오면 정확히 하나의 스케줄링 큐에 배치됩니다.
- 무작위, 더 짧은 큐 등
- 그런 다음 기본적으로 독립적으로 스케줄링됩니다.
- 동기화를 피하고 캐시 친화성을 제공할 수 있습니다.
<img width="503" alt="스크린샷 2024-03-20 오후 1 54 41" src="https://github.com/Mouon/OS/assets/137624597/ae7814fb-786b-4117-bce6-eb59e0cf3cf4">

### Load imbalance
부하가 균등하게 나눠줄 필요성 대두
<img width="503" alt="스크린샷 2024-03-20 오후 1 56 24" src="https://github.com/Mouon/OS/assets/137624597/2d26fb19-9469-4d1a-a0c0-08e54466b111">

아래 그림처럼 비어있는 큐있으면 이동 시키는 작업이 필요하다.

- Work stealing : If the target queue is more full than the source queue, the source will “steal” one or more jobs from the target to help balance load
- 한가한놈이 작업을 훔쳐간다 (고맙게도?)
- CPU가 바껴서 캐시문제 생김 ( 장단점 존재해서 운영체제마다 다르다)
- 기본적으로는 너무 자주하지않는것이 원칙입니다.






