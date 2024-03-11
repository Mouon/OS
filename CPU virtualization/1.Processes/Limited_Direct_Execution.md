# Direct Execution


## OS
- Create entry for process list
- 프로그램에 메모리를 할당
- 메모리로부터 프로그램 로드 ( CPU는 storage에 있는 명령어를 실행 못시킴, 그래서 메모리로 로드해야함 )
- Set up stack with argc/argv ( 스택을 초기화 )
- 레지스터들을 초기화
- Execute call main()

## Program
- Run main()
- Execute return from main()

## 운영체제가 관여를 해줘야함
### 그렇지 않으면 문제가 발생함

# Problem 1: Restricted Operations
Restricted operations (privileged operations)

## Processor Modes
- User mode (level 3 : Applications)
- Kernel mode (level 0 : Operation system kernel) : 제약 없이 모든 instruction이 실행 가능하다

## System Calls
- Trap instruction : 커널모드로 승격됨
- Return-from-trap instruction : 커널모드가 다시 유저모드로 변함

### Trap table
- When the machine boots up, the OS informs the hardware of the locations of trap handlers
- A system-call number is assigned to each system call
- 응용프로그램은 단순히 번호만 명시하지 정학한 주소는 명시하지 못함
  
# Problem 2: Switching Between Processes


