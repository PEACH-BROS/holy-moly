# 프로세스와 쓰레드에 대해 설명해주세요.

[비밥의 프로세스 vs 쓰레드](bebop.md)

[엘리의 프로세스 vs 쓰레드](elly-process-vs-thread.md)

[코일의 프로세스 vs 쓰레드](coyle-process-vs-thread.md)

### 프로세스란 ? 

프로세스는 메모리에 올라가서 실행중인 작업을 의미하는데 이는 운영체제로부터 사용할 자원을 할당받는 것을 말한다.

프로세스가 다른 프로세스의 자원에 접근하려면 프로세스간의 통신을 해야하기 때문에 그 비용이 꽤나 크다.

![](https://camo.githubusercontent.com/c0dccd0a2ddcd699ca03a320f705ce6f24de350a/68747470733a2f2f676d6c776a64393430352e6769746875622e696f2f696d616765732f6f732d70726f636573732d616e642d7468726561642f70726f636573732e706e67)



프로세스는 Code, Data, Stack, Heap (각각 **독립된 메모리** 영역)을 할당받는다. 

- Code : 프로그램의 실제 코드 저장
- Data : 프로세스가 실행될 때 정의된 전역 변수, static 변수 저장
- Stack : 함수에서 다른 함수를 실행하는 등의 서브루틴 정보 저장(❗️재귀와 스택이 관련 있는 이유)
- Heap : 런타임 중 동적으로 할당받는 변수들 저장 (함수 내에서 선언된 변수 등)



### 프로세스간의 통신

각 프로세스는 별도의 주소 공간에서 실행되며, 한 프로세스는 다른 프로세스의 변수나 자료구조에 접근할 수 없다.

한 프로세스가 다른 프로세스의 자원에 접근하려면 프로세스 간 통신 (IPC, Inter-Process-Communication)을 사용해야 한다. (ex. 파이프, 파일, 소캣)



### 쓰레드란 ?

쓰레드는 프로세스 내에서 실행되는 흐름의 단위를 의미한다.

프로세스는 최소한 하나의 쓰레드를 가진다.

쓰레드를 사용하는 이유는 프로세스간의 통신의 비용이 크기 때문에 쓰레드를 사용한다.



### 쓰레드의 구조 

- 프로세스 내에서 **Stack**만 따로 할당받고, Code, Data, Heap 영역 (주소 공간이나 자원)은 공유한다.



### 프로세스 vs 쓰레드

그러나, 쓰레드는 동일 프로세스 내의 쓰레드들의 경우, 부모 프로세스의 메모리를 공유한다. 따라서 하나 이상의 쓰레드가 동일한 자원을 동시에 접근할 수 있기 때문에 여러 동기화 이슈들을 신경써줘야한다. 또한, 그 안에서 각 쓰레드들은 자신만의 리소스 풀(스택)을 가진다.



### 멀티 프로세싱이란? 

- 하나의 응용프로그램을 여러 개의 프로세스로 구성해 각 프로세스가 하나의 작업(태스크)를 처리하도록 하는 것이다.
  - Ex) 구글 크롬탭

**멀티 프로세싱의 장점**

👍  여러 개의 자식 프로세스 중 하나에 문제가 발생하면, 그 자식 프로세스만 죽는다. (다른 프로세스에 영향이 없다.)

**멀티 프로세싱의 단점**

👎 Context Switching 오버헤드

👎 프로세스 간 어렵고 복잡한 통신 기법 IPC



#### 멀티 스레드란? 

하나의 응용프로그램을 여러 개의 스레드로 구성하고, 각 스레드가 하나의 작업을 처리하도록 하는 것이다.

예를 들어 하나의 프로그램에서 하나의 쓰레드는 DB에 쿼리를 보내고 결과를 받는 동안, 다른 하나의 쓰레드는 유저에게 프로그레스 바를 보여줄 수 있다.

### 



##### 멀티 스레드의 장점

👍 빠른 Context Switching

👍 간단한 통신 방법으로 인한 프로그램 응답 시간 단축 == 사용성 

##### 멀티 스레드의 단점

👎 주의 깊은 설계가 필요하다.

👎 디버깅이 까다롭다.

👎 자원 공유로 인한 동기화 문제가 발생한다.

👎 하나의 스레드에 문제가 발생하면 전체 프로세스가 영향을 받는다.
