# Primary Key vs Unique Key

[코일의 PK vs UK](coyle.md)

[엘리의 PK vs UK](elly-pk-vs-uk.md)

[비밥의 PK vs UK](bebop.md)

[스티치의 PK vs UK](stitch.md)



## Primary Key

- UNIQUE + NOT NULL (중복을 허용하지 않으며, null일 수 없다.)
- tuple의 유일성을 보장해주는 컬럼

```mysql
CREATE TABLE team (
    id LONG PRIMARY KEY,
    name VARCHAR(10)
);
```



## Unique Key

- 중복을 허용하지 않는 컬럼
- null 입력이 가능하다.

```mysql
CREATE TABLE locationCode (
    code INT UNIQUE,
    location VARCHAR(10)
);
```

 

To be continue... ㅜㅡㅜ