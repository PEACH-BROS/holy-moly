# SQL 실행 순서

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