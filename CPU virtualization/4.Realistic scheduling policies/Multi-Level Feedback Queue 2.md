## Problems

- Starvation 라는 문제가 있을 수 있습니다 : interactive jobs들이 많다면..

## Priority Boost
#### After some time period S, move all the jobs in the system to the topmost queue
CPU를 할당받지 못한채로 너무 오랜시간 방치되면 우선순위를 높여줍니다.

<img width="601" alt="스크린샷 2024-03-18 오후 2 12 50" src="https://github.com/Mouon/OS/assets/137624597/8280a4e9-32ad-49b8-8134-afee6260e2dd">

오랜시간동안 기아상태를 유지합니다. 이럴때 Priority boost를 적용합니다.

<img width="601" alt="스크린샷 2024-03-18 오후 2 13 28" src="https://github.com/Mouon/OS/assets/137624597/1cc6573e-fc58-4489-a622-0ee4bfa93ce0">

위와같이 주기적으로 우선순위를 올려줍니다.

## Gaming the scheduler
#### Issuing an I/O operation just before the time slice is over
- 누적해서 타임슬라이스 만큼 채웠으면 우선순위를 낮춥니다.
- 누적해서 봐도 타임슬라이스 만큼 차지하지않았다면 우선순위를 유지합니다.

#### Gaming the scheduler
<img width="601" alt="스크린샷 2024-03-18 오후 2 18 39" src="https://github.com/Mouon/OS/assets/137624597/04dc1359-c975-410c-8ce5-2d66f6e6bac6">

10 쯤에 일부러 I/O를 줘서 CPU를 차지하는 장난을 치는데 이러면 누렁이가 CPU 할당을 못받음
하지만 `누적해서 타임슬라이스 만큼 채웠으면 우선순위를 낮춥니다.` 라는 규칙을 적용하면 누적값을 기준을 하기때문에
<img width="601" alt="스크린샷 2024-03-18 오후 2 21 31" src="https://github.com/Mouon/OS/assets/137624597/4cde6ed7-fa6c-4207-beb6-95269f2aa79d">
위와 같이 우선순위가 낮아집니다.

## Changing behavior
만약에 CPU-bound -> I/O-bound 로 옮겨지면 어떻게 되는문제는 어떻게 해결되는가?

Priority boost를 주는 값을 적절하게 주어 해결한다.  
(Default value in Solaris: 1 s)  
하지만 정답이 있는 문제는 아닙니다.


타임슬라이스를 정하는 것도 어려운 문제입니다.
솔라리스는 타임슬라이스값을 우선순위에 따라 다르게 제공합니다.
우선 순위가 높은 프로세스는 타임슬라이스가 작은것이좋지만 (Default value in Solaris: 20 ms)
낮은 우선순위의 경우에는 긴 타임슬라이스를 갖는 것이 좋습니다. (Default value in Solaris: few hundred ms)




