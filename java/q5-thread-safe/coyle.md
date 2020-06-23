## 1-1. Thread-safe란 무엇인가요?

답변 : 멀티 스레드 환경에서 공유 자원에 대해 여러 쓰레드가 접근하여도 이상 현상 없이 동작하는 것

예를 들면 class가 static한 변수 A를 들고 있고 100개의 쓰레드가 동시에 A의 값을 1 증가시키는 명령 100번 받았을 때 10000의 값을 반환하는 것

가장 쉽게 스레드 안전성을 달성하는 방법은 상태비저장 구현, 불변이다.

## 1-2. 그럼 자바에서 공유 자원에 대해 어떻게 thread-safe 하게 할까요?

답변 :  synchronized가 있다.

1. Atomic class
2. Vector, HashTable, CuncurrentHashMap, String 사용
   - ThreadSafe Collection 사용
3. Lock & UnLock을 통해 한 시점에 하나의 Thread만 해당 Method 혹은 Block을 실행할 수 있도록 Thread를 제어한다.
4. Double Check Locking
5. Volatile

## 1-3. 방금 말한 Synchronized는 어떻게 구현되어 있을까요?

답변 : 메소드 혹은 코드 블록을 블락, 언블락한다.

## 1-4. Synchronized의 단점은 무엇이 있을까요?

답변 : 락을 크게 건다. 비용이 크다.

- synchronized는 한 쓰레드가 해당 메소드를 실행하는 동안, 다른 쓰레드는 하염없이 기다려야 하기 때문에 전반적으로 프로그램의 속도를 느리게 만든다.

## 1-5. 말한 단점 외에도 Synchronized를 자주 쓰지 않는 이유는 뭘까요?

답변 : dead lock 이슈가 발생할 수 있다.

- 객체 자체에 lock이 걸린다?

## 1-6. Synchronized의 단점을 커버하는 방식은 뭐가 있나요?

답변 : volatile 존재

volatile

- `volatile` 키워드는 Java 변수를 Main Memory에 저장하겠다는 것을 명시하는 것이다.

- 매번 변수의 값을 Read 할때마다 CPU cache에 저장된 값이 아닌 Main Memory에서 읽는 것이다.

- 또한 변수의 값을 write 할때마다 Main Memory에 까지 작성하는 것이다.
- `volatile` : Main Memory에 값을 read & write 하는 키워드

- 상황?
  - **하나의 Thread만 write하고 나머지 Thread가 read하는 상황인 경우**
  - 변수의 값이 최신의 값으로 읽어와야 하는경우(최신의 값을 보장)

- 주의할 점 : 성능에 어느 정도 영향을 줄 수 있다.
  - 이유 : CPU cache보다 Main Memory 접근이 비용이 더 크기 때문이다.



### 출처

[Atomic operation이란?](
