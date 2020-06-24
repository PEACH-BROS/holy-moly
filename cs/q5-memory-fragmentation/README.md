# 메모리 단편화에 대해 설명하세요

[비밥의 메모리 단편화](bebop.md)

[엘리의 메모리 단편화](elly-memory-fragmentation.md)

[스리리의 메모리 단편화](stitch.md)



## 메모리 단편화 : *Memory Framentation*

기억 장치의 빈 공간 또는 자료가 여러 개의 조각으로 나뉘는 현상을 말한다. 이 현상은 기억장치의 사용 가능한 공간을 줄이거나, 읽기와 쓰기의 수행속도를 늦추는 문제점을 야기한다 (사용 가능한 메모리가 충분히 존재하지만, 할당(사용)이 불가능한 상태)



- 외부 단편화 : *External Fragmentation*
- 내부 단편화 : *Internal Fragmentation*



## 연속적 할당 : *Continuous Allocation*

기존 메모리는 **logical address**가 연속적이면 **physical address**도 연속적으로 배치된다.

하나의 프로세스를 물리적인 공간에 반으로 쪼개서 배치할 수 없고 연속적으로 이어지게 배치를 해야한다는 것.

이러한 이유로 External Fragmentation이 발생하게 됨.

*cf) MMU*



## 외부 단편화 : *Exteranl Fragmentation*

전체 공간을 계산해봤을 때 요청을 만족할만한 충분한 메모리가 있음에도, 가능한 공간들이 연속적이지 않을 때. 즉, 저장 공간이 작은 hole들로 조각조각 나있을 때.

이때, hole은 특정 프로세스가 작업을 종료하고 메모리를 반환함으로 생기는 빈 공간을 말함.

이러한 문제를 해결하기 위해 나온 방법으로 `Compaction`과 `Paging`이 존재.

![image](https://user-images.githubusercontent.com/19922698/85363320-95513500-b55b-11ea-880d-ae47648892f5.png)



### 압축 : *Compaction*

기존의 hole을 제외하고 메모리에 올려져 있는 프로세스를 이동시켜 공간을 확보하는 방법.

이는 남아있는 hole을 연속적인 공간으로 만들어 줄 수 있어서 외부 단편화 문제를 해결할 수 있다.

그러나 프로세스를 메모리가 아닌 secondary storage*(ex. HDD)*에 임시로 복사하고 다시 가져와야 하기 때문에 실행 속도가 느리다는 단점을 가지고 있음.



### 페이징 : *Paging*

페이징은 프로세스를 일정 크기인 페이지(page)로 잘라서 메모리에 적재하는 방식을 말함. 

물리 메모리를 사용 할 때 고정된 크기의 메모리를 프레임 단위로 나누어 사용하면서 연속되지 않은 메모리들이 발생하였다.

이러한 문제를 해결하기 위해 **가상 메모리** 계층을 두고  **매핑 테이블을 이용**하여 실제로 단편화되어 있는, **연속적이지 않은 물리적 메모리를 사용**함으로써  **외부 단편화 문제를 해결**할 수 있게된다.

logical address를 동일한 크기로 자르고 physical address도 동일한 크기로 잘라서 mapping을 시키면 프로세스가 continuous하게 저장되지 않아도 됨.

> 이때, Logical address space를 동일한 크기로 나눈 것을 페이지(*page*)라고 하고 Physical address space를 동일한 크기로 나눈 것을 프레임(*frame*)라고 한다.

![image](https://user-images.githubusercontent.com/19922698/85364673-9d5ea400-b55e-11ea-9121-4abe528507a3.png)

#### 장점

- Physical memory에서 할당과 반환이 쉽다.
- 외부 단편화가 발생하지 않는다.
- 전체 virtual address space가 전체 physical address space보다 커도 상관없다.

#### 단점

- 내부 단편화가 발생할 수 있다.
  - **하지만 외부 단편화로 발생하는 메모리 손실에 비해 내부 단편화로 발생하는 메모리 손실은 미비하다.**
- 메모리 참조 오버헤드가 존재한다.
  - 페이지 테이블과 메모리 각각을 확인해야 하므로 2번의 메모리 참조가 발생한다.
  - 이를 해결하기 위해 TLB(translation lookaside buffers)를 사용한다.
- 페이지 테이블에 추가적인 공간을 필요로 한다.
- 페이지 단위를 작게 하면 내부 단편화 역시 해결할 수 있지만, 그만큼 page mapping 과정이 증가하기 때문에 **trade off**이다.





## 내부 단편화 : *Internal Fragmentation*

프로세스가 필요한 양보다 더 큰 메모리가 할당되어서 프로세스에서 사용되는 메모리 공간이 낭비 되는 상황.

페이징을 예로 들어보면, 프로세스의 크기가 6kb이고 페이지의 크기가 4kb일 경우 해당 프로세스는 2개의 페이지를 할당받아야 한다. 이때 할당받는 공간은 8kb이지만 실제 프로세스가 필요로 하는 공간은 6kb이므로 2kb의 공간은 사용되지 않고 낭비된다. 이를 **내부 단편화**라고 한다.

![image](https://user-images.githubusercontent.com/19922698/85363300-84082880-b55b-11ea-8bfc-5f8ef596cd3d.png)

![image](https://user-images.githubusercontent.com/13347548/85362158-b82e1a00-b558-11ea-812b-9d811006154f.png)



### 세그멘테이션 : Segmentation

가상메모리를 서로 *다른* 크기의 세그먼트로 분할해서 메모리 할당 후, 실제 메모리 주소로 변환

각 세그먼트는 연속적인 공간에 저장되어 있으며, 매핑을 위해 segment table이 필요하다.

세그먼트의 크기가 다 다르기 때문에, 페이징 기법처럼 메모리를 미리 분할해 둘 수 없고, 메모리가 적재될 때 빈 공간을 찾아 할당한다.

❓세그멘테이션 기법이 어떻게 내부 단편화를 해결하지?

❗️메모리는 byte 단위가 아니라 시스템 block 단위로 할당되고, block의 크기는 시스템마다 다르다. 예를 들어, 1 block = 1 KB인 시스템이 있다. 프로세스가 14.3KB를 요청하면 시스템은 블록 단위로 15KB만큼 할당해준다. 이렇게 되면 0.7KB의 메모리 낭비가 일어나게 된다.

세그멘트 테이블은 세그먼트들의 정보가 담겨있는데, 세그먼트의 시작주소(base), 세그먼트의 크기(=프로세스가 필요한 메모리 공간)(limit)를 가지고 있다.

![image](https://user-images.githubusercontent.com/13347548/85366894-34c5f600-b563-11ea-90cd-7f9f05b7b392.png)

#### 🚨 하지만 외부 단편화 문제가 심각하기 때문에 페이징을 쓰는게 더 좋다.



> ### **메모리 구조**
>
> ![image](https://user-images.githubusercontent.com/19922698/85486815-57f5b180-b606-11ea-8e58-b2de22ff0e73.png)

Primary Memory : 우리가 쓰는 RAM

Secondary Storage : HDD, 외장하드, CD 등

