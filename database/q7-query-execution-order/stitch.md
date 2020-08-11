# SQL 실행 순서

## 실행 순서

```mysql
select
	job_id
	from employees
	where salary > 3000
	group by job_id
	having count(*) > 1
	order by job_id
```

1. from 절
2. where 절
3. group by 절
4. having 절
5. select 절
6. order by 절

<br/>

## having vs where

전체 조건의 처리에 대해서는 having에서 거르는 것 보다 where에서 거르는 것이 좋다.

group by의 결과에 대해 조건을 처리할 경우 having을 사용하는 것이 적절하다.



## Alias 사용

order by절에서 select 절의 alias를 사용 가능하다.

where 절에서는 실행 순서가 select 보다 앞서기 때문에 select 절의 alias를 사용할 수 없다.

