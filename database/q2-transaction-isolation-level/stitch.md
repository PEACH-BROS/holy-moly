# 트랜잭션 격리 수준에 대하여

## 트랜잭션 : *Transaction*

- **논리적인 작업 셋** 자체가 100% 적용되거나 또는 아무것도 적용되지 않아야 함을 보장해주는 것.

- Commit 또는 Rollback, 두 가지 작업만 이루어짐을 보장해주는 것.



## ACID : *Atomicity, Consistency, Isolation, Durability*

- **Transaction**이 안전하게 수행된다는 것을 보장하기 위한 성질을 가리키는 약어.



### 원자성 : *Atomicity*

- Transaction이 완전히 실행되거나 전혀 실행되지 않음을 의미.
- **All-or-Nothing**
- Transaction에 포함된 모든 작업이 성공*(Commit)* 또는 실패*(Abort)* 하는 성질을 원자성.



### 일관성 : *Consistency*

- Transaction을 수행하는 경우 실행부터 종료까지 데이터베이스가 **일관적**이어야 함을 의미.
- 일관성이란 '내부적 일관성'을 의미.
- Database 측면에서는 적어도 무결성 제약조건*(Integrity Constraint)*을 모두 충족시킨다는 의미.



### 고립성 : *Isolation*

- 동시에 실행되는 여러 개의 Transaction이 서로 영향을 주지 않는 성질.
- 동시에 실행되는 Transaction은 서로 격리된다는 것을 의미.
- 고립성을 잘 갖춘 시스템이라면, 사용자 관점에서는 시스템을 **혼자서만 사용하고 있는 것**처럼 생각하고 이용할 수 있음.



### 영속성 : *Durability*

- Commit이 완료된 Transaction은 손상되지 않는 성질.
- Transaction이 성공적으로 Commit 됐다면 하드웨어 결함이 발생하거나 Database가 죽더라도 Transaction에 기록한 모든 데이터는 손실되지 않는다는 보장.
- 모든 Transaction을 Log로 남긴 채로 Rollback도 가능.



## 트랜잭션 격리 수준 : *Transaction Isolation Level*

### 트랜잭션 격리 수준이란

- **동시**에 여러 트랜잭션이 처리될 때, 특정 트랜잭션이 다른 트랜잭션에서 변경하거나 조회하는 데이터를 볼 수 있도록 허용할지 말지를 결정하는 것.
- 격리 수준은 **Read Uncommitted**, **Read Committed**, **Repeatable Read**, **Serializable**의 4가지로 나뉨.
- **Dirty Read**라고도 하는 Read Uncommitted는 일반적인 데이터페이스에서는 거의 사용하지 않음.



### 부정합 문제(읽기 이상 현상) : Read Phenomena

- **Dirty Read**
  - Transaction 1에서 데이터 'A'에 접근하여 'VALUE' 값을 'X'에서 'Y'로 변경, 그리고 아직 Commit을 하지 않음.
  - Transaction 2에서 데이터 'A'에 접근하여 'VALUE' 값을 읽으면 'Y'라는 결과가 나옴.
  - 하지만 Transaction 1이 최종 Commit을 하지 않고 종료한다면 Transaction 2는 잘못된 데이터를 가지게 됨.
  - 이러한 상황을 **Dirty Read**라고 함.
- **Non-Repeatable Read**
  - Transaction 1에서 데이터 'A'의 'VALUE' 값을 읽는 쿼리를 실행 시 'X'라는 결과가 나옴.
  - Transaction 2에서 데이터 'A'의 'VALUE' 값을 'X'에서 'Y'로 Update 후 Commit.
  - Transaction 1에서 데이터 'A'의 'VALUE' 값을 읽는 **이전과 동일한 쿼리**를 실행했는데 'Y'라는 결과가 나옴.
  - 이러한 상황을 **Non-Repeatable Read**라고 함.
- **Phantom Read**
  - Transaction 1에서 데이터 'A'를 조회하니 'VALUE' 값이 'X'라는 결과가 나옴.
  - Transaction 2에서 테이블에 'RESULT'라는 컬럼을 추가 후, 데이터 'A'의 'RESULT' 값을 '10'으로 Update후 Commit.
  - Transaction 1에서 데이터 'A'를 다시 조회하니 'VALUE' 값으로 'X', 'RESULT' 값으로 '10'이 나옴.
  - 그리고 Transaction 2가 Rollback을 하면 Transaction 1은 잘못된 데이터를 가지게 됨.
  - 이러한 상황을 **Phantom Read**라고 함.



### 트랜잭션 격리 수준에 따른 Read Phenomena 발생 여부

|                  | Dirty Read    | Non-Repeatable Read | Phantom Read  |
| ---------------- | ------------- | ------------------- | ------------- |
| Read Uncommitted | 발생          | 발생                | 발생          |
| Read Committed   | 발생하지 않음 | 발생                | 발생          |
| Repeatable Read  | 발생하지 않음 | 발생하지 않음       | 발생          |
| Serializable     | 발생하지 않음 | 발생하지 않음       | 발생하지 않음 |



### Read Uncommitted

> Dirty Read, Non-Repeatable Read, Phantom Read 발생

- 각 트랜잭션에서의 변경 내용이 Commit이나 Rollback 여부에 상관 없이 다른 트랜잭션에서 보여진다.
- 



### Read Committed





### Repeatable Read





### Serializable





## Undo

### Undo란

- **Undo** = Rollback
- **Undo Data**
  - 원본 데이터
  - 'A'를 'B'로 Update 할 경우 'A'를 **Undo Data**라고 함.
- **Undo Tablespace**
  - 사용자가 DML*(Data Manipulation Language)*을 수행할 경우 Undo Data들을 저장해 두는 특별한 Tablespace.
  - Instance 별로 적어도 하나 이상의 Undo Tablespace가 있어야 함.
- **Undo Segment**
  - Undo Data를 저장하고 있는 테이블.
  - Rollback을 할 경우 **Undo Segment**에 저장된 **Undo data**를 사용해서 Rollback을 진행.



### Undo의 기능

- **Transaction Rollback**
  - 특정 작업을 수행한 후 Commit을 수행하지 않은 작업에 Rollback을 수행하게 되면 작업 수행 전의 데이터로 복구되는 기능.
- **Read Consistency** 유지
  - Transaction이 변경되는 동안 다른 사용자들은 이 Consistent Read에 의해 Commit 되지 않은 변경 사항을 볼 수 없는 기능.
- **Transaction Recovery**
  - Transaction이 진행되는 동안 Instance가 실패한 경우 Database가 다시 열릴 때 Commit 되지 않은 사항은 Rollback 되어야 하는데 이때 Undo Segment 정보가 사용됨.



## 참고 링크

> [ACID - 기계인간 John Grib](https://johngrib.github.io/wiki/ACID/)
>
> [Database 트랜잭션을 위한 ACID의 개념]([https://jins-dev.tistory.com/entry/Database-%ED%8A%B8%EB%9E%9C%EC%9E%AD%EC%85%98%EC%9D%84-%EC%9C%84%ED%95%9C-ACID-%EC%9D%98-%EA%B0%9C%EB%85%90](https://jins-dev.tistory.com/entry/Database-트랜잭션을-위한-ACID-의-개념))
>
> [Real My SQL](#)
>
> [UNDO REDO - dbcafe](http://dbcafe.co.kr/wiki/index.php/UNDO_REDO)
>
> [Undo Tablesapce와 관리 - ultrasound]([https://ultrasound.tistory.com/entry/Undo-Tablespace%EC%99%80-%EA%B4%80%EB%A6%AC](https://ultrasound.tistory.com/entry/Undo-Tablespace와-관리))
>
> 