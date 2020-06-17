# ACID에 대하여

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