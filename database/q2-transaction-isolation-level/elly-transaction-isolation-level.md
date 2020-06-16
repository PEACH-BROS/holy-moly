# 트랜잭션 격리 수준



### 트랜잭션 격리 수준이 뭐지?

여러 트랜잭션이 동시에 처리될 때, 각 트랜잭션이 서로 얼마나 고립되어 있는지를 나타낸다.

격리 수준은 크게 4가지로 나뉜다.

- READ UNCOMMITTED
- READ COMMITTED
- REPEATABLE READ
- SERIALIZABLE



아래로 내려갈수록 트랜잭션 간 고립 정도가 높아지며, 성능이 떨어진다. (일반적으로)

일반적인 서비스에서는 READ COMMITTED(ex. Oracle)나 REPEATABLE READ(ex. MySQL)를 많이 사용한다.





우선, 격리 수준으로 인해 발생할 수 있는 문제들에 대해 살펴보자.

## Dirty Read

Commit되지 않은 정보를 읽으면서 발생하는 문제이다.





## Non-Repeatable Read

하나의 트랜잭션 내에서 똑같은 SELECT 쿼리를 수행했을 경우 항상 같은 결과를 반환해야 한다는 REPEATABLE READ 정합성에 어긋나는 것이다.



## Phantom Read

한 트랜잭션 내에 같은 쿼리를 두 번 실행했는데, 첫 번째 쿼리에서 없던 유령(Phantom) 레코드가 두 번째 쿼리에서 나타나는 현상이다.



---

이제 격리 수준 4가지에 대해 살펴보자.

## 레벨 0. READ UNCOMMITTED

**어떤 트랜잭션의 변경내용이 COMMIT이나 ROLLBACK과 상관 없이 다른 트랜잭션에서 보여진다.**

아래와 같은 문제가 발생할 수 있다.

```
1. A 트랜잭션에서 10번 사원의 나이를 27살 -> 28살로 바꿈
2. 아직 커밋하지 않음
3. B 트랜잭션에서 10번 사원의 나이를 조회함
4. 28살이 조회됨                     //Dirty Read
5. A 트랜잭션에 문제가 발생해 ROLLBACK함
6. B 트랜잭션은 10번 사원이 여전히 28살이라고 생각하고 로직을 수행함
```



> 이렇게 되면 데이터 일관성에 문제가 많이 생긴다. RDBMS 표준에서는 격리수준으로 인정하지도 않는다.



## 레벨 1. READ COMMITTED

**어떤 트랜잭션의 변경 내용이 COMMIT되어야만, 다른 트랜잭션에서 조회할 수 있다.**

Oracle을 비롯해 가장 많이 선택되는 격리수준이다.



```
1. A 트랜잭션에서 10번 사원의 나이를 27살 -> 28살로 바꿈
2. 아직 COMMIT하지 않음
3. B 트랜잭션에서 10번 사원의 나이를 조회함
4. 27살이 조회됨                 //A 트랜잭션이 아직 커밋되지 않았기 때문에!
```



하지만 다음과 같은 **NON-REPEATABLE READ 문제**가 발생할 수 있다.

```
3. B 트랜잭션에서 10번 사원의 나이를 조회함
4. 27살이 조회됨
5. A 트랜잭션 COMMIT
6. B 트랜잭션에서 10번 사원의 나이를 다시 조회
7. 28살이 조회됨                //A 트랜잭션이 커밋되었기 때문에
```

이는 하나의 트랜잭션 내에서 똑같은 SELECT 쿼리를 수행했을 경우 항상 같은 결과를 반환해야 한다는 REPEATABLE READ 정합성에 어긋나는 것이다.

작업이 금전적인 처리와 연결되어 있다면 문제가 발생할 수 있다.

예를 들어, 어떤 트랜잭션에서는 입/출금 처리가 계속 진행되고, 오늘의 입금 총합을 보여주는 트랜잭션이 있다고 하면, 총합을 계산하는 SELECT 쿼리는 실행될 때마다 다른 결과값을 가져올 것이다.





## 레벨 2. REPEATABLE READ

**트랜잭션이 시작되기 전에 커밋된 내용에 대해서만 조회할 수 있는 격리수준이다.**

(동일 트랜잭션 내에서 일관성을 보장한다.)



레벨 1에서 발생했던 NON-REPEATABLE READ 부정합이 발생하지 않는다.

```
1. 10번 트랜잭션이 500번 사원을 조회 (사원의 이름은 A)
2. 12번 트랜잭션이 500번 사원의 이름을 B로 변경하고 COMMIT
3. 10번 트랜잭션이 500번 사원을 다시 조회
4. Undo 영역에 백업된 데이터 반환 (조회된 사원의 이름은 A)
```

 즉, 자신의 트랜잭션 번호보다 낮은 번호에서 변경(Commit)된 것만 보게 되는 것이다. (12번은 10번을 알지만, 10번은 12번을 모른다.) Undo 영역에 백업된 모든 레코드는 변경을 발생시킨 트랜잭션 번호가 포함되어 있다.



Repeatable Read 격리수준에서는 트랜잭션이 시작된 시점의 데이터를 일관되게 보여주는 것을 보장해야 한다. 따라서, 한 트랜잭션의 실행시간이 길어질수록 해당 시간만큼 계속 멀티버전을 관리해야 하는 단점이 있다. 하지만 실제로 영향을 미칠 정도로 오래 지속되는 경우는 드물어서 Read Committed와 Repeatable Read의 성능 차이는 거의 없다고 보면 된다.





### REPEATABLE READ에서 발생할 수 있는 데이터 부정합

#### 1. UPDATE 부정합

❗️REPEATABLE READ에서 어떤 트랜잭션이 락을 걸어도, UPDATE, 작업이 가능하다.

```sql
START TRANSACTION; -- transaction id : 1
SELECT * FROM Member WHERE name = 'elsa';
		
		START TRANSACTION; -- transaction id : 2
		SELECT * FROM Member WHERE name = 'elsa';
		UPDATE Member SET name = 'anna' WHERE name = 'elsa';
		COMMIT;

UPDATE Member SET name = 'olaf' WHERE name = 'elsa'; -- 0 rows affected
COMMIT;
```

최종 결과, `name = 'anna'`가 된다.



2번 트랜잭션에서 `name = 'anna'`로 변경하고 COMMIT을 하면 `name = 'elsa'`의 내용을 Undo로그에 남겨놔야 한다. 그래야 1번 트랜잭션에서 일관되게 데이터를 보장해줄 수 있기 때문이다.

그리고 현재 1번 트랜잭션이 보고 있는 `name = 'elsa'`는 레코드 데이터가 아닌 Undo영역의 데이터이고, 여기서는 쓰기 잠금을 걸 수 없다.

그래서 UPDATE 쿼리에 대해 쓰기 잠금을 시도하려고 하지만, `name = 'elsa'`인 레코드는 존재하지 않으므로, 0 rows affected가 출력되고, 아무 변화도 일어나지 않는다.



#### 2. Phantom READ

REPEATABLE READ 이하에서만 발생하고, SERIALIZABLE은 발생하지 않는다.

```sql
START TRANSACTION; -- transaction id : 1
SELECT * FROM Member; -- 0건 조회

	START TRANSACTION; -- transaction id : 2
	INSERT INTO Member VALUES(1, 'elsa', 30);
	COMMIT;

SELECT * FROM Member; -- 여전히 0건 조회
UPDATE Member SET name = 'elsa' WHERE id = 1; -- 1 rows affected
SELECT * FROM Member; -- 1건 조회
COMMIT;
```

REPEATABLE READ에 의하면 원래 출력되지 않아야 하는데 UPDATE문의 영향을 받은 후부터 출력한다.



참고로 DELETE에 대해서는 적용되지 않는다.

```sql
START TRANSACTION; -- transaction id : 1
SELECT * FROM Member; -- 1건 조회

	START TRANSACTION; -- transaction id : 2
	DELETE FROM Member WHERE id = 1;
	COMMIT;
	
SELECT * FROM Member; -- 여전히 1건 조회
UPDATE Member SET name = 'elsa' WHERE id = 1; -- 0 rows affected
SELECT * FROM Member; -- 여전히 1건 조회
COMMIT;
```





## 레벨 3. SERIALIZABLE

가장 단순하고 가장 엄격한 격리수준.

InnoDB에서 기본적으로 순수한 SELECT 작업은 아무런 잠금을 걸지 않고 동작하는데, 격리수준이 SERIALIZABLE일 경우, 읽기 작업에도 공유 잠금을 설정하고, 이러면 동시에 다른 트랜잭션에서 이 레코드를 변경하지 못하게 된다.

이러한 특성 때문에 동시처리 능력이 다른 격리수준보다 떨어지고, 성능저하가 발생하게 된다.







read committed:

repeatable read: innodb 기본 스토리지 엔진. innodb는 테이블 락이 아니라 로우 락이다. 그래서 . select한 것에 대해서는 락이 걸려 있고, insert, update, delete 작업에 대해서는 락이 안 걸린다.



innodb 기본 설정. select한 것에 대해선 락이 걸려잇어서, 업데이트 불가능

insert/delete는 락이 테이블단위로 안걸려있으니까 선행 트랜잭션이 커밋하는 순간 샐랙한다. 로우 수가 불일치하니까에러발생



serializable: 한 트랜잭션이 진행중일 때 읽기/쓰기락 둘 다 걸림







> Redo와 Undo

**Redo** : 다시 하다. (복구의 역할. 복구할 때 사용자가 했던 작업을 그대로 다시 진행한다.)

**Undo**: 원상태로 돌리다 (작업 Rollback, 복구할 때 사용자의 작업을 반대로 진행한다.)

**Undo 로그?**

UPDATE, DELETE 문장으로 데이터를 변경했을 때, 변경되기 전의 데이터를 보관하는 곳.

`UPDATE member SET name = '홍길동' WHERE member_id = 1;`

이 문장이 실행되면 트랜잭션을 Commit하지 않아도 실제 데이터 파일(데이터, 인덱스 버퍼) 내용은 `홍길동`으로 변경된다. 그리고 변경되기 전의 값이 `김길동` 이었다면, Undo 영역에는 `김길동` 이라는 값이 백업되는 것이다. 이 상태에서 사용자가 Commit하게 되면 현재 상태(`홍길동`)가 그대로 유지되고, Rollback하게 되면 Undo 영역의 백업된 데이터를 다시 데이터 파일로 복구한다.



**왜 Undo 영역이 필요할까?**

1. 트랜잭션의 Rollback 대비용
2. 트랜잭션의 격리 수준을 유지하면서 높은 동시성 제공. (특히 Repeatable Read에서)



Commit되지 않은 상태에서 세션이 비정상 종료되었다면 Undo를 이용해 Rollback한다.







> 참고
>
> [기록은-기억의-연장선-트랜잭션-격리-수준](https://joont92.github.io/db/트랜잭션-격리-수준-isolation-level/)







