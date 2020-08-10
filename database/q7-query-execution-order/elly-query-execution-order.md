# 쿼리 실행 순서

```sql
5 SELECT JOB_ID, NAME, AVG(SALARY) SAL_AVG
1 FROM MEMBER
	JOIN TEAM
	ON MEMBER.TEAM_ID = TEAM.TEAM_ID
2 WHERE SALARY > 10000
3 GROUP BY JOB_ID
4 HAVING COUNT(*) > 1 --GROUP BY 이후 사용되는 조건절
6 ORDER BY SAL_AVG DESC;
```



실행 순서 : 

**FROM - WHERE - GROUP BY - HAVING - SELECT - ORDER BY**

*FWGHSO*

좀 더 상세히 들어가자면,

FROM - **ON - JOIN** - WHERE - GROUP BY - HAVING - SELECT - **DISTINCT** - ORDER BY - **TOP**



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



> 참고
>
> https://myjamong.tistory.com/172  
> https://eehoeskrap.tistory.com/77  
> https://stackoverflow.com/questions/2263186/in-which-sequence-are-queries-and-sub-queries-executed-by-the-sql-engine