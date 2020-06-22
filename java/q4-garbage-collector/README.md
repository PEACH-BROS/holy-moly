# Java의 Garbage Collector에 대해 설명해보세요.

[엘리의 가비지 컬렉터](elly-garbage-collector.md)

[비밥의 가비지 컬렉터](bebop.md)

[코일의 가비지 컬렉터](coyle-gc.md)

스티치는 서핑중

---

# 배경지식

### Garbage Collection?

JVM의 Heap 영역에서 사용 중인 객체와 사용 중이지 않은 객체를 식별하고, 사용하지 않는 객체를 삭제하는 프로세스를 말한다.

자바의 GC는 "Reachability"라는 개념을 사용한다.  
어떤 객체에 유효한 참조가 존재한다면 "Reachable", 그렇지 않다면 "Unreachable"로 구별하고, Unreachable한 놈들이 GC의 대상이 된다!

**힙에 있는 객체들에 대한 참조는 4가지 종류 중 하나이다.**

**GC Root가 되는 유효한 참조**

1. Java 스택, Java 메소드 실행 시 사용되는 지역변수와 파라미터에 의한 참조
2. JNI에 의해 생성된 객체에 대한 참조
3. 메소드 영역의 static 변수에 의한 참조

**GC Root는 아니지만 유효한 참조**

1. 힙 내의 다른 객체에 의한 참조

**GC Root 란?**

항상 유효한 최초의 객체

![GC Root](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fk.kakaocdn.net%2Fdn%2Fcg8Cze%2FbtqBDa8hTHI%2FsFicepGVyJY1WdT3IN2lF0%2Fimg.png)



### Heap 영역

- New Generation
  - Eden
  - Survival 0
  - Survival 1
- Old Generation

## GC의 작동 순서

1. **Mark**: GC Root로 부터 Reachability한 객체를 찾아서 마킹  
   Reachable Object가 참조하고 있는 객체도 찾아 마킹

   ![image](https://user-images.githubusercontent.com/19922698/85224883-61192f80-b408-11ea-822c-6e71b18e2705.png)

2. **Sweep(Normal Deletion)**: Unreachable Object를 Heap에서 제거한다. 이 때, Reachable Object와 제거된 영역의 포인터를 남겨둔다!

   ![image](https://user-images.githubusercontent.com/19922698/85224937-8e65dd80-b408-11ea-9012-02cf62df0c1a.png)

3. 추가로 **Compact** 과정이 존재한다. Sweep 후 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 나눈다. (이 과정은 메모리 할당을 더 빠르고 쉽게 만든다!)

   ![img](https://k.kakaocdn.net/dn/pHG9V/btqBFMdMpUh/VLUOTKEDH0h4VNx1NoG5Wk/img.png)

Mark & Sweep 과정을 거치면서 구멍 뚫린 메모리 공간을 효율적으로 사용하기 위해 **메모리를 다시 정렬하는 과정을 Compact**라고 하는데 이 과정은 GC 효율의 시작이자 끝인 **Stop the World를 유발**한다.

### stop-the-world

: GC를 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것.

GC를 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춘다. GC 작업을 완료한 이후에 중단했던 작업을 다시 시작한다. 어떤 GC 알고리즘을 사용하더라도 stop-the-world는 발생한다.

stop the world는 compact 과정과 gc가 일어나면서 메모리의 할당 영역이 바뀔 때 필요하다.

#### **Young Generation과 Old Generation**

이렇게 GC를 실행하는 순간 모든 작업을 멈춘다. 그리고, Heap의 모든 메모리를 뒤져 가며 객체를 식별한다면 굉장히 비효율적일 것이다.

그래서 GC는 아래 2가지 가설 하에 만들어졌다.

#### Weak Generational Hypothesis

- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 젊은 객체로의 참주는 아주 적게 존재한다.

### GC 상세 과정

#### 1. 새로운 객체가 Eden 영역에 할당된다.

![image](https://user-images.githubusercontent.com/19922698/85226910-e276bf00-b414-11ea-8a91-26dbb5104b8c.png)

#### 2. Eden 영역이 가득 차면! Minor GC가 일어난다. 이 때, Reachable 객체는 Survival 0 또는 1 *(우선은 S0라고 해두자)*로 이동하고, Eden 영역은 비어있는 상태가 된다. (Eden 영역을 비우면서 Unreachable 객체들은 제거된다)

![image](https://user-images.githubusercontent.com/19922698/85227010-6630ab80-b415-11ea-8125-2ab29bb7a607.png)

![image](https://user-images.githubusercontent.com/19922698/85227061-c0ca0780-b415-11ea-83b8-d0666572596b.png)

#### 3. 또 다시 Eden 영역이 가득 차면 Minor GC가 일어난다. 이번에는 Survival 0에 있던 객체들이 age 값이 증가한 채로 Survival 1 영역으로 이동한다. (S0은 비워진다)

![image](https://user-images.githubusercontent.com/19922698/85227303-96794980-b417-11ea-931a-b5fe6fb9c49f.png)

#### 4. 다음 Minor GC에서도 같은 일이 반복된다. Eden 영역에 새로 할당된 Reachable 객체들 + 기존 S1에 있던 객체들의 age 값이 증가한 채로 S0 영역으로 이동하고, S1은 비워진다.

![image](https://user-images.githubusercontent.com/19922698/85227292-895c5a80-b417-11ea-82a3-d473cd61ef34.png)



#### 5. Promotion : age 값이 특정 임계점(그림에서는 8)에 도달하면, Young Generation에서 Old Generation으로 승진한다.

![image](https://user-images.githubusercontent.com/19922698/85227283-7ba6d500-b417-11ea-9963-d99db720d5d2.png)

*Tenured: Old Generation 영역*



#### 6. Promotion이 반복되면 Old Generation이 가득 차게 되고, Major GC가 일어난다.

![image](https://user-images.githubusercontent.com/19922698/85227384-14d5eb80-b418-11ea-8268-33b1d184d8ab.png)

### GC의 종류

1. Serial GC
   - GC를 처리하는 스레드가 1개
   - CPU코어가 1개만 있을 때 사용
   - Mark-Compact Collection 알고리즘
     - 데이터 해제 후 한곳으로 몰아둬 메모리 파편화를 방지
2. Parallel GC
   - GC 처리 스레드 여러 개
   - Serial GC보다 빠르게 객체 처리
   - Parallel GC는 메모리가 충분하고 코어의 개수가 많을 때 사용하면 좋다.
3. Concurrent Mark Sweep GC (CMS GC)
   - Stop the world를 줄인다.
   - 빠른 응답이 중요한 어플리케이션에 사용된다.
   - 다른 GC보다 메모리와 CPU를 더 많이 사용한다.
     - GC 사이클이 조금 더 길기 때문에
   - Compaction 존재 X
   - Initial Mark
     - Stack의 모든 변수를 스캔하면서 각각 어떤 객체를 참조하고 있는지 찾아서 마킹
   - Concurrent Mark
     - 이후의 객체 그래프 참조 마킹
   - Remark
     - Initial Mark, Concureent Mark 과정 중 다른 스레드가 새로 생성한 객체들을 다시 한번 체크 후 마크
   - Concurrent Sweep
     - 마킹안된 데이터를 해제
     - 다른 스레드 같이 실행된다

4. G1 GC
   - 각 영역을 Region 영역으로 나눈다.
   - GC가 일어날 때 전체 영역을 탐색하지 않는다.
   - 바둑판의 각 영역에 객체를 할당하고 GC를 실행
     - 영역이 꽉차면 빈 영역에 객체를 할당하고 GC를 실행
   - STW 시간이 짧다.
   - Compaction을 사용