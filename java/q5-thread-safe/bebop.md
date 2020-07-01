# Multi Thread에 대해 설명해주세요.

## 1-1. Thread-safe란 무엇인가요?

멀티스레드 환경에서 공유된 자원을 여러개의 스레드가 동시에 접근 할 때 발생 할 수 있는 문제점을 해결하는 방법을 말한다.

발생할 수 있는 문제란 교착상태(데드락)와 기아상태를 말한다.

## 1-2. 그럼 자바에서 공유 자원에 대해 어떻게 thread-safe 하게 할까요?

공유자원에 접근하는 **메소드** 혹은 **메소드 내 코드 블럭 일부분**에 대해 Synchronized 키워드를 이용하여 lock을 걸어서 구현한다.

이렇게 하면 교착상태에 대한 문제점을 해결할 수 있다.

##### 교착상태 발생 조건

- 상호배제 : 프로세스들이 필요로 하는 자원에 대한 배타적인 통제권을 요구한다.
- 점유대기 : 프로세스가 할당된 자원을 가진 상태에서 다른 자원을 기다린다.
- 비선점 : 프로세스가 어떤 자원의 사용을 끝낼 때까지 그 자원을 뺏을 수 없다.
- 순환대기 : 각 프로세스는 순환적으로 다음 프로세스가 요구하는 자원을 가지고 있다.

> 참고

[https://velog.io/@pa324/%EC%9A%B4%EC%98%81%EC%B2%B4%EC%A0%9C-Thread%EC%8A%A4%EB%A0%88%EB%93%9C-rqk2kafwhw](https://velog.io/@pa324/운영체제-Thread스레드-rqk2kafwhw)

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

`volatile ` 예약어를 사용해서 가장 최근에 기록된 값을 읽어오도록 할수 있다.  
하지만 배타적 수행과는 상관이 없기 때문에 안절 실패의 오류가 발생할 수 있다.

`java.util.concurrent.atomic` 패키지의 atomic 객체들을 사용하면 배타적인 수행과 안전한 읽기가 가능하다.  
또한 `synchronized` 보다 성능상 우수하다.

또한 동시성 컬렉션에 저장하는 방식도 있다.

Concurrent api를 사용하는 방법이 있다.  
동기화를 위한 잠금 단위가 훨씬 세밀하기 때문에 성능상 더 좋다.

특히 읽기 작업(`get()`)의 경우 locking 하는 과정이 필요가 없다.

##### 어떻게?

> Retrievals reflect the results of the most recently *completed* update operations holding upon their onset

가장 최근 성공적으로 업데이트된 수행내역을 따로 복사해서 저장해두고 그 부분에서 읽기 작업을 수행하기 때문이다.

> 참고

https://stackoverflow.com/questions/1291836/concurrenthashmap-vs-synchronized-hashmap

https://docs.oracle.com/javase/8/docs/api/java/util/concurrent/ConcurrentHashMap.html