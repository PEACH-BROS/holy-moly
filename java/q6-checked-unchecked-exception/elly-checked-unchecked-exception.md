# Checked Exception vs Unchecked Exception

:rocket: 나의 답변

Checked Exception은 컴파일 시에 체크가 가능한 익셉션이기 때문에 Checked라고 부르고, Unchecked Exception은 런타임 시에 발생하는 익셉션입니다. Checked Exception은 반드시 처리가 필요합니다.

---



![image](https://user-images.githubusercontent.com/19922698/87325467-71867b00-c56c-11ea-868f-cff01a224e51.png)





### 트랜잭션 Rollback 여부

Checked Exception : Rollback :x: 

Unchecked Exception : Rollback :o: 

❗️ **스프링은 디폴트로 Unchecked Exception과 Error에 대해 롤백 정책을 설정해두었다.**

```java
@Transactional
public void saveBook(BookRequest bookRequest) {
		bookRepository.save(bookRequest.toBook());
}

//사실 스프링은 아래와 같이 이해한다.
@Transactional(rollbackFor = {RuntimeException.class, Error.class})
public void saveBook(BookRequest bookRequest) {
		bookRepository.save(bookRequest.toBook());
}
```

🤔 그럼 @Transactional 을 붙인다고 다 롤백이 되는게 아니었네?



### `Error` VS `Exception`

Error : 심각한 수준의 오류. 미리 예측해서 처리할 수 없는 영역. 

Exception : 개발자가 구현한 로직에서 발생.

---

:pencil: 궁금해요

### Checked Exception 처리 방법

1. `try-catch 블록` : 예외 복구
2. `throw` : 예외 전환
3. `throws` : 예외처리 회피



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





> 참고
>
> https://vitalholic.tistory.com/246  
> https://close852.tistory.com/47  
> https://hamait.tistory.com/213  
>

