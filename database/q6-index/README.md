# 인덱스가 뭐죠?

[비밥의 인덱스](bebop.md)

[스티치의 인덱스](stitch.md)

[엘리의 인덱스](elly-index.md)

## Full table scan

- row의 값을 순차적으로 scan하며 값을 비교한다.
- 가장 느린 scanning 방법이며 많은 자료가 담긴 Disk를 읽기 위한 I/O를 사용하며 자원을 잡아먹는다.
- 결국엔 **속도도 느리고 자원도 많이 사용하는 좋지 않은 방법**이다.

이러한 문제를 해결하기 위해 Database에서 Index 기능을 제공한다.



## Index란

- Index란 내가 원하는 부분을 쉽고 빠르게 찾아서 전달해주는 역할을 한다.
- **읽기 성능을 향상**시키기 위한 일종의 자료구조라고 볼 수 있다.
- Index는 관련된 Table과 **별도로 저장**되며 Index로 설정한 컬럼값이 변경되거나 추가되면 Index도 업데이트가 동시에 된다.
- Index에 주로 사용하는 **자료구조는 B+ Tree 알고리즘**이다.
- PK에는 Database가 자동으로 Index 기능을 설정하여 관리한다.



### Index 동작 방식

- 특정 컬럼을 Index로 설정하면 해당 컬럼에 대한 Index Table이 생성이 된다.
- Index로 설정된 컬럼에 대한 where 문이 포함된 쿼리가 나갈 때 Index Table에 저장된 key-value 값을 참조해서 결과 값을 반환해온다.



#### 동작 순서

1. Index Table에서 where에 포함된 값을 찾음.
2. 해당 값의 table_id [PK]를 가져옴.
3. 가져온 table_id [PK] 값으로 원본 테이블에서 값을 조회해옴.



DBMS는 INDEX를 다양한 알고리즘으로 관리를 하고 있는데 일반적으로 사용되는 알고리즘은 B+ Tree 알고리즘이다.



#### 범위 스캔

- 인덱스는 키 컬럼 순으로 정렬되어 있기 때문에 특정 값을 찾아다 해당 범위를 넘어서는 값을 만나면 멈춘다.



## Clustered Index

- 물리적으로 데이터를 정리하여 Index 테이블은 하나만 존재한다.
- 데이터 페이지(테이블)가 물리적으로 정렬되어 있기 때문에 순차적 데이터를 접근할 때 편하다.

<br/>

## Non-Clustered Index

- 데이터는 임의적으로 존재하지만 논리적 순서는 인덱스에 의해 지정된다.
- 데이터 페이지(테이블)가 물리적으로 정렬되어 있지 않기 때문에 인덱스에 의해 찾아가야 한다.



## Index 설정하기

### MySQL Index 설정하기

- 기존 테이블에 Index 추가하기.

  ```mysql
  create index part_of_name on customer (name(10));
  ```

- 테이블 생성 및 Index 추가하기.

  ```mysql
  create table lookup (id int) engine = memory;
  create index id_index on lookup (id) using btree;
  ```

### MySQL Engine Option

![mysql engine option](https://nesoy.github.io/assets/posts/20170709/1.PNG)

### JPA Index 설정하기

```java
import javax.persistence.Column;
import javax.persistence.Entity;
import javax.persistence.Index;
import javax.persistence.Table;

@Entity
@Table(name = "region",
       indexes = {@Index(name = "my_index_name",  columnList="iso_code", unique = true),
                  @Index(name = "my_index_name2", columnList="name",     unique = false)})
public class Region{

    @Column(name = "iso_code", nullable = false)
    private String isoCode;

    @Column(name = "name", nullable = false)
    private String name;

}
```



### 인덱스에서 사용하는 알고리즘

1. **B-Tree(Balanced Tree) 인덱스**

   가장 많이 사용된다.

   컬럼의 원래 값을 변형하지 않고, 값의 앞부분만 잘라서 인덱스 구조체 내에서 항상 정렬된 상태를 유지한다.

   원래 값을 이용해 인덱싱한다.

   키 값이 정렬되어 있지만, 데이터 파일의 레코드는 정렬되어 있지 않다.

   🤔 왜 페이지로 나뉘어 있는지는 모르겠다.

   ![image](https://user-images.githubusercontent.com/19922698/89736501-c69ea980-daa4-11ea-8a4a-9bb7aabd844d.png)

2. **Hash 인덱스**

   컬럼의 값으로 해시 값을 계산해 인덱싱한다. 매우 빠른 검색을 지원한다.

3. **Fractal-Tree 인덱스**

   B-Tree의 단점 보완. 마찬가지로 값 변형 없다. 근데 데이터 저장/삭제될 때 처리 비용이 상당히 줄어든다.



### 클러스터 키 선택 기준? (InnoDB 테이블에서는...)

1. PK가 있으면 PK를 클러스터 키로 선택

2. NOT NULL 옵션의 UNIQUE 인덱스 중에서 첫 번째 인덱스를 클러스터 키로 선택

3. 자동 증가 컬럼을 내부적으로 추가해서 클러스터 키로 선택





인덱스는 SortedList 자료구조의 형태를 띄고 있기 때문에 항상 정렬된 상태를 가지고 있다.

- 저장될 때마다 값을 정렬하느라 느리다.
  - CUD 의 처리가 느리다.
- 대신 빠르게 값을 찾을 수 있다.
  - R 의 처리는 빠르다.



### 선택도(기수성) - Cardinality

기수성이라는 말은 모든 인덱스 중 유니크한 인덱스의 값을 나타내는 말이다.  
즉, 10개의 인덱스 키가 있다고 했을때, 유니크한 값의 갯수가 4라면 기수성은 4이다.  
이러한 기수성이 높을수록 선택 대상이 줄어들어서 빠르게 검색할 수 있다.

인덱스를 적용하면 무조건 읽기의 성능이 향상될것이라 오해 할 수 있는데 그렇지 않다.  
인덱스를 이용한 읽기 작업의 인덱스를 이용하지 않은 읽기작업보다 비용이 더 들어가기 때문이다.  
따라서 인덱스를 통해 읽어야 하는 레코드의 수가 20~25%를 넘어서면 인덱스를 이용한 읽기 작업이 오히려 더 성능을 떨어 뜨릴수 있다.  
따라서 데이터 베이스 옵티마이저가 이를 판단해서 인덱스를 사용하기도 사용하지 않기도 한다.