# JVM



## JVM의 역할?

자바 프로그램을 실행시킨다.

- 실행될 클래스 파일(`.class`)을 메모리에 로드한다. (클래스 로드)
- 클래스 로드가 끝난 후 main메소드를 찾아 필요한 변수들을 스택에 쌓는다.
- 다음 라인을 실행하면서 함수 호출, 객체 할당 등 상황에 맞는 작업을 수행한다.



## JVM의 구조

![img](https://media.vlpt.us/images/litien/post/a65da4a8-5dc4-422b-b91e-cafeafe464d3/image.png)

크게 Class Loader, Execution Engine, Garbage Collector 그리고 Runtime Data Area 4가지 영역으로 나뉜다.



**Class Loader**

 JVM 내부로 클래스를 로드하고, 링크를 통해 배치한다.  
런타임 시 동적으로 클래스를 로드한다.



**Execution Engine**

클래스 로더를 통해 Runtime Data Area에 배치된 **바이트 코드**(JVM이 이해할 수 있는 코드)를 **명령어 단위로** 실행한다.  
❗️**인터프리터 방식**(바이트코드를 한 줄씩 읽고 해석)과 **JIT 컴파일 방식**(자주 쓰이는 코드들을 미리 기계어로 변환시켜둠)을 혼합해 해석한다.



**Garbage Collector**

메모리 관리 기능을 담당하는 친구!  
Runtime Data Area의 힙 영역을 관리한다.  
애플리케이션이 생성한 객체의 생존 여부를 판단해 더 이상 사용되지 않는 객체는 해제한다.



### Runtime Data Area

**메소드 영역**

바이트 코드가 저장되는 공간  
클래스 로더에 의해 로딩된 클래스, 메소드, static 변수, 전역변수가 저장된다.  
GC 영역 밖  
프로그램 시작부터 종료 시까지 메모리에 남는다.





**힙 영역** 

런타임 시 결정되는 참조 타입 변수의 값이 저장되는 공간  
new 연산자를 통해 동적으로 생성되는 인스턴스가 저장되는 공간  
GC의 대상이 되는 영역!  



여기까지는 모든 스레드가 공유한다.

---

여기서부터는 스레드마다 한 개씩 생성된다.



**스택 영역**

컴파일 시 결정되는 원시 타입 변수들이 저장되는 공간  
지역변수, 파라미터, 리턴값, 참조변수에 대한 주소가 저장된다.  
First-In-Last-Out 구조로 저장  
주로 금방 사용되고 지워질 친구들이 저장된다.





**PC 레지스터**

JVM이 수행할 명령어의 주소를 저장한다.  
스레드가 시작될 때마다 생성된다.





**네이티브 메소드 스택**

바이트 코드가 아닌, 기계어로 작성된 코드를 실행하는 공간  
자바 외의 다른 언어로 작성된 코드를 위한 공간





## 스택 프레임?

현재 실행중인 메소드가 스택의 최상단에 올라와 있고,

이미 실행한 메소드들이 쌓여있는 구조





> 참고
>
> [https://velog.io/@litien/JVM-%EA%B5%AC%EC%A1%B0](https://velog.io/@litien/JVM-구조)  
> https://gbsb.tistory.com/2  
> https://yaboong.github.io/java/2018/05/26/java-memory-management/  
> https://minwan1.github.io/2018/06/06/2018-06-06-Java,JVM/

