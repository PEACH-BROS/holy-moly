# 인터프리터 언어 vs 컴파일 언어

## 인터프리터 언어

인터프리터 언어는 인터프리터가 번역할 소스 코드를 받아  
한 줄씩 번역해서 실행시킨다.

따라서 인터프리터만 설치가 되어 있다면 서로 다른 시스템에서도 실행시킬수 있다.

다만 매번 번역하는 과정이 존재하기 때문에 바이너리 코드를 실행시키는 컴파일 언어보다 느릴수 밖에 없다.

대표적으로 Javascript가 있다.

## 컴파일 언어

컴파일이란 한 언어를 다른 언어로 번역하는 과정을 의미한다.  
따라서 컴파일을 했다는 것이 항상 바이너리 코드로 소스코드를 번역했다는 것을 의미하지는 않는다.

하지만 대부분의 경우 컴파일 과정을 거쳐 기계가 이해하기 쉬운 기계어로 바꾸는 것을 의미한다.

컴파일러가 기계어로 언어를 바꾸어 주기 때문에  
컴파일 언어가 인터프리터 언어보다 프로그램의 속도가 빠르다.  

하지만 명령어 셋이 서로 다른 시스템(ex. x64, x86)에서 프로그램이 동작하지 않을 수 있다.

대표적으로 C가 있다.

## Java는 인터프리터 언어? 컴파일 언어?

Java는 컴파일러와 인터프리터를 모두 가지고 있는 하이브리드 언어이다.

Java 컴파일러가 `.java` 파일을 `.class`파일(바이트 코드)로 번역을 하고  
Java 인터프리터가 실행시간에 바이트 코드를 한 줄씩 번역하여 실행한다.  
또한 인터프리터의 단점을 극복하기 위해 JIT 컴파일러를 통해  
자주 사용되는 바이트 코드를 기계어로 컴파일 하여 사용한다. (캐싱)

---

> 참고

https://imasoftwareengineer.tistory.com/43