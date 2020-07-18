# @Transactional

### Transaction이란?

DB 상태를 변환시키는 하나의 작업 단위. 트랜잭션 단위로 **커밋**과 **롤백**이 진행된다.

---

### @Transactional?

**스프링에서 제공**하는 트랜잭션 처리 방법 중 하나.

이렇게 명시적으로 적어주는 것을 **선언적 트랜잭션**이라고 한다.

> `@Transactional`이 적용된 클래스 또는 메소드에 **트랜잭션 기능이 적용된 프록시 객체**가 생성된다. *(AOP)*

- 보통 CREATE, UPDATE, DELETE 작업에는 건다. **(❗️rollback이 가능해야 하기 때문에)**

- SELECT는 선택적이다. **(❗️rollback을 할 필요가 없기 때문!)**
  - READ할 때 읽기 락이 걸린다. 한 번 읽고 나서 쭉 작업을 진행하는데 그동안 계속 Transaction을 잡을 필요가(=읽기 락을 잡을 필요가) 없을 때가 있다.

---

### 제공하는 옵션들

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

`REQUIRED`(디폴트) : 부모 트랜잭션 내에서 실행하며, 부모 트랜잭션이 없을 경우 새로운 트랜잭션을 생성

`REQUIRES_NEW` : 부모 트랜잭션을 무시하고 무조건 새로운 트랜잭션을 생성

`SUPPORT` : 부모 트랜잭션 내에서 실행되며 부모 트랜잭션이 없을 경우 nontransactionally로 실행

`MANDATORY` : 부모 트랜잭션 내에서 실행되며 부모 트랜잭션이 없을 경우 예외 발생

`NOT_SUPPORT` : nontransactionally로 실행하며 부모 트랜잭션 내에서 실행될 경우 일시 정지

`NEVER` : nontransactionally로 실행되며 부모 트랜잭션이 존재하면 예외 발생

`NESTED` : 해당 메소드가 부모 트랜잭션에서 진행될 경우 별개로 커밋되거나 롤백될 수 있음. 둘러싼 트랜잭션이 없을 경우 `REQUIRED`와 동일하게 작동

<br>



> 참고
>
> https://goddaehee.tistory.com/167  
> https://taetaetae.github.io/2017/01/08/transactional-setting-and-property/  
>