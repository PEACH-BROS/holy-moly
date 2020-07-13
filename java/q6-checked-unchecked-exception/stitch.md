# Checked Exception vs Unchecked Exception

## Error

**에러(Error)**는 시스템에 무엇인가 비정상적인 상황이 발생한 경우에 사용된다. 주로 자바 가상머신에서 발생시키는 것이며 예외와 반대로 이를 애플리케이션 코드에서 잡으려고 하면 안 된다. 에러의 예로는 `OutOfMemoryError`, `ThreadDeath`, `StackOverflowError` 등이 있다.



## Exception

**예외(Exception)**란 입력 값에 대한 처리가 불가능하거나, 프로그램 실행 중에 참조된 값이 잘못된 경우 등 정상적인 프로그램의 흐름을 어긋나는 것을 말한다. 그리고 자바에서 예외는 개발자가 직접 처리할 수 있기 때문에 예외 상황을 미리 예측하여 핸들링할 수 있다.



## 예외 구분

**RuntimeException을 상속하지 않는 클래스**는 Checked Exception, 반대로 상속한 클래스는 Unchecked Exception으로 분류할 수 있다.

![exception](https://madplay.github.io/img/post/2019-03-02-java-checked-unchecked-exceptions-1.png)

|     구분      |           Checked Exception            | Unchecked Exception                               |
| :-----------: | :------------------------------------: | ------------------------------------------------- |
|   확인 시점   |          컴파일(Compile) 시점          | 런타임(Runtime) 시점                              |
|   처리 여부   |       반드시 예외 처리해야 한다        | 명시적으로 하지 않아도 된다                       |
| 트랜잭션 처리 |  예외 발생시 롤백(rollback)하지 않음   | 예외 발생시 롤백(rollback)해야 함                 |
|     종류      | IOException, ClassNotFoundException 등 | NullPointerException, IllegalArgumentException 등 |

### Checked Exception

프로그래머가 사용하는 라이브러리, 코드 등에서 new 생자를 이용해 만들어졌고, 해당 모듈을 사용하는 오브젝트에게 여기서 예외가 발생할 수 있으니 throws 키워드를 이용해서 상위 caller에게 예외를 전달하든, 직접 확인해서 처리하든지 하라는 확인이 필요한 예외이다.



### Unchecked Exception

프로그래머가 사용하는 라이브러리, 코드 등에서 만들어졌지만, 해당 모듈을 사용하는 오브젝트에서 여기서 발생한 예외를 확인해서 처리하라는 강제를 두지 않는다. 예외는 발생할 수 있지만 필요 없거나 어차피 처리하지 못할 예외이니 그냥 상위 caller로 던지라는 뜻이다. 계속 던지다 보면 JVM이 받아서 프로그램을 종료하거나 처리한다.