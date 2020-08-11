# 쿼리 실행 순서에 대해 아는대로 말하세요.

[엘리의 쿼리 실행 순서](elly-query-execution-order.md)

[비밥의 쿼리 실행 순서](bebop.md)

[스티치의 쿼리 실행 순서](stitch.md)

[코일의 쿼리 실행 순서](coyle.md)

### 쿼리 실행 과정

MySQL 기준 쿼리의 실행 과정은 다음과 같다.

1. 사용자가 요청한 SQL문장을 MySQL 서버가 이해할 수 있도록 분리한다.

   - SQL 문법 상 틀린 것이 있는지 확인한다.

2. SQL의 parse tree를 확인하여 탐색에 필요한 table, index를 선정한다.

   - 이중 2에 해당하는 작업은 DB의 **옵티마이저가 쿼리 최적화 및 실행 계획을 수립**하는 단계이다.

   - 이 과정에서 다음과 같은 일이 벌어진다.
     - 불필요한 조건 제거, 복잡한 연산 단순화
     - 여러개의 테이블 조인이 있을경우 어떠한 순서로 읽을지 결정
     - 각 테이블에 사용된 조건과 인덱스 통계 정보를 이용해 사용할 인덱스 결정
     - 가져온 레코드를 임시 테이블에 넣고 한번 더 가공 해야하는지 결정

   1. 쿼리 분석 : 조인인지 확인

   2. 인덱스 선택 : 테이블 스캔 or 가장 효율적인 인덱스를 선택

   3. 조인 처리 : JOIN, UNION, GROUP BY, ORDER BY 절로 적절한 작업 순서 선택

3. 선정된 table과 index를 이용해 데이터를 가져온다.

4. 컴파일

 - 컴파일하고 이진 코드가 생성된다. 보통 컴파일이 완료되면 exe와 같은 이진코드로 만들어진 실행파일이 만들어지지만 DB는 메모리에만 올리기 때문에 컴파일 속도가 빠르다.

5. 실행

 - 알맞은 순서로 실행한다.

> 통계정보?
>
> 이전에 인덱스가 레인지 스캔을 할지 테이블 풀 스캔을 할지 결정하기 위해서 인덱스를 사용 할 지 말지 결정하는 조건으로 사용되는 것을 알아 봤었다.

<br/>

### 쿼리 실행 순서

---

FROM

ON 
JOIN

---

WHERE

---

GROUP BY

HAVING

---

SELECT

---

DISTINCT

---

ORDER BY

---

LIMIT

---

(WHERE 조건 절과) ORDER BY 또는 GROUP BY가 일치할 경우에 index를 이용하면 단계 자체가 불필요하기 때문에 삭제 된다.

<br/>

**🤔 서브쿼리의 경우?**

어떤 서브쿼리문인지에 따라 다르다.

<br/>

```sql
SELECT *
FROM PLAYER
WHERE TEAM_ID IN (
                  SELECT ID
                  FROM TEAM
  							 )
```

➡️ **not correlated 서브쿼리 (메인쿼리 컬럼을 갖고 있지 않다)**

만약, TEAM.ID가 PLAYER.TEAM_ID 개수보다 적으면, 서브쿼리를 먼저 실행하는 것이 효율적이다.
서브쿼리를 먼저 실행 후, 메모리에 올려둔 다음에 거기서 WHERE절과 맞는 것을 찾는다.

<br/>

```sql
SELECT *
FROM PLAYER
WHERE TEAM_ID IN (
                  SELECT TEAM_ID
                  FROM TEAM
  								WHERE TEAM.type = PLAYER.type
  							 )
```

➡️ **correlated 서브쿼리 (메인쿼리의 컬럼을 참조한다)**

이 상황에서는 서브쿼리를 먼저 수행할 수 없다. 서브쿼리만 수행해서는 PLAYER.type을 알 길이 없기 때문이다. 

그리고, PLAYER.type의 값은 메인쿼리의 각 행마다 다를 수 있으므로, 서브쿼리는 각 행별로 1번씩 더 실행될 수 있다.



만약 TEAM.type에 대해 가능한 값이 몇 개 없다면, 똑똑한 RDBMS는 *서브쿼리를 한 번 실행하는 비용*이 *각 행에 대해 수행하는 비용*보다 저렴할 것이라고 추측한다. not correlated에서 사용되는 탐색 방식을 사용할 수 있다.

(아마도 FROM TEAM 이 FROM TEAM, PLAYER로 변하지 않을까 싶다.)

<br/>

### HAVING vs WHERE

전체 조건의 처리에 대해서는 having에서 거르는 것 보다 where에서 거르는 것이 좋다.

group by의 결과에 대해 조건을 처리할 경우 having을 사용하는 것이 적절하다.

### Alias 사용

order by절에서 select 절의 alias를 사용 가능하다.

where 절에서는 실행 순서가 select 보다 앞서기 때문에 select 절의 alias를 사용할 수 없다.