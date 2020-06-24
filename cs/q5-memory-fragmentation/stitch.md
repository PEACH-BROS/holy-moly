# 메모리 단편화에 대하여

## 메모리 단편화 : *Memory Framentation*

> 기억 장치의 빈 공간 또는 자료가 여러 개의 조각으로 나뉘는 현상을 말한다. 이 현상은 기억장치의 사용 가능한 공간을 줄이거나, 읽기와 쓰기의 수행속도를 늦추는 문제점을 야기한다 - [wikipedia](https://ko.wikipedia.org/wiki/단편화))

단편화의 종류는 아래와 같음.

- 외부 단편화 : *External Fragmentation*
- 내부 단편화 : *Internal Fragmentation*
- 자료 단편화 : *Data Fragmentation*



## 연속적 할당 : *Continuous Allocation*

**logical address**가 연속적이면 **physical address**도 연속적으로 배치되어야 함.

하나의 프로세스를 물리적인 공간에 반으로 쪼개서 배치할 수 없고 연속적으로 이어지게 배치를 해야한다는 것.

이러한 이유로 External Fragmentation이 발생하게 됨.



## 외부 단편화 : *Exteranl Fragmentation*

전체 공간을 계산해봤을 때 요청을 만족할만한 충분한 메모리가 있음에도, 가능한 공간들이 연속적이지 않을 때. 즉, 저장 공간이 작은 hole들로 조각조각 나있을 때.

이때, hole은 특정 프로세스가 작업을 종료하고 메모리를 반환함으로 생기는 빈 공간을 말함.

이러한 문제를 해결하기 위해 나온 방법으로 Compaction과 Paging이 존재.



### 압축 : *Compaction*

![image-20200624025222817](/Users/junyoung/Library/Application Support/typora-user-images/image-20200624025222817.png)

기존의 hole을 제외하고 메모리에 올려져 있는 프로세스를 이동시켜 공간을 확보하는 방법.

이는 남아있는 hole을 연속적인 공간으로 만들어 줄 수 있어서 외부 단편화 문제를 해결할 수 있다.

그러나 프로세스를 메모리가 아닌 secondary storage에 임시로 복사하고 다시 가져와야 하기 때문에 실행 속도가 느리다는 단점을 가지고 있음.



### 페이징 : *Paging*

![image-20200624025917060](/Users/junyoung/Library/Application Support/typora-user-images/image-20200624025917060.png)

페이징은 프로세스를 일정 크기인 페이지(page)로 잘라서 메모리에 적재하는 방식을 말함. 

logical address를 동일한 크기로 자르고 physical address도 동일한 크기로 잘라서 mapping을 시키면 프로세스가 continuous하게 저장되지 않아도 됨.

> 이때, Logical address space를 동일한 크기로 나눈 것을 페이지(*page*)라고 하고 Physical address space를 동일한 크기로 나눈 것을 프레임(*frame*)라고 한다.



### 페이징이 가지는 장단점

#### 장점

- Physical memory에서 할당과 반환이 쉽다.
- 외부 단편화가 발생하지 않는다.
- virtual address space가 physical address space보다 커도 상관없다.

#### 단점

- 내부 단편화가 발생할 수 있다.
  - 하지만 외부 단편화로 발생하는 메모리 손실에 비해 내부 단편화로 발생하는 메모리 손실은 미비하다.
- 메모리 참조 오버헤드가 존재한다.
  - 페이지 테이블과 메모리 각각을 확인해야 하므로 2번의 메모리 참조가 발생한다.
  - 이를 해결하기 위해 TLB(translation lookaside buffers)를 사용한다.
- 페이지 테이블에 추가적인 공간을 필요로 한다.



## 내부 단편화 : *Internal Fragmentation*

프로세스가 필요한 양보다 더 큰 메모리가 할당되어서 프로세스에서 사용되는 메모리 공간이 낭비 되는 상황.

페이징을 예로 들어보면, 프로세스의 크기가 6kb이고 페이지의 크기가 4kb일 경우 해당 프로세스는 2개의 페이지를 할당받아야 한다. 이때 할당받는 공간은 8kb이지만 실제 프로세스가 필요로 하는 공간은 6kb이므로 2kb의 공간은 사용되지 않고 낭비된다. 이를 **내부 단편화**라고 한다.



## 참고 링크

> [Memory Fragmentation - 메모리 단편화 - huisam](7c483f1b2bd0f7b1a69b1330790913e6f82b18ec)
>
> [메모리 단편화(Memory Fragmentation) - junsday](https://junsday.tistory.com/36)
>
> [Fragmentation 메모리 단편화란 무엇인가? - IT 양햄찌(jhnyang)](https://jhnyang.tistory.com/264)
>
> [메모리 관리기법 - 페이징(paging)이란? - IT 양햄찌(jhnyang)](https://jhnyang.tistory.com/290?category=815411)