# Checked Exception VS Unchecked Exception

[비밥의 Checked vs Unchecked Exception](bebop.md)

[코일의 Checked vs Unchecked Exception](coyle.md)

[엘리의 Checked vs Unchecked Exception](elly-checked-unchecked-exception.md)

[스티치의 Checked vs Unchecked Exception](./stitch.md)

---



## Error

**에러(Error)**는 시스템에 무엇인가 비정상적인 상황이 발생한 경우에 사용된다. 주로 자바 가상머신에서 발생시키는 것이며 예외와 반대로 이를 애플리케이션 코드에서 잡으려고 하면 안 된다. 에러의 예로는 `OutOfMemoryError`, `ThreadDeath`, `StackOverflowError` 등이 있다.



## Exception

**예외(Exception)**란 입력 값에 대한 처리가 불가능하거나, 프로그램 실행 중에 참조된 값이 잘못된 경우 등 정상적인 프로그램의 흐름을 어긋나는 것을 말한다. 그리고 자바에서 예외는 개발자가 직접 처리할 수 있기 때문에 예외 상황을 미리 예측하여 핸들링할 수 있다.

![exception](https://madplay.github.io/img/post/2019-03-02-java-checked-unchecked-exceptions-1.png)

|     구분      |           Checked Exception            | Unchecked Exception                               |
| :-----------: | :------------------------------------: | ------------------------------------------------- |
|   확인 시점   |          컴파일(Compile) 시점          | 런타임(Runtime) 시점                              |
|   처리 여부   |       반드시 예외 처리해야 한다        | 명시적으로 하지 않아도 된다                       |
| 트랜잭션 처리 |  예외 발생시 롤백(rollback)하지 않음   | 예외 발생시 롤백(rollback)해야 함                 |
|     종류      | IOException, ClassNotFoundException 등 | NullPointerException, IllegalArgumentException 등 |

## Unchecked Exception

Unchecked Exception은 명시적으로 Exception을 처리해줘야할 필요가 없기 때문에 **Exception을 Runtime에만 다루어 주면된다**.

그리고 **트랜잭션의 rollback 기본 전략에 속하므로 Exception 발생시 rollback이 진행**된다.



## Checked Exception

Checked Exception은 명시적으로 Exception을 처리해줘야하기 때문에 **컴파일 타임에 반드시 Exception을 다루어 주어야한다.**

그리고 **기본적**으로 트랜잭션에서는 RuntimeException을 기준으로 rollback을 진행하기 때문에 **Checked Exception이 발생한다고 rollback이 되지는 않는다**.



### Checked Exception 처리 방법

> ### `throw` vs `throws`

`throw` : 예외를 발생시키는 것

`throws` : 메소드 레벨에 붙는 키워드. 현재 메소드에서 상위 메소드로 예외를 던진다. 메소드 실행 시 발생하는 exception을 선언한다.

```java
public void foo throws SubwayException {
 	 subway.run(); //SubwayException을 발생시킬 수 있는 메소드
}


public void run() {
    if (driver.isNotReady()) {
      throw new SubwayException("기관사가 준비되지 않았어요");
    }
    ...
}
```



## 트랜잭션 Rollback 여부

Checked Exception : Rollback :x: 

Unchecked Exception : Rollback :o: 

❗️ **스프링은 디폴트로 Unchecked Exception과 Error에 대해 롤백 정책을 설정해두었다.**

```java
@Transactional
public void saveBook(BookRequest bookRequest) {
		bookRepository.save(bookRequest.toBook());
}

//사실 스프링은 아래와 같이 이해한다.
@Transactional(rollbackFor = {RuntimeException.class, Error.class}) // default
public void saveBook(BookRequest bookRequest) {
		bookRepository.save(bookRequest.toBook());
}
```

