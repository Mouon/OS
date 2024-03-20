# Multiprocessor Scheduling




## Load metric
Load balancing은 최대한 루마 한 노드안에서 하려하고  
migration은 자제하려고 합니다.

#### /kernel/sched/sched.h
```
struct sched_class {
void (*enqueue_task) (struct rq *rq,
struct task_struct *p,
int flags); void (*dequeue_task) (struct rq *rq,
                             struct task_struct *p,
                             int flags);
...
struct task_struct *(*pick_next_task)(struct rq *rq); ...
int (*balance)(struct rq *rq, struct task_struct *prev,
... }
struct rq_flags *rf);
```

core 마다 큐(세트)가 존재합니다.  
`pick_next_task` 다음번에 실행할 프로세스 지정


```
extern const struct sched_class dl_sched_class;
extern const struct sched_class rt_sched_class;
extern const struct sched_class fair_sched_class;
extern const struct sched_class idle_sched_class;
```


`extern const struct sched_class idle_sched_class;` 돌릴 프로세스 없으면 없으면 idle이 실행됨


