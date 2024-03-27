# Segment
- A contiguous portion of the address space of a particular length
    - E.g., code, stack, heap

# Segmentation
- Places each segment in different parts of physical memory
- Avoids filling physical memory with unused virtual address space
    - 낭비되고있는 공간을 잡는다.
 

## Relocation
<img width="495" alt="스크린샷 2024-03-27 오후 1 46 28" src="https://github.com/Mouon/OS/assets/137624597/d02690f5-f849-4579-8422-27abbc14922b">

stack과 Program Code사이를 비워둘 필요는 없음  
(프로세스 단위가아니라 새그먼트 단위로)새그먼트의 Base와 Size가 각각 필요하다.  

<img width="225" alt="스크린샷 2024-03-27 오후 1 53 18" src="https://github.com/Mouon/OS/assets/137624597/094dc0a0-f858-4cb9-aec2-1c5da4841079">
- Fetch from address 100
  32KB + 100 = 32868B   
  100 is less than 2KB

- Load from address 4200 ( 힙의 시작점부터 offset으로 계산해야한다.)
  34KB + 4200 = 39016 (x)
  offset = 4200 – 4KB
