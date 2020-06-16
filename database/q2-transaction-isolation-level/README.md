# 트랜잭션 고립 수준에 대해 설명해보세요

[비밥의 트랜잭션 고립 수준](bebop.md)

[엘리의 트랜잭션 고립 수준](elly-transaction-isolation-level.md)

[코일의 트랜잭션 고립 수준](coyle.md)

[깍두기의 트랜잭션 고립 수준](stitch.md)



### 트랜잭션

데이터에 대한 하나의 논리적 실행 단계이다.



### 격리성 (고립성) 

- 고립성은 서로 다른 트랜잭션은 서로 영향을 미치지 말아야 함을 의미



## 격리성 이슈

- 동시성과 격리성은 트레이드 오프 관계이다.



**Dirty Read**

1. 트랜잭션 T1이 값을 A를 B로 변경하고 아직 커밋 하지 않았을 때
2. 트랜잭션 B가 값을 조회하면 B가 조회된다.
3. T1이 최종 커밋을 하지 않고 종료하면, T2가 가진 데이터는 꼬이게 된다.

- 요약 : 커밋되기 전 데이터를 조회하면 커밋이 될지 안될지 모르므로 문제가 발생할 수 있다.
- 해결 방안 : **공유 Lock**을 걸어서 T1이 A에 접근하고 있는 동안 다른 트랜잭션이 접근 못하게한다.



**Non-Repeatable Read**





**Phantom Read**



## 트랜잭션 격리(고립) 수준

> - 트랜잭션 시나리오가 같은데도 결과가 다를 수 있는 4가지 트랜잭션 고립 수준
> - 트랜잭션의 격리성을 확보하기 위해 락을 사용한다.



### 발생가능한 문제 

| 격리수준 \| 발생 문제 | Dirty Read | Non-Repetable Read | Phantom Read |
| --------------------- | ---------- | ------------------ | ------------ |
| Read Uncommitted      | O          | O                  | O            |
| Read Committed        | X          | O                  | O            |
| Repetable Read        | X          | X                  | O            |
| Serializable          | X          | X                  | X            |



## 레벨 0. READ UNCOMMITED

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



## 레벨 1. READ COMMITED

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



예를 들어, 어떤 트랜잭션에서는 입/출금 처리가 계속 진행되고, 오늘의 입금 총합을 보여주는 트랜잭션이 있다고 하면, 총합을 계산하는 SELECT 쿼리는 실행될 때마다 다른 결과값을 가져올 것이다.



## 레벨 2. REPEATABLE READ

**트랜잭션이 시작되기 전에 커밋된 내용에 대해서만 조회할 수 있는 격리수준이다.**

레벨 1에서 발생했던 NON-REPEATABLE READ 부정합이 발생하지 않는다.

```
1. 10번 트랜잭션이 500번 사원을 조회 (사원의 이름은 A)
2. 12번 트랜잭션이 500번 사원의 이름을 B로 변경하고 COMMIT
3. 10번 트랜잭션이 500번 사원을 다시 조회
4. Undo 영역에 백업된 데이터 반환 (조회된 사원의 이름은 A)
```

즉, 자신의 트랜잭션 번호보다 낮은 번호에서 변경(Commit)된 것만 보게 되는 것이다. (12번은 10번을 알지만, 10번은 12번을 모른다.) Undo 영역에 백업된 모든 레코드는 변경을 발생시킨 트랜잭션 번호가 포함되어 있다.

Repeatable Read 격리수준에서는 트랜잭션이 시작된 시점의 데이터를 일관되게 보여주는 것을 보장해야 한다. 하지만 실제로 영향을 미칠 정도로 오래 지속되는 경우는 드물어서 Read Commited와 Repeatable Read의 성능 차이는 거의 없다고 보면 된다.

- **MySQL의 InnoDB Storage Engine에서 기본적으로 사용되는 격리수준**이다.
- InnoDB => Row lock 이므로 SELECT 중에도 다른 트랜잭션이 다른 row를 변경할 수 있다. (U,D)



### REPEATABLE READ에서 발생할 수 있는 데이터 부정합

---

Not yet......

#### 1. SELECT FOR UPDATE에서 발생하는 UPDATE 부정합

❗️REPEATABLE READ에서 어떤 트랜잭션이 락을 걸어도, UPDATE, DELETE 작업이 가능하다.

```sql
START TRANSACTION; -- transaction id : 1
SELECT * FROM Member WHERE name = 'elsa' FOR UPDATE;
		
		START TRANSACTION; -- transaction id : 2
		SELECT * FROM Member WHERE name = 'elsa';
		UPDATE Member SET name = 'anna' WHERE name = 'elsa'; // DELETE도 가능
		COMMIT;

UPDATE Member SET name = 'olaf' WHERE name = 'elsa'; -- 0 rows affected
COMMIT;
```

최종 결과, `name = 'anna'`가 된다.

2번 트랜잭션에서 `name = 'anna'`로 변경하고 COMMIT을 하면 `name = 'elsa'`의 내용을 Undo로그에 남겨놔야 한다. 그래야 1번 트랜잭션에서 일관되게 데이터를 보장해줄 수 있기 때문이다.

그리고 현재 1번 트랜잭션이 보고 있는 `name = 'elsa'`는 레코드 데이터가 아닌 Undo영역의 데이터이고, 여기서는 쓰기 잠금을 걸 수 없다.

그래서 UPDATE 쿼리에 대해 쓰기 잠금을 시도하려고 하지만, `name = 'elsa'`인 레코드는 존재하지 않으므로, 0 rows affected가 출력되고, 아무 변화도 일어나지 않는다.

---

#### Phantom READ

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

### 

## 레벨 3. SERIALIZABLE

가장 단순하고 가장 엄격한 격리수준.

InnoDB에서 기본적으로 순수한 SELECT 작업은 아무런 잠금을 걸지 않고 동작하는데, 격리수준이 SERIALIZABLE일 경우, 읽기 작업에도 공유 잠금을 설정하고, 이러면 동시에 다른 트랜잭션에서 이 레코드를 변경하지 못하게 된다.

이러한 특성 때문에 동시처리 능력이 다른 격리수준보다 떨어지고, 성능저하가 발생하게 된다.
