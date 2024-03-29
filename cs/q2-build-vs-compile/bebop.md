# 빌드 vs 컴파일

## 컴파일

컴파일이란 소스코드(=원시코드)를 또 다른 타겟코드(=목적코드)로 변환하는 과정을 의미한다.  
즉, 소스코드를 반드시 기계가 이해할 수 있는 기계어로 변환하는 것만이 컴파일을 의미하는 것은 아니다.

하지만 대부분의 경우 소스코드를 기계가 이해할 수 있는 기계어(binary code)로 변환하는 과정을 컴파일이라 칭한다.

Java의 경우 컴파일 과정을 거친 소스코드(`.java`)는 JVM이 이해할 수 있는 바이트 코드(`.class`, 반 기계어)로 변환한다.  
JIT 컴파일러는 바이트 코드를 네이티브 코드로 변경한다.

## 빌드

빌드는 소소코드를 실행 가능한 산출물로 만드는 일련의 과정 전체를 의미한다. (task의 집합)  
따라서 컴파일 과정은 빌드의 일부분이 된다.

대표적인 빌드 도구로는 Gradle, Maven, Ant가 있다.

빌드 도구의 대표적인 기능은 다음과 같다.

- 컴파일
- 컴포넌트 패키징 (jar와 같은 배포 가능한 상태로 만드는 작업)
- 파일 조작
- 테스트
- 배포



> 참고

[https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EC%97%90%EC%84%9C-%EC%8B%A4%ED%96%89%EA%B9%8C%EC%A7%80-1/](https://homoefficio.github.io/2019/01/31/Back-to-the-Essence-Java-컴파일에서-실행까지-1/)

https://bloofer.tistory.com/21