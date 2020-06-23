# Multi Thread에 대해 설명해주세요.

[비밥의 Thread-Safe](bebop.md)



[엘리의 Thread-Safe](elly-thread-safe.md)



[코일의 Thread-Safe](coyle.md)



---
## 1-1. Thread-safe란 무엇인가요?

특정 쓰레드가 작업을 수행 중일 때, 다른 쓰레드의 실행에 의해 영향을 받지 않게 하는 것

- 어떤 자원에 대해 여러 쓰레드가 동시에 접근해도 프로그램 실행에 문제가 없다.

## 1-2. 그럼 자바에서 공유 자원에 대해 어떻게 thread-safe 하게 할까요?

공유자원에 접근하는 **메소드** 혹은 **메소드 내 코드 블럭 일부분**에 대해 Synchronized 키워드를 이용하여 lock을 걸어서 구현한다.

```java
public synchronized void foo() { ... } //메소드에 동기화 락이 걸린다.
```

```java
public void foo() {
  	synchronized(this) {
      ...//동기화할 로직
    }
}
```

##### 교착상태 발생 조건

- 상호배제 : 프로세스들이 필요로 하는 자원에 대한 배타적인 통제권을 요구한다.
- 점유대기 : 프로세스가 할당된 자원을 가진 상태에서 다른 자원을 기다린다.
- 비선점 : 프로세스가 어떤 자원의 사용을 끝낼 때까지 그 자원을 뺏을 수 없다.
- 순환대기 : 각 프로세스는 순환적으로 다음 프로세스가 요구하는 자원을 가지고 있다.



## 1-3. 방금 말한 Synchronized는 어떻게 구현되어 있을까요?

Synchronized 키워드로 걸어 놓은 객체 혹은 메소드 시그니쳐에 대해 접근 할 때 OS 수준의 스레드 동기화 메커니즘(ex. mutex 이용)을 사용할 수 있도록 구현되어 있을것이다.

또한 lock으로 교착 상태를 해결하고 내부적인 구현으로 기아 상태가 걸리지 않도록 thread에게 우선순위를 부여할 것이다.

##### 기아상태 해결방법 Example

- 프로세스 우선순위 수시 변경을 통해 각 프로세스 높은 우선순위를 가지도록 기회 부여
- 오래 기다린 프로세스의 우선순위 높이기
- 우선순위가 아닌 요청 순서대로 처리하는 요청큐 사용



## 1-4. Synchronized의 단점은 무엇이 있을까요?

메소드에 사용하는게 아닌 코드 블럭 일부분에 사용해야 하는경우 락 객체를 생성해야 하고 개발자가 락 객체를 관리해줘야 하는 관리 포인트가 생기게된다.

관리를 잘 못하게 된다면 데드락과 같은 상황에 빠질 수 있다.

또한 동기화 처리로 인한 성능 저하가 발생할 수 있다.

## 1-5. 말한 단점 외에도 Synchronized를 자주 쓰지 않는 이유는 뭘까요?



## 1-6. Synchronized의 단점을 커버하는 방식은 뭐가 있나요?



- `Vector`나 `Hashtable` 등 동기화된 컬렉션 사용은 반드시 1개의 쓰레드만 사용하게 되어 있었으므로 성능 이슈가 발생한다.
- 읽기 전용 객체(= 불변 객체)
  - ex) Map Interface keySet



Concurrent api를 사용하는 방법이 있다.
동기화를 위한 잠금 단위가 훨씬 세밀하기 때문에 성능상 더 좋다.

특히 읽기 작업(`get()`)의 경우 locking 하는 과정이 필요가 없다.

##### 어떻게?

> Retrievals reflect the results of the most recently *completed* update operations holding upon their onset

가장 최근 성공적으로 업데이트된 수행내역을 따로 복사해서 저장해두고 그 부분에서 읽기 작업을 수행하기 때문이다.



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
