
# Scheduling Metrics
### Turnaround time
프로세스 완료시간에서 도착시간을 뺀다.
### Response time
interactive(사용자랑 interactive 하는 범주에서 좋은 대접해주려 노력함)
프로세스가 처음 시작되는 시점부터 도착시간을 뺀다.
- RR reduces response time but is terrible for turnaround time
- 라운드 로빈은 response time 줄이지만 turnaround time은 증가시킨다.

## 어떻게 하면 response time과 turnaround time를 줄여줄 수 있는 방법은 무엇인가


# Multi-Level Feedback Queue
## CPU 코어마다 여러개의 큐가 한 세트씩 존재합니다.

## Basic rules
- Rule 1: If Priority(A) > Priority(B), A runs (B doesn’t) : Priority가 높은놈이 실행
- Rule 2: If Priority(A) = Priority(B), A & B run in RR : 같은면 라운드 로빈

#### MLFQ Example (Ready Queue)
숫자 높은게 priority가 높다.

<img width="397" alt="스크린샷 2024-03-18 오후 1 53 18" src="https://github.com/Mouon/OS/assets/137624597/f77f324f-7ffb-4886-b42f-25bc0a0fa486">

처음에는 A,B가 라운드 로빈으로 실행되다가 C,D가 순차적으로 실행됩니다.

## Priorities를 어떻게 정할까?
#### 응용프로그램이아닌 OS가 정할 것이다.

- I/O를 요청하느라 blocked 상태로 자주간다면 priority를 높인다. (I/O 빈도수높으면 우선순위 높다)
- CPU를 많이 차지하면 우선순위를 낮춘다.

#### Workload 를 구분합니다.
- I/O-bound jobs : Interactive and short-running
- CPU-bound jobs : Compute intensive and longer-running

#### Priority adjustment algorithm
- 시스템에 들어오면 가장 높은 우선수위를 할당합니다.
- 특정 time slice 다쓰면 우선순위를 낮춥니다.
- 만약에 할당후에 특정한 하나의 타임슬라이스 내부에서 반납한다면 그대로 우선순위 상태를 유지합니다.

### A single long-running job
<img width="581" alt="스크린샷 2024-03-18 오후 2 01 13" src="https://github.com/Mouon/OS/assets/137624597/6ce00494-d2ee-46ac-b928-ff581431e763">

### Along came a short job
<img width="581" alt="스크린샷 2024-03-18 오후 2 01 28" src="https://github.com/Mouon/OS/assets/137624597/0239b298-17e7-4d19-ae54-827874523e73">

파랑 프로세스가 200 시점에 들어와서 타임슬라이스를 다 사용했으므로 Q1 에서 Q2로 우선순위가 낮아집니다.
파랑프로세스이 Q0에서 라운드 로빈으로 실행되지않는 이유는 종료되었기때문입니다.

<img width="581" alt="스크린샷 2024-03-18 오후 2 08 08" src="https://github.com/Mouon/OS/assets/137624597/428cd9ad-7e59-4627-9259-fbfd59791c64">

I/O때문에 반납하므로 우선순위 유지



