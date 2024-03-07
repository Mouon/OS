# Sheduling Queues
운영체제입장에서 각 프로세스의 추척및 관리가 용이해야함  
예를들어 CPU를 오래 점유하고있는 프로세스에게서 할당 중인 CPU를 가져와 대기중인 다른 프로세스에게 할당하고 싶을때  
Ready 상태의 프로세스를 찾는데 오래걸린다면 context swith의 오버헤드가 커지는 문제가 발생함  
따라서 각 상태별로 큐를 만들어 관리함 (운영체제 마다 다른데 리눅스는 red black tree로 관리)
## OS manages three types of queue
- Run queue (개념적으로만 큐지 하나만 들어갈 수 있음)
- Ready queue
- Waiting queue  
