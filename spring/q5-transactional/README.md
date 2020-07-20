# @Transactional 이란?

[엘리의 @Transactional](elly-transactional.md)

[비밥의 @Transactional](bebop.md)

[스티치의 @Transactional](stitch.md)

[코일의 @Transactional](coyle.md)

---

## 트랜잭션의 성질 - ACID

**:star: 원자성(Atomicity) :star:**

 \- 한 트랜잭션 내에서 실행한 작업들은 하나로 간주한다. 즉, 모두 성공 또는 모두 실패. 

**▶ 일관성(Consistency)**

 \- 트랜잭션은 일관성 있는 데이타베이스 상태를 유지한다. (data integrity 만족, null 제약조건 등)

**▶ 격리성(Isolation)**

 \- 동시에 실행되는 트랜잭션들이 서로 영향을 미치지 않도록 격리해야한다.

**▶ 지속성(Durability)**

 \- 트랜잭션을 성공적으로 마치면 결과가 항상 저장되어야 한다.

<br/>

## @Transactional

> `@Transactional`이 적용된 클래스 또는 메소드에 **트랜잭션 기능이 적용된 프록시 객체**가 생성된다. *(AOP)*

❗️ 메서드에 선언된 `@Transactional`의 설정이 가장 우선시 되기 때문에 공통적인 규칙은 인터페이스, 클래스 등에 적용하고 특별한 설정은 메서드에 적용할 수 있다.

<br/>

## @Transactional 옵션

| 속성          | 설명                                                         | 사용 예 (디폴트)                                     |
| ------------- | ------------------------------------------------------------ | ---------------------------------------------------- |
| isolation     | 트랜잭션 고립 레벨 설정                                      | @Transactional(isolation = Isolation.DEFAULT)        |
| propagation   | 트랜잭션 전파 규칙 정의                                      | @Transactional(propagation = Propagation.REQUIRED)   |
| readOnly      | 해당 트랜잭션을 읽기 전용 모드로 처리                        | @Transactional(readOnly = false)                     |
| rollbackFor   | 정의된 Exception에 대해 rollback 수행                        | @Transactional(rollbackFor = Exception.class)        |
| noRollbackFor | 정의된 Exception에 대해서는 rollback을 수행하지 않음         | @Transactional(noRollbackFor = Exception.class)      |
| timeout       | 지종한 시간 내에 해당 메소드 수행이 완료되지 않을 경우 rollback 수행. | @Transactional(timeout = -1) //-1일 경우, no timeout |



**isolation** : 트랜잭션 고립 레벨 설정

![image](https://user-images.githubusercontent.com/19922698/87809863-0b189a00-c897-11ea-8163-539f28366e7d.png)

<br>

**propagation** : 전파 옵션 설정

![image](https://user-images.githubusercontent.com/19922698/87810346-ce996e00-c897-11ea-90ca-0b460e44c18f.png)



<br/>

### @Transactional 모드

**Proxy**와 **AspectJ**가 있는데 Proxy가 Default로 설정되어있다.

Proxy 모드는 다음과 같은 경우 동작하지 않는다.

- 반드시 public 메서드에 적용되어야 한다.
  - Protected, Private 메서드에서는 선언되어도 에러가 발생하지는 않지만, 동작하지도 않는다.
  - Non-Public 메서드에 적용하고 싶으면 AspectJ 모드를 고려해야 한다.
- `@Transactional`이 적용되지 않은 Public 메서드에서 `@Transactional`이 적용된 Public 메서드를 호출할 경우 트랜잭션이 동작하지 않는다.





<br/>

> **탐구**

### 🤔 테스트 메소드에 @Transactional을 붙이는 게 안 좋을까?

조심해서 사용할 것.

테스트 코드에서 @Transactional을 안 붙인 상태에서 lazy loading으로 값을 꺼내버리면 `LazyInitializationException`이 발생한다. 

테스트 코드에서 lazy loading 작업을 진행할 session이 없기 때문이다.



이럴 경우, 추가적으로 조인 쿼리를 만들어서 해결할 수 있다.



다만, @Transactional을 테스트 메소드에 적용하면 롤백이 가능하다. 개꿀!