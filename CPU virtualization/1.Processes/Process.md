# 프로세스

### 프로세스란
`프로그램과 구분되는 개념`
#### 프로그램
프로그램은 C언어로 코딩된 파일이 실행될때 컴파일되면서 Executable 파일이 생성되는데 그것이 프로그램
instruction과 static데이터들은 Executable파일 형태로 Disk에 저장됨
#### 프로세스
`실행중인 프로그램`  
Executable 파일을 실행시키려면 그 파일을 클릭하거나, 커맨드를 날리는데 그때 실행되는 인스턴스를 프로세스라고함
여러개의 프로세스가 실행될 수 있는데, 프로세스는 state를 갖으며 실행됨
Machine state
- Memory : 어떤 프로그램이 실행되기 위햐선 메모리에 적재되어야 하는데 적재되어야하는 내용 -> instruction,data
- Register : program counter(CPU가 실행해야하는 instruction을 가르키는 register),  stack pointer(현재 stack의 탑이 어디냐)..
- Others : 오픈한 파일의 리스트 등

## fork() System call
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
int main(int argc, char *argv[]){
  printf("hello world (pid:%d)\n", (int) getpid()); int rc = fork();
  if (rc < 0) {
    fprintf(stderr, "fork failed\n");
    exit(1);
    } else if (rc == 0){
    printf("I am child (pid:%d)\n", (int) getpid()); }
    else
    {
      printf("I am parent of %d (pid:%d)\n", rc, (int) getpid());
    }
  return 0;
}
```
### 결과 1
```
prompt> ./p1
hello world (pid:29146)
hello, I am parent of 29147 (pid:29146) hello, I am child (pid:29147)
prompt>
```
### 결과 2
```
prompt> ./p1
hello world (pid:29146)
hello, I am child (pid:29147)
hello, I am parent of 29147 (pid:29146) prompt>
```
운영체제가 CPU를 누구한테 먼저 배정할지에 대한 policy들에 의해서 결정되는지 개발자 입장에선 예측하기 힘듬  
## wait() System call
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/wait.h>

int main(int argc, char *argv[]){
    printf("hello world (pid:%d)\n", (int) getpid());
    
    int rc = fork();
    if (rc < 0) {
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) {
        printf("I am child (pid:%d)\n", (int) getpid());
    } else {
        int wc = wait(NULL);
        printf("I am parent of %d (wc:%d) (pid:%d)\n", rc, wc, (int) getpid());
    }
    return 0;
}

```
child 프로세스가 하나라도 종료될때까지 기다림
## 결과
```
prompt> ./p2
hello world (pid:29266)
hello, I am child (pid:29267)
hello, I am parent of 29267 (wc:29267) (pid:29266) prompt>
```
wailt 시스템콜을 했기때문에 parent에게 할당됬던 CPU가 child에게 양보되어서  child가 먼저 실행된 것이지 (child에게 먼저 할당되었을 수 있음, CPU의 할당 순서를 보장하는 것이 아니라는 말) 
wait 시스템 콜을 했다고해서 fork시스템 콜 이후에 CPU자원이 child에게 먼저가는 것은 아님

## exec() System call
```
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <string.h>
#include <sys/wait.h>

int main(int argc, char *argv[]) {
    printf("hello world (pid:%d)\n", (int) getpid());
    
    int rc = fork();
    if (rc < 0) {
        fprintf(stderr, "fork failed\n");
        exit(1);
    } else if (rc == 0) {
        // Child process
        printf("I am child (pid:%d)\n", (int) getpid());
        
        char *myargs[3];
        myargs[0] = strdup("wc");  // "wc" is a command-line utility to count lines, words, and bytes.
        myargs[1] = strdup("p3.c"); // Assuming "p3.c" is a file you want to count words, lines, and bytes in.
        myargs[2] = NULL;  // Mark the end of the array
        
        execvp(myargs[0], myargs); // Replace the child process with the "wc" program.
        printf("this shouldn't print out");
    } else {
        // Parent process
        int wc = wait(NULL); // Wait for child to complete
        printf("I am parent of %d (wc:%d) (pid:%d)\n", rc, wc, (int) getpid());
    }
    return 0;
}

```
## 결과
```
prompt> ./p3
hello world (pid:29383)
hello, I am child (pid:29384)
29 107 1030 p3.c
hello, I am parent of 29384 (wc:29384) (pid:29383) prompt>
```
wait 시스템 콜이 parent쪽에서 호출되므로 마지막엔 parent의 pid출력


## How to provide the illusion of many CPUs?
코어수보다 많은 프로세스가 생성되면 어떻게 효율적으로 실행되는지가 관심사

# Virtualization of the CPU
## Time sharing
- Illusion that many virtual CPUs exist when in fact there is only
one physical CPU (or a few)
- Users can run as many concurrent processes as they would like
- Potential cost : 전체적인 성능이 떨어지는 것이 아닌가 라는 의문
- Context switch: 프로그램이 실행되다가 잠시 멈추고 다시 실행되는 메커니즘
- scheduling policy : 언제 switch를 할지에 관한 policy

![image](https://github.com/Mouon/-/assets/137624597/faf2fc1a-edd2-40c9-97b3-9d3dff1794b4)


# Process State

<img width="237" alt="스크린샷 2024-03-06 오전 9 46 48" src="https://github.com/Mouon/-/assets/137624597/afd9620b-461c-479a-b9ed-9f78d4a65d70">

### Running 
OS가 Ready상태의 프로세스를 할당하면 Running 상태로 전환됨
### Ready
프로세스가 생성이 되면 기본적으로 Ready 상태에 속함
### Blocked (Waiting)
프로세스가 CPU를 할당 받아서 실행중에 IO요청을 하면 IO장치의 응답을 기다리며 대기하게되는데 이때 Blocked상태가 되며 CPU를 Ready상태의 프로세스에게 할당합니다.
IO가 끝나면 다시 Ready상태로 됨


# 운영체제는 Process의 state를 어떤식으로 관리하고있는가?
운영체제는 프로세스가 생성될때마다 해당 프로세스를 관리해줄 수 있는 자료구조를 생성합니다.  그 자료구조는 보통 process controll block이라고 불립니다. `PCB`
## Data Structure
### Linux kernel
 `/include/linux/sched.h`  
pcb 여할을 함
```
struct task_struct {
    volatile long state; /* TASK_RUNNING, TASK_INTERRUPTIBLE ... */
    void *stack; /* Pointer to the kernel-mode stack */
    ...
    unsigned int cpu;/* 어느 CPU에서 도는지 */
    ...
    struct mm_struct *mm;
    ...
    struct task_struct *parent;
    struct list_head children;
    ...
    struct files_struct *files; /* 오픈한 파일 리스트를 관리함 */
    ...
};

```
리눅스는 Running, Ready 상태를 구분하지 않음

