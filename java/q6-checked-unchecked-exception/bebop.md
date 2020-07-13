# Checked Exception VS Unchecked Exception

자바는 예외상황을 처리하기 위해 Throwable을 사용한다.  
정확히는 Throwable을 상속받는 두가지 구현체를 더 세분화 하여 사용하는데, Exception과 Error이다.

Error은 복구가 불가능한 오류를 나타낼때 사용한다.

Exception은 복구가 가능한 오류를 나타낼때 사용한다.

그리고 Exception은 성격에 따라 두 가지로 나뉘게 되는데 이 두 가지가 Checked Exception과 Unchecked Exception이다.

Unchecked Exception은 RuntimeException을 포함하여 RuntimeException을 상속받는 모든 객체를 의미하고  
Checked Exception은 이외의 Exception을 의미한다.

Checked Exception은 명시적으로 Exception을 처리하는(handling) 과정을 코드로 작성해야하기 때문에 Checked이고  
Unchecked Exception은 명시적으로 Exception을 처리하지 않아도 되기 때문에 Uncheked 이다.

## Checked Exception

Checked Exception은 명시적으로 Exception을 처리해줘야하기 때문에 **컴파일 타임에 반드시 Exception을 다루어 주어야한다.**

그리고 **기본적**으로 트랜잭션에서는 RuntimeException을 기준으로 rollback을 진행하기 때문에 **Checked Exception이 발생한다고 rollback이 되지는 않는다**.

## Unchecked Exception

Unchecked Exception은 명시적으로 Exception을 처리해줘야할 필요가 없기 때문에 **Exception을 Runtime에만 다루어 주면된다**.

그리고 **트랜잭션의 rollback 기본 전략에 속하므로 Exception 발생시 rollback이 진행**된다.