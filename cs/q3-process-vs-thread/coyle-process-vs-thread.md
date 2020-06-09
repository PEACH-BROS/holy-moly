### 프로세스와 쓰레드의 차이를 말해!

  프로세스는 프로그램이 실행되는 단위이다. 하나의 프로그램이 실행되는 경우 메모리 상의 하나의 프로세스가 해당 프로그램을 실행합니다.
 멀티 프로세서를 지원하는 CPU를 가진 경우, 여러 개의 프로세스를 실행시킬 수 있어 여러가지 프로그램을 동시에 실행하는 멀티테스킹이 가능하다.

  기본적으로 하나의 프로세스는 하나의 쓰레드를 가지고 있으나, 멀티코어 CPU를 가진 컴퓨터의 경우 하나의 프로세스가 두 개 이상의 쓰레드를 가지고 프로그램을 실행할 수 있다.
 예를 들어 하나의 프로그램에서 하나의 쓰레드는 DB에 쿼리를 보내고 결과를 받는 동안, 다른 하나의 쓰레드는 유저에게 프로그레스 바를 보여줄 수 있습니다.

### 프로세스와 쓰레드의 차이를 말하라니깐 무슨 프로세스와 쓰레드가 뭔지 설명하고 있어!!!!!!

  프로세스와 쓰레드의 가장 큰 차이점은 **자원의 공유 여부** 이다. 각각의 프로세스는 독립된 메모리를 가지고 있어, 하나의 프로세스나 다른 프로세스의 자원을 직접 접근할 수 없다. 그러므로 **IPC**, **Shared memory** 등 프로세스 간의 통신을 통해 필요 데이터를 주고 받을 수 있다.

 그러나, 쓰레드는 동일 프로세스 내의 쓰레드들의 경우, 부모 프로세스의 메모리를 공유한다. 따라서 하나 이상의 쓰레드가 동일한 자원을 동시에 접근할 수 있기 때문에 여러 동기화 이슈들을 신경써줘야한다. 또한, 그 안에서 각 쓰레드들은 자신만의 리소스 풀을 가진다. 

### Process 더 말하기

스케줄링의 대상이 되는 작업(task)라는 용어와 거의 같은 의미로 쓰인다. 결국 **실행 중인** 프로그램이다.

프로세스의 구성

![](https://smjeon.dev/assets/img/os/process.png)

1. Program code : text
2. Glbal data : data
3. Temporary data : stack
   1. local variable, function parameter, return address
4. Heap
   1. 실행하는 동안 동적으로 할당되는 메모리 영역
5. PC를 포함한 프로세서 레지스터의 값들

### Thread 더 말하기

쓰레드는 프로세스 내에서 실행되는 흐름의 단위. 일반적으로 한 프로그램은 하나의 스레드를 가지지만, 프로그램 환경에 따라 둘 이상의 스레드를 동시에 실행할 수 있다.

 

![](https://smjeon.dev/assets/img/os/thread.png)

Code, Data, Heap 영역은 공유한다.오직 **Stack만 공유하지 않는다.**



#### Thread 사용의 장점

1. 자원의 효율적 사용
2. 사용자의 응답성
3. 프로세스 간 전환 **보다** 문맥 전환에 비용이 적다

#### Thread 사용의 단점

1. 자원 동시접근 처리
2. 디버깅 어렵...!

- https://smjeon.dev/etc/process-thread/
- [https://junyongs.wordpress.com/2014/08/05/process-vs-thread-%ED%94%84%EB%A1%9C%EC%84%B8%EC%8A%A4-vs-%EC%93%B0%EB%A0%88%EB%93%9C/](https://junyongs.wordpress.com/2014/08/05/process-vs-thread-프로세스-vs-쓰레드/)
- https://gmlwjd9405.github.io/2018/09/14/process-vs-thread.html