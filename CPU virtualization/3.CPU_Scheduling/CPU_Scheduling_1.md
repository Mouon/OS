# Workload Assumptions
- 모든 작업(프로세스)은 같은 시간동안 작동한다.
- 모든 프로세스들은 동시에 실행이 되었다.
- 프로세스 실행되면 종료시까지 작동한다.
- 모든 프로세스들은 CPU만 사용한다. (io 안함, 즉 blocked 고려안할거임)
- 우리는 모든 프로세의 reun-time을 알고있다.

(ps. 말도안되는거 알아요)

# First In, First Out (FIFO)  

<img width="638" alt="스크린샷 2024-03-13 오후 1 54 31" src="https://github.com/Mouon/OS/assets/137624597/044a689f-8864-4dcc-a1cd-251c75990933">

요청순서대로..

- 누렁이는 처음 출발해서 10에 끝
- 파랭이는 10 실행 Ready는 10
- 초록이는 10 실행 Ready는 20
#### Average turnaround time = 20

<img width="536" alt="스크린샷 2024-03-13 오후 1 57 25" src="https://github.com/Mouon/OS/assets/137624597/5ff5694d-3dd7-4763-8a47-e4a7cb69031f">

#### Average turnaround time = 110
파랑이 초록이는 짧게 실행하는데 대기시간 걸려서 turnaround time이 길어짐 `Convoy effect`

## 해결책 제안
`짧은 순서로 실행시키겠다.`

<img width="536" alt="스크린샷 2024-03-13 오후 1 58 27" src="https://github.com/Mouon/OS/assets/137624597/3fcc5e2c-68fb-4d08-a8e5-84350c78ac21">

#### Average turnaround time = 50 
많이 개선된 모습


<img width="525" alt="스크린샷 2024-03-13 오후 2 01 37" src="https://github.com/Mouon/OS/assets/137624597/deec0761-3ace-4805-b1be-5de3ca2f94e7">

## B,C 가 나중에 도착했는데 선점을 못해서... 괜히 또 길어짐 

