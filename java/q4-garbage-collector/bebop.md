# Garbage Collector

## Reachbaility

GC는 JVM 힙영역에서 사용중이지 않은 객체를 식별해서 삭제하는 프로세스이다.

이때 사용하지 않는 객체를 판단할때 'Reachability' 개념을 사용한다.

| Reachability | 접근 가능 | 접근 불가능 |
| ------------ | --------- | ----------- |
|              | Reachable | Unreachable |

당연히 접근 불가능한 객체를 식별하여 GC의 대상으로 본다.

접근 불가능한 객체는 유효한 참조를 가지고 있는지를 가지고 판단하는데 최상위에는 GC Root 가 있으며 항상 유효한 최초의 객체이다.

**GC Root가 되는 유효한 참조**는 다음과 같다.

1. Java 스택, 메소드 실행시 사용되는 지역 변수, 파라미터에 의한 참조
2. JNI가 생성한 객체 참조
3. 메소드 영역의 정적 변수 참조

그리고 **GC Root는 아니지만 유효한 참조**로 **힙 내 다른 객체에 의한 참조**가 있다.

## Mark & Sweep

Mark는 **참조되고 있는 객체를 Marking**하는 행위를 의미하고

Sweep은 **참조되지 않은 객체를 Sweep**하는 것을 의미한다.

Mark & Sweep 과정을 거치면서 구멍 뚫린 메모리 공간을 효율적으로 사용하기 위해 **메모리를 다시 정렬하는 과정을 Compact**라고 하는데 이 과정은 GC 효율의 시작이자 끝인 **Stop the World를 유발**한다.

이하 M-S-C

## Stop the World (이하 STW)

Stop the World는 GC가 실행될때 GC 스레드를 제외한 다른 스레드를 모두 멈추는 행위를 말한다.

어떠한 GC 알고리즘을 사용하더라도 Stop the World 는 발생한다.

### Generational Garbage Collection 방식

자바의 Heap 메모리는 Young generation과 Old Generation 영역으로 나뉘어 관리된다.

Young : 새로운 객체들이 위치하며 Eden + 2개의 survival 영역으로 구성되어 있다.  
이 영역에서의 GC는 Minor GC라 한다.

Old : Young 영역에서 오래 살아남은 객체들이 이동하는 곳이다.  
이 영역에서의 GC는 Major GC라 한다.

## GC 과정

1. 새로운 객체가 Eden에 할당

   - Eden 이 가득차면서 Minor GC 실행

   - 살아남은 객체는 Survival 영역(S0)으로 이동
     - 다음 Minor GC 발생시 S0에 있던 객체들과 함께 S1 영역으로 이동

2. Young 영역에서 오래 살아남은 객체가 Old 영역으로 이동

   - 이를 Promotion 이라고 한다.

3. Promotion이 반복되어 Old 영역이 가득차면 Major GC 발생
