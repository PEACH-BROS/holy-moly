# Checked Exception vs Unchecked Exception



## I Said



Unchecked Exception은 런타임에 발생하는 Exception들이고 Checked Exception은 그외의 모든 Exception입니다. 예를 들면 NPE 같은 경우 Unchecked Exception입니다. 



## 차이 도표



| .                          | Checked Exception         | Unchecked Exception                            |
| :------------------------- | :------------------------ | :--------------------------------------------- |
| **처리 여부**              | 반드시 예외 처리 해야함   | 예외 처리 하지 않아도됨                        |
| **트랜잭션 Rollback 여부** | Rollback 안됨             | Rollback 진행                                  |
| **대표 Exception**         | IOException, SQLException | NullPointerException, IllegalArgumentException |



- UnChecked Exceptoin 은 Runtime Exception을 상속받는다.

  

### Unchecked Exception

- 명시적인 예외 처리를 강제하지 않는 특징이 있기 때문에 Unchecked Exception이라 하며, catch로 잡거나 throw로 호출한 메서드로 예외를 던지지 않아도 상관이 없다.
- 트랜잭션 롤백 O



### Checked Exception

- 반드시 명시적으로 처리해야 하기 때문에 Checked Exception이라고 하며, try catch를 해서 에러를 잡든 throws를 통해서 호출한 메서드로 예외를 던져야 한다.
- 트랜잭션 롤백 X



---



예외 복구 전략이 명확하고 그것이 가능하다면 Checked Exception을 try catch로 잡고 해당 복구를 하는 것이 좋다.

하지만 그러한 경우는 흔하지 않으면 Checked Exception이 발생하면 더 구체적인 Unchecked Exception을 발생시키고 예외에 대한 메세지를 명확하게 전달하는 것이 효과적이다.



무책임하게 상위 메서드로 throw를 던지는 행위는 하지 않는 것이 좋다. 상위 메서드들의 책임이 그만큼 증가하기 때문이다. Checked Exception은 기본 트랜잭션에 속성에서는 rollback을 진행하지 않는 점도 알고 있어야 실수를 방지할 수 있다.





