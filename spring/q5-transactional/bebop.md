# @Transactional 이란?

데이터베이스에서 의미하는 Transaction을 의미하며 크게는 Class에 작게는 Method에 적용할 수 있는 어노테이션이다.

성공 여부에 따라 rollback을 할수 있으며 rollback 정책과 Transaction Isolation level 등을 설정하고 사용 할 수 있다.

Class에 Transaction을 적용하면 기본적으로 모든 Method에 적용된다.  
그리고 해당 클래스를 상속받는 Class에도 동일하게 적용이 된다.

이러한 @Transaction 어노테이션을 사용한다면 메소드를 실행할 때 Proxy 객체를 이용해서 메소드 시작전에 Transaction의 시작을 메소드가 끝날 때 Trasaction의 종료를 알아서 해준다.

## 옵션

| Modifier and Type        | Optional Element and Description                             |
| :----------------------- | :----------------------------------------------------------- |
| `isolation`              | 트랜잭션의 고립레벨을 설정한다.                              |
| `noRollbackFor`          | 특정 익셉션들을 클래스로 지정하고 익셉션이 발생하여도 롤백을 진행하지 않는다. |
| `noRollbackForClassName` | 특정 익셉션들을 클래스 이름(풀패키지 경로)로 지정하고 익셉션이 발생하여도 롤백을 진행하지 않는다. |
| `propagation`            | 트랜잭션의 전파 수준을 설정한다.                             |
| `readOnly`               | true로 설정하면 읽기(READ) 전용 트랜잭션이 된다.             |
| `rollbackFor`            | 특정 익셉션들을 클래스로 지정하고 익셉션이 발생하면 롤백을 진행한다. |
| `rollbackForClassName`   | 특정 익셉션들을 클래스 이름(풀패키지 경로)로 지정하고 익셉션이 발생하면 롤백을 진행한다. |
| `timeout`                | 트랜잭션이 진행됨에 있어 timeout 값을 설정한다.              |
| `transactionManager`     | 트랜잭션 매니저를 명시적으로 지정할 수 있다.  <br />Bean의 이름을 지정해 주어야 한다. |
| `value`                  | `transactionManager` 와 동일하다.                            |

isolation 설정도 중요하지만 이전에 isolation level에 대해 다룬적이 있기때문에 다루지 않겠다.

### Propagation

기본 전파 정책은 [`Propagation.REQUIRED`](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Propagation.html#REQUIRED) 이다

#### REQUIRED

부모 트랜잭션 내에서 실행하며 부모 트랜잭션이 없을 경우 새로운 트랜잭션을 생성한다.

#### SUPPORTS

이미 시작된 트랜잭션이 있으면 참여하고 그렇지 않으면 트랜잭션 없이 진행하게 만든다. 

#### **REQUIRES_NEW**

부모 트랜잭션을 무시하고 무조건 새로운 트랜잭션이 생성한다.

#### MANDATORY

이미 시작된 트랜잭션이 있으면 참여한다.   
하지만 **트랜잭션이 시작된 것이 없으면 새로 시작하는 대신 예외를 발생**시킨다. 

####  REQUIRES_NEW

항상 새로운 트랜잭션을 시작한다.

이미 진행 중인 트랜잭션이 있으면 종료될때까지 기다린 후 실행한다.

####  NOT_SUPPORTED

트랜잭션을 사용하지 않게 한다.

이미 진행 중인 트랜잭션이 있으면 기다린 후 실행한다.

#### **NEVER**

트랜잭션을 사용하지 않도록 강제한다.

이미 진행 중인 트랜잭션도 존재하면 안된다 있다면 예외를 발생시킨다.

#### **NESTED**

이미 진행중인 트랜잭션이 있으면 중첩 트랜잭션을 시작한다.

*중첩 트랜잭션 :  트랜잭션 안에 다시 트랜잭션을 만드는 것*



> 참고

[공식 문서 링크](https://docs.spring.io/spring-framework/docs/current/javadoc-api/org/springframework/transaction/annotation/Transactional.html)

https://goddaehee.tistory.com/167

