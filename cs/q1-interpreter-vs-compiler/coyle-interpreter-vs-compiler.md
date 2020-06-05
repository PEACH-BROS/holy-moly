compiler / interpreter 차이
컴파일언어와 인터프린터 언어의 가장 큰 차이점은 컴파일 시점입니다. (런타임 전에 컴파일을 하는지 안하는지)
Java는 런타임 이전 컴파일을 통해 기계어가 아닌 바이트어로 변경되지만 컴파일 시점 때문에 컴파일 언어라고 부릅니다.

## interpreter 언어란?

  원시코드를 기계어로 변환하는 과정 없이 한 줄 한 줄 해석하여 바로 명령어를 실행하는 언어입니다. 대표적으로 R, Ruby가 있습니다. 빌드 시간이 없습니다.
 그러나 런타임에는 한 줄 한 줄 읽어나가며 실행하기 때문에 속도가 비교적 느립니다.
 
  소스 프로그램을 한번에 기계어로 변환시키는 컴파일러와는 달리 프로그램을 한 단계씩 기계어로 해석하여 실행하는 ‘언어처리 프로그램’입니다. 
 줄 단위로 번역, 실행되기 때문에 **시분할 시스템에 유용**하며 **원시 프로그램의 변화에 대한 반응**이 빠릅니다. 한 단계씩 테스트와 수정을 하면서 진행시켜 나가는
 **대화형 언어**에 적합합니다.

## 빌드 시간은 무엇이죠?
 
  소스 파일을 실행 파일로 변경하는 과정입니다. 인터프리터 언어는 빌드 과정이 없고, 컴파일러는 빌드 과정을 통하여 변경됩니다.

## Compiler 언어란 ?
 
  빌드 과정을 거쳐 실행됩니다. 소스 파일들을 모두 기계어로 변환하여 기계어 코드를 실행합니다. 모든 소스 파일을 변경해놓기 때문에 빌드 과정에서는 인터프리터에 비해
 시간을 소요하나 런타임에서는 모든 명령이 기계어로 변경되어있기 때문에 런타임에서는 인터프리터 언어에 비해 빠릅니다.
 
## Java는 인터프리터 언어인가 컴파일러 언어인가?
  Java는 Java 바이트 코드로 컴파일 한 뒤 자바 인터프리터, Run Time System이 이식된 모든 플랫폼에서 자바 바이트 코드를 직접 실행합니다.
 javac로 컴파일 하여 중간 언어(클래스 파일)을 메모리에 올린 뒤 한 줄씩 자바 인터프리터가 번역하므로 **컴파일 언어**이면서 **인터프리터 언어**이다.

참고 
 - https://postitforhooney.tistory.com/entry/InterpreterCompiler-%EC%9D%B8%ED%84%B0%ED%94%84%EB%A6%AC%ED%84%B0%EC%99%80-%EC%BB%B4%ED%8C%8C%EC%9D%BC%EB%9F%AC%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90
 - http://ojc.asia/bbs/board.php?bo_table=LecJava&wr_id=561