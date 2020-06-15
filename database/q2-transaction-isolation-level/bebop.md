# 트랜잭션 고립 수준에 대해 설명해보세요

## 트랜잭션 격리 수준 (Isolation Level)

트랜잭션은 ACID 속성을 보장해야합니다.

그 중 I 에 해당하는 Isolation은 트랜잭션의 고립성을 의미하고, 고립성은 서로 다른 트랜잭션은 서로 영향을 미치지 말아야 함을 의미합니다.

하지만 이 고립성은 동시성과 충돌할 수 밖에 없는 속성입니다.

고립성 수준, 다시말해 격리 수준이 올라감에 따라 동시성이 떨어지는 문제가 발생하게 됩니다.

그렇다고 고립성 수준을 낮추게 된다면 ACID의 C, Consistency인 일관성, 즉 데이터의 무결성 문제가 발생할 수 있습니다.

따라서 서비스를 운영할 때 서비스의 성격에 알맞는 격리 수준을 선택해야 할 필요가 있습니다.

## 0. 발생 가능한 문제

발생 가능한 문제를 격리수준에 따라 표로 나타내면 다음과 같습니다.

| 격리수준 \| 발생 문제 | Dirty Read | Non-Repetable Read | Phantom Read |
| :-------------------: | :--------: | :----------------: | :----------: |
|   Read Uncommitted    |     O      |         O          |      O       |
|    Read Committed     |     X      |         O          |      O       |
|    Repetable Read     |     X      |         X          |      O       |
|     Serializable      |     X      |         X          |      X       |

각각 격리수준에 따라 발생문제가 어떠한 것이고 어떠한 상황에서 발생하는지 알아보도록 하겠습니다.

## 1. Read Uncommitted

트랜잭션의 가장 낮은 격리 수준입니다.

격리 수준 이름에도 써있듯이 **Commit되지 않은 상황의 정보를 Read 할 수 있는** 격리 수준입니다.

따라서 가장 많은 무결성 문제가 발생할 수 있는데, 

이 수준에서만 볼 수 있는 문제는 Dirty Read가 있습니다.

---

가게( store )에 보석( jewelry )이 있다는 가정하에 설명을 하겠습니다.

그리고 트랜잭션 1은 주인, 트랜잭션 2는 손님으로 빗대어 설명하겠습니다.

| 트랜잭션의 행동                | 트랜잭션  (주인)                                         | 트랜잭션 (손님)           | 결과                                         |
| ------------------------------ | -------------------------------------------------------- | ------------------------- | -------------------------------------------- |
|                                |                                                          | **손님 트랜잭션 시작**    | Jewelry_id =1 jewelry = '원석'               |
| 트랜잭션 2 SELECT              | **주인 트랜잭션 시작**                                   | SELECT jewelry FROM store | 쿼리 결과 '원석'                             |
| 트랜잭션 1 UPDATE              | UPDATE store SET jewelry = '루비' WHERE jewelry = '원석' |                           | Not committed Jewelry_id =1 jewelry = '루비' |
| 트랜잭션 2 SELECT (Dirty Read) |                                                          | SELECT jewelry FROM store | **쿼리 결과 '루비'**                         |
| 트랜잭션 1 롤백 발생           | **Roll back**                                            |                           | **Jewelry_id =1 jewelry = '원석'**           |
| 트랜잭션 종료                  |                                                          |                           |                                              |

1. 가게에는 1개의 보석이 '원석'의 상태로 있습니다.
2. 트랜잭션 1(주인) 이 보석을 '원석'을 '루비'로 변경합니다. 하지만 Commit은 하지 않았습니다.
3. 트랜잭션 2(손님) 은 보석을 Read해서 '루비'로 확인하였습니다.
4. 트랜잭션 1(주인) 은 사정이 생겨 Roll back 하여 다시 보석은 '원석'으로 돌아가게 됩니다.

이 경우 실제 보석은 '원석' 이지만 트랜잭션2(손님)은 이를 '루비'로 알고 있습니다.  
손님의 트랜잭션이 끝나기 전에 '루비'를 가지고 작업을 하면 문제가 발생할 여지가 있습니다.

이렇게 확정되지 않은 ( Not Committed ) 데이터를 읽음으로써 Dirty Read 문제가 발생합니다.

## 2. Read committed

이제 **Commit된 결과만을 읽는** 트랜잭션 격리 수준입니다.

이제 Uncommited 된 데이터를 Read 하는 상황에서 발생하였던 **Dirty Read는 발생하지 않습니다.**

하지만 **Non-Repetable Read 문제는 여전히 발생**합니다.

| 트랜잭션의 행동                        | 트랜잭션  (주인)                                         | 트랜잭션 (손님)           | 결과                                         |
| -------------------------------------- | -------------------------------------------------------- | ------------------------- | -------------------------------------------- |
|                                        |                                                          | **손님 트랜잭션 시작**    | Jewelry_id =1 jewelry = '원석'               |
| 트랜잭션 2 SELECT                      | **주인 트랜잭션 시작**                                   | SELECT jewelry FROM store | 쿼리 결과 : 1 '원석'                         |
| 트랜잭션 1 UPDATE                      | UPDATE store SET jewelry = '루비' WHERE jewelry = '원석' |                           | Not Committed Jewelry_id =1 jewelry = '루비' |
| 트랜잭션 2 SELECT (Dirty Read X)       |                                                          | SELECT jewelry FROM store | 쿼리 결과 : 1 '원석'                         |
| 트랜잭션 1 Commit                      | Commit                                                   |                           | Committed Jewelry_id =1 jewelry = '루비'     |
| 트랜잭션 2 SELECT (Non-Repetable Read) |                                                          | SELECT jewelry FROM store | **쿼리 결과 : 1 '루비'**                     |
| 트랜잭션 종료                          |                                                          |                           |                                              |

격리수준이 한단계 올라가면서 **Commit된 데이터만 읽게되었습니다**.

따라서 이전 격리수준에서 Dirty Read가 발생했던 지점에서 '루비'로 읽었던 정보를 이번에는 원석으로 읽음으로써 Dirty Read가 발생하지 않았습니다.

하지만 손님 트랜잭션이 진행되는 동안 동일한 쿼리를 가지고 SELECT을 했는데 두 결과가 다른 것을 볼 수 있습니다.

처음에는 '원석'이지만 중간에 주인 트랙잭션이 UPDATE한 결과를 Commit함으로써 나중에 SELECT한 결과는 '루비'가 된것입니다.

손님 트랜잭션 입장에서 **첫번째 SELECT에서 '원석'으로** 데이터를 알고 있기 때문에 **그에 따른 행동**을 해오고 있었을 것입니다.
그러나 **트랜잭션 중간에 다시 SELECT을 해보니 '원석'으로 알고 있떤 데이터가 갑자기 '보석'이 되어있습니다**.

즉 하나의 **트랜잭션 (이 경우 손님 트랜잭션)이 진행되는 동안 동일한 SELECT은 동일한 결과를 반환**해야하는 하는데
그렇지 못한 상황이 발생했으므로 이를 **Non-Repetable Read라** 합니다.

## 3. Repetable Read

이제 한 단계 더 격리 수준이 올라갔습니다.

READ COMMITTED 격리 수준에서 Non-Repetable Read 가 발생하였지만 **REPETABLE READ은 Non-Repetable Read가 발생하지 않습니다.**

하지만 여전히 **PhanTom Read 문제는 발생하는 단계**입니다.

| 트랜잭션의 행동                          | 트랜잭션 (주인)                                          | 트랜잭션 (손님)           | 결과                                         |
| ---------------------------------------- | -------------------------------------------------------- | ------------------------- | -------------------------------------------- |
|                                          |                                                          | **손님 트랜잭션 시작**    | Jewelry_id =1 jewelry = '원석'               |
| 트랜잭션 2 SELECT                        | **주인 트랜잭션 시작**                                   | SELECT jewelry FROM store | 쿼리 결과 : 1 '원석'                         |
| 트랜잭션 1 UPDATE                        | UPDATE store SET jewelry = '루비' WHERE jewelry = '원석' |                           | Not committed Jewelry_id =1 jewelry = '루비' |
| 트랜잭션 2 SELECT (Dirty Read X)         |                                                          | SELECT jewelry FROM store | 쿼리 결과 : 1 '원석'                         |
| 트랜잭션 1 Commit                        | Commit !                                                 |                           | Committed Jewelry_id =1 jewelry = '루비'     |
| 트랜잭션 2 SELECT (Non-Repetable Read X) |                                                          | SELECT jewelry FROM store | 쿼리 결과 : 1 '원석'                         |
| 트랜잭션 1 INSERT COMMIT                 | **INSERT INTO store VALUES '사파이어' Commit!**          |                           |                                              |
| 트랜잭션 2 SELECT (Phantom Read)         |                                                          | SELECT jewelry FROM store | **쿼리 결과 : 2 '원석', '사파이어'**         |
| 트랜잭션 종료                            |                                                          |                           |                                              |

이전 격리 수준에서 Non-Repetable Read가 발생했던 지점에서 '루비'로 읽었던 정보가 이번에는 '원석'으로 읽히는 것을 볼 수 있습니다.

이 단계의 격리 수준에서는 **자신보다 늦게 시작한 트랜잭션이라면 Commit된 결과일 지라도 읽지 않고 이전 결과를 가져오게 됩니다.**

하지만 **INSERT 쿼리에는 그러한 제약조건이 해당하지 않는 단계**입니다. 따라서 **중간에 INSERT된 데이터**가
**마지막에 SELECT한 결과에 반영**되어서 유령처럼 갑자기 데이터가 등장하는 Phantom Read 현상이 나타나게 됩니다.

### 4. Serializable

가장 높은 격리 수준입니다.

지금까지 나타나던 데이터의 정합성 문제는 나타나지 않습니다.

하지만, 그 만큼 Lock이 까다롭고 빡빡하게 잡히기 때문에 동시성 이슈가 발생하게되고,
성능상 다른 격리 수준에 비해 많이 떨어지게 됩니다.