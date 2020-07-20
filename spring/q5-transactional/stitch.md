# `@Transactional` 이란

## Transaction이란

하나의 논리적 기능을 수행하기 위한 작업의 단위이다. 간단하게 말하자면 쪼갤 수 없는 작업의 최소 단위라 볼 수 있다.

### Transaction의 조건(ACID)

1. **원자성**(*atomicity*) - 하나의 트랜잭션은 더 이상 작게 쪼갤 수 없는 최소한의 업무 단위이다.
2. **일관성**(*consistency*) - 트랜잭션이 완료된 결과값은 일관성 있는 상태로 유지되어야 한다.
3. **고립성**(*Isolation*) - 트랜잭션을 수행하는 도중에 다른 연산 작업이 끼어들어서는 안 된다.
4. **지속성**(*durability*) - 성공적으로 수행된 트랜잭션은 영구적으로 반영되어야 한다.

<br/>

## Spring의 Transaction

### Transaction 설정 방법

스프링에서는 **xml 설정 파일** 또는 **어노테이션 방식**으로 선언적으로 트랜잭션을 사용할 수 있다.

위의 방식을 <u>선언적 트랜잭션</u>이라 부르고, 이 중 `@Transactional` 어노테이션을 선언하여서 사용하는 방식이 일반적이다.



## `@Transactional` 

`@Transactional` 어노테이션은 클래스의 메서드 뿐 아니라 인터페이스, 클래스 선언에도 사용할 수 있다.

메서드에 선언된 `@Transactional`의 설정이 가장 우선시 되기 때문에 공통적인 규칙은 인터페이스, 클래스 등에 적용하고 특별한 설정은 메서드에 적용할 수 있다.



### `@Transactional` 속성

#### 격리레벨(*isolation*)

트랜잭션에서 일관성이 없는 데이터를 허용하도록 하는 수준을 말한다.

- DEFAULT - DB 설정, 기본 격리 수준(기본 설정)
- READ_UNCOMMITED(level 0) - 커밋되지 않은 데이터에 대한 읽기 허용
  - Dirty Read 문제 발생
- READ_COMMITED(level 1) - 커밋된 데이터에 대해 읽기 허용
  - Dirty Read 문제 방지
- REPEATABLE_READ(level 2) - 동일 필드에 대해 다중 접근 시 모두 동일한 결과를 보장
  - Non-Repeatable Read 문제 방지
- SERIALIZABLE - 가장 높은 격리, 성능 저하의 우려가 있음
  - Phantom Read 방지

##### 사용 방법

```java
@Transactional(isolation=Isolation.DEFAULT)
public void save(Entity e) {
    ...
}
```

#### 전파속성(*propagation*)

트랜잭션 도중 다른 트랜잭션을 호출(실행)하는 상황에 선택할 수 있는 옵션이다.

- REQUIRED - 트랜잭션이 있으면 그 상황에서 실행, 없으면 새로운 트랜잭션 실행(기본 설정)
- SUPPORTS - 트랜잭션을 필요로 하지 않으나, 트랜잭션 상황에 있따면 포함되어서 실행
- REQUIRES_NEW - 대상은 자신만의 고유한 트랜잭션으로 실행
- MANDATORY - 작업은 반드시 특정한 트랜잭션이 존재한 상태에서만 가능
- NOT_SUPPORTED - 트랜잭션이 있는 경우에는 트랜잭션이 끝날 때까지 보류된 후 실행
- NEVER - 트랜잭션 상황에 실행되면 예외 발생
- NESTED - 기존에 트랜잭션이 있는 경우, 포함되어서 실행

```java
@Transactional(propagation=Propagation.REQUIRED)
public void save(Entity e) {
    ...
}
```



### `@Transactional` 모드

Proxy와 AspectJ가 있는데 Proxy가 Default로 설정되어있다.

Proxy 모드는 다음과 같은 경우 동작하지 않는다.

- 반드시 public 메서드에 적용되어야 한다.
  - Protected, Private 메서드에서는 선언되어도 에러가 발생하지는 않지만, 동작하지도 않는다.
  - Non-Public 메서드에 적용하고 싶으면 AspectJ 모드를 고려해야 한다.
- `@Transactional`이 적용되지 않은 Public 메서드에서 `@Transactional`이 적용된 Public 메서드를 호출할 경우 트랜잭션이 동작하지 않는다.