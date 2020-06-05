### 컴파일 ? 
소스 코드를 기계어로 변환하는 과정이며 컴파일러를 통해 변환되며 목적 코드가 생성된다.

자바의 경우 JVM에서 실행 가능한 바이트 코드의 클래스 파일(.class)이 생성된다.

### 링크 ? 
링크는 클래스 A에서 클래스 B를 사용하는 경우 서로를 이어주는 작업이다.

메모리 상에는 클래스 A와 클래스 B는 서로 어디에 위치하는 지 모른다.

이를 링크가 해결해준다.

여러 개로 분리된 소스코드들을 컴파일한 최종 실행 가능한 파일을 만들기 위해 이어주는 작업이라고 보면된다.

정적 링크와 동적 링크가 존재하는데 정적 링크는 컴파일된 소스파일을 연결해서 실행가능한 파일로 만드는 것이고
동적 링크는 프로그램이 실행 도중일 때 외부의 코드를 읽어오는 것이다.

JVM이 프로그램 실행 도중 필요한 클래스들을 클래스패스에 로드해주는데 이는 동적 링크에 해당한당.

### 빌드 ?
소스코드를 실행 가능한 코드로 만드는 일련의 과정을 빌드라고 하며 컴파일은 빌드에 속해있다.

### Java에서의 빌드 과정

> JVM(Java Virtual Machine)이 OS와 프로그램의 사이에서 기계어로 해석해주는 역할을 하기 때문에 OS에 종속적이지 않고 어느 컴퓨터에서도 .java파일을 기계어로 만들어낼 수 있다. 

1. java 컴파일러에 의해 javac 명령어를 사용해 .class 파일로 만든다. (.class파일은 아직 컴퓨터가 읽을 수 없는 자바 바이트 코드 = 반기계어)
2. .class파일은 클래스 로더에 의해 JVM 내로 로드된다.
3. 실행 엔진(JIT, 인터프리터)에 의해 기계어로 변환되어 메모리 상(Runtime Data Area)에 배치된다.
  - 인터프리터 : 한줄 한줄 영차 영차 너무 느려;
  - JIT : 인터프리터의 속도를 해결하기 위해 한줄 한줄 인터프리팅하지 않고 바이트 코드 전체를 해석해서 실행한다. 
    -  JIT는 해석된 코드를 캐시에 보관하므로 한 번 해석된 이후에는 빠르게 수행한다. 
    - 그러나 한번 실행되는 코드는 인터프리터가 훨씬 빠르므로 한번만 실행하면 되는 코드는 인터프리터가 훨씬 낫다.
    
# 내가 말한 컴파일과 빌드의 차이점은 무엇인가 ?

컴파일은 소스 코드를 바이너리 코드(기계어)로 바꾸는 일련의 과정이다.

~~빌드는 컴파일된 파일들이 메모리에 적재되는 과정을 말한다.~~
 - 빌드는 소스 코드를 실행가능한 코드로 만드는 과정

### 참고 
 - https://freezboi.tistory.com/39
 - https://aljjabaegi.tistory.com/387