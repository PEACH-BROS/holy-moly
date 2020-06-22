# Garbage Collector

# 배경지식

### Garbage Collection?

JVM의 Heap 영역에서 사용 중인 객체와 사용 중이지 않은 객체를 식별하고, 사용하지 않는 객체를 삭제하는 프로세스를 말한다.

자바의 GC는 "Reachability"라는 개념을 사용한다.  
어떤 객체에 유효한 참조가 존재한다면 "Reachable", 그렇지 않다면 "Unreachable"로 구별하고, Unreachable한 놈들이 GC의 대상이 된다!

> **힙에 있는 객체들에 대한 참조는 4가지 종류 중 하나이다.**
>
> 1. 힙 내의 다른 객체에 의한 참조
> 2. Java 스택, Java 메소드 실행 시 사용되는 지역변수와 파라미터에 의한 참조
> 3. JNI에 의해 생성된 객체에 대한 참조
> 4. 메소드 영역의 static 변수에 의한 참조

---

### System.gc()?

![image](https://user-images.githubusercontent.com/19922698/85226692-dccca980-b413-11ea-8640-3fd728d95104.png)

여기서 더 이상 들어갈 수 없다! **native** 키워드는 자바가 아닌 언어(보통 C, C++)로 구현한 후, 자바에서 사용하려고 할 때 이용한다. 자바로 구현하기 까다로운 것을 다른 언어로 구현해서 자바에서 사용하기 위한 방법이다. 이를 실행할 때 JNI를 사용한다.

---

### stop-the-world

: GC를 실행하기 위해 JVM이 애플리케이션 실행을 멈추는 것.

GC를 실행하는 쓰레드를 제외한 나머지 쓰레드는 모두 작업을 멈춘다. GC 작업을 완료한 이후에 중단했던 작업을 다시 시작한다. 어떤 GC 알고리즘을 사용하더라도 stop-the-world는 발생한다.

---

### **Young Generation과 Old Generation**

이렇게 GC를 실행하는 순간 모든 작업을 멈춘다. 그리고, Heap의 모든 메모리를 뒤져 가며 객체를 식별한다면 굉장히 비효율적일 것이다.

그래서 GC는 아래 2가지 가설 하에 만들어졌다.

#### Weak Generational Hypothesis

- 대부분의 객체는 금방 접근 불가능한 상태(Unreachable)가 된다.
- 오래된 객체에서 젊은 객체로의 참주는 아주 적게 존재한다.



이 가설의 장점을 최대한 살리고, JVM의 성능 향상을 위해 JVM의 Heap 영역은 Young Generation과 Old Generation으로 나뉘어 관리한다.

<br>

---

# GC의 작동 과정

1. **Mark**: GC는 Stack의 GC Root로부터 모든 변수를 스캔하면서 각각 어떤 객체를 참조하고 있는지 찾아서 마킹한다. Reachable한 Object를 마킹한다!

   ❗️GC Root로부터 찾아 들어가나? 아님 모든 heap 영역을 처음부터 탐색하나?

   ![image](https://user-images.githubusercontent.com/19922698/85224883-61192f80-b408-11ea-822c-6e71b18e2705.png)

2. **Sweep(Normal Deletion)**: Unreachable Object를 Heap에서 제거한다. 이 때, Reachable Object와 제거된 영역의 포인터를 남겨둔다!

   ![image](https://user-images.githubusercontent.com/19922698/85224937-8e65dd80-b408-11ea-9012-02cf62df0c1a.png)

3. 추가로 **Compact** 과정이 존재한다. Sweep 후 분산된 객체들을 Heap의 시작 주소로 모아 메모리가 할당된 부분과 그렇지 않은 부분으로 나눈다. (이 과정은 메모리 할당을 더 빠르고 쉽게 만든다!)

   ![img](https://k.kakaocdn.net/dn/pHG9V/btqBFMdMpUh/VLUOTKEDH0h4VNx1NoG5Wk/img.png)



---



# GC의 작동 순서

![image](https://user-images.githubusercontent.com/19922698/85226514-cffb8600-b412-11ea-91a5-073d2a89b0f7.png)



**Young Generation:** 새로운 객체들이 이 영역에 할당된다. `Eden`, `Survival 0`, `Survival 1` 영역으로 나뉘어지며, Young 영역에서 일어나는 GC를 `Minor GC`라고 한다.

**Old Generation:** Young 영역에서 오랫동안 살아남은 객체들은 Old 영역으로 이동한다. 여기서 일어나는 GC를 `Major GC` or `Full GC` 라고 한다.

**Metaspace:** 자바 7까지 Permanent Generation 영역이었으나, 제거되었다. 클래스 메타데이터들이 이 영역에 할당된다.





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



# GC의 종류

#### 1. Serial GC

Java SE 5, 6에서의 기본 가비지 컬렉터. 싱글 쓰레드 환경에 맞게 설계되었다. Young/Old Generation에 대한 GC과정이 싱글 쓰레드로 동작하기 때문에, 다른 GC에 비해 Stop-The-World 시간이 길다.

#### 2. Parallel GC

Serial GC와 같지만, Young 영역의 GC과정을 멀티 쓰레드로 수행한다. 그래서 Serial GC에 비해 Stop-The-World 시간이 줄어든다.

![image](https://user-images.githubusercontent.com/19922698/85227976-954a1b80-b41b-11ea-9523-89a831ee2156.png)

#### 3. Parallel Old GC

Young/Old 영역 모두 멀티 쓰레드로 수행한다. Parallel GC보다 더 빠른 성능을 보여준다.



#### 4. CMS GC (Concurrent Mark Sweep)

Old 영역에 대한 GC이며, Young 영역은 Parallel GC와 같은 방식으로 동작한다. Stop-The-World 시간을 최소화하기 위해 설계되었다. 따라서, 다른 GC와 달리 **Compact 과정이 없다.**



#### 5. G1 GC (Garbage First GC)















> 참고  
> https://www.oracle.com/webfolder/technetwork/tutorials/obe/java/gc01/index.html   
> https://javabom.tistory.com/7  
> https://www.youtube.com/watch?v=vZRmCbl871I   
> https://d2.naver.com/helloworld/1329