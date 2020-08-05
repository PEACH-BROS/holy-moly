# Primary Key vs Unique Key

### Primary Key

- UNIQUE + NOT NULL (중복을 허용하지 않으며, null일 수 없다.)

- row?의 유일성을 보장해주는 컬럼

```mysql
CREATE TABLE team (id LONG PRIMARY KEY, name VARCHAR(10));
```



### Unique Key

- 중복을 허용하지 않는 컬럼
- null 입력이 가능하다.

```mysql
CREATE TABLE locationCode (code INT UNIQUE, location VARCHAR(10));
```

<br/>

---

PK가 Unique하다 -> (O)

PK가 UK를 포함한다. -> (X)