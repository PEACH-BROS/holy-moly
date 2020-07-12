# AOP란

Spring의 핵심 기능 중 하나인 **DI가 모듈 간 결합도를 낮춰준다**면, **AOP는 애플리케이션 전체에 걸쳐 사용되는 기능을 재사용하도록 지원한다.**  
즉, 개발자가 핵심 비즈니스 로직 외의 반복되는 **부가 기능**들에 대해 신경쓰지 않게 하도록 도와준다.

<br>

## AOP(Aspect-Oriented Programming) : 프로젝트 구조를 바라보는 관점을 바꿔보자



![image-20200712170003882](/Users/yebin/Library/Application Support/typora-user-images/image-20200712170003882.png)

각 Service의 핵심 기능에서 바라보았을 때, Board, User, XXX 등 공통된 요소가 없다.

이런 관점에서는 각 Service의 코드를 각자 구현하고 있다.



이를 부가기능 관점에서 바라보면 이렇게 보인다.



![image](https://user-images.githubusercontent.com/19922698/87241765-4105ea80-c461-11ea-8dd8-889e0ce0ef48.png)



부가기능의 관점에서는 각 Service는 수행시간 측정을 나타내는 before, after 메소드가 공통된다.

**AOP는 여기서부터 시작된다.**



- OOP : 비즈니스 로직의 모듈화

  ​			모듈화의 핵심 단위는 비즈니스 로직

- AOP : 인프라 or 부가기능의 모듈화

  ​			ex. 로깅, 트랜잭션, 보안 등



OOP에서 공통 기능을 재사용하기 위해선 상속이나 위임을 사용한다.

하지만, 전체 어플리케이션 여기저기서 사용되는 부가기능들을 상속이나 위임으로 처리하기에는 깔끔하게 모듈화가 어렵다.



그래서 이 문제를 해결하기 위해 AOP가 등장한다.

- 어플리케이션 전체에 흩어진 공통 기능이 하나의 장소에서 관리된다.
- 다른 서비스 모듈들이 본인의 목적에만 충실하고, 그 외 사항들은 신경쓰지 않아도 된다.



<br>

## AOP 용어



#### 타겟 (Target)

`부가기능을 부여할 대상`을 말한다.

여기선 핵심 기능을 담당하는 getBoards 혹은 getUsers를 하는 Service들을 이야기한다.



#### 애스팩트 (Aspect) = 모듈!

객체지향 모듈을 object라고 부르는 것처럼 `부가기능 모듈`을 aspect라고 부른다.

aspect는 부가 기능을 정의한 **advice**와 advice를 어디에 적용할지 결정하는 **pointcut**을 함께 갖고 있다.



#### 어드바이스 (Advice)

실질적으로 부가기능을 담은 `부가기능 구현체`를 말한다.

타겟 오브젝트에 종속되지 않기 때문에 순수하게 부가기능에만 집중할 수 있다.

종류에는 `@Before`, `@AfterReturning`, `@AfterThrowing`, `@After`, `@Around` 등이 있다.



#### 조인포인트 (JoinPoint)

`advice가 적용될 수 있는 위치`를 말한다.

다른 AOP 프레임워크와 달리, Spring에서는 메소드 조인포인트만 제공하고 있다.



#### 포인트컷 (PointCut)

joinpoint의 부분집합으로, `실제 advice가 적용되는 joinpoint`를 나타낸다.

즉, advice를 적용할 joinpoint를 선별하는 기능을 정의한 모듈이다.



## 프록시 (Proxy)

target을 감싸서 target의 요청을 대신 받아주는 wrapping 오브젝트이다. (target의 Proxy이다.)

**❗️ Proxy는 핵심 기능의 실행은 다른 객체에 위임하고, 부가 기능의 처리를 담당한다.**

![image](https://user-images.githubusercontent.com/19922698/87242518-66e2bd80-c468-11ea-8769-78e99ea155dd.png)

![image](https://user-images.githubusercontent.com/19922698/87245267-0a3ecd00-c47f-11ea-8080-5c159f5ca7ac.png)



### Proxy 객체의 처리 순서

1. Proxy 호출
2. Proxy 객체가 부가기능 처리 (pre-processing)
3. Proxy 처리 메소드가 실제 구현 함수에게 실행 위임
4. Proxy 객체가 남은 부가기능 처리 (post-processing)



### 스프링에서는 왜 Proxy를 사용해 AOP를 구현할까?

Proxy 객체는 런타임 시점에 생성이 가능하다.

컴파일 타임에 부가기능 코드를 관리할 필요가 없다는 것은 개발자가 핵심 비즈니스 로직에 집중할 수 있게 해준다.







> **참고**
>
> https://jojoldu.tistory.com/69?category=635883   
> https://tram-devlog.tistory.com/entry/Spring-AOP-weaving-proxy  
> 스프링 5 프로그래밍 입문 - 최범균 저 (Chapter 7. AOP)

