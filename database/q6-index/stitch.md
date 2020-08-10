# Index에 대하여

## Full table scan

- row의 값을 순차적으로 scan하며 값을 비교한다.
- 가장 느린 scanning 방법이며 많은 자료가 담긴 Disk를 읽기 위한 I/O를 사용하며 자원을 잡아먹는다.
- 결국엔 **속도도 느리고 자원도 많이 사용하는 좋지 않은 방법**이다.

이러한 문제를 해결하기 위해 Database에서 Index 기능을 제공한다.

<br/>

## Index란

- Index란 내가 원하는 부분을 쉽고 빠르게 찾아서 전달해주는 역할을 한다.
- **읽기 성능을 향상**시키기 위한 일종의 자료구조라고 볼 수 있다.
- Index는 관련된 Table과 **별도로 저장**되며 Index로 설정한 컬럼값이 변경되거나 추가되면 Index도 업데이트가 동시에 된다.
- Index에 주로 사용하는 **자료구조는 B+ Tree 알고리즘**이다.
- PK에는 Database가 자동으로 Index 기능을 설정하여 관리한다.

<br/>

### Index 동작 방식

- 특정 컬럼을 Index로 설정하면 해당 컬럼에 대한 Index Table이 생성이 된다.
- Index로 설정된 컬럼에 대한 where 문이 포함된 쿼리가 나갈 때 Index Table에 저장된 key-value 값을 참조해서 결과 값을 반환해온다.

#### 동작 순서

1. Index Table에서 where에 포함된 값을 찾음.
2. 해당 값의 table_id [PK]를 가져옴.
3. 가져온 table_id [PK] 값으로 원본 테이블에서 값을 조회해옴.

DBMS는 INDEX를 다양한 알고리즘으로 관리를 하고 있는데 일반적으로 사용되는 알고리즘은 B+ Tree 알고리즘이다.

#### B+ Tree Index 알고리즘

![b-tree](https://iq.opengenus.org/content/images/2018/06/b--search.jpg)

##### 구성

- 리프 노드: 실제 데이터가 저장되는 노드
- 논리 노드: 리프 노드까지의 경로 역할을 하는 노드
- 루트 노드: 경로의 출발점 노드

B+ Tree는 리프 노드에 이르기까지 자식 노드에 대한 포인터가 저장되어 있어 탐색에 있어 루트 노드에서 어떤 리프 노드에 이르는 한 개의 경로만 검색하면 되기 때문에 검색에 있어 매우 효율적인 알고리즘이다.

<br/>

### Index 종류

인덱스는 DB Table과 달리 데이터 중복성을 가질 수 있다. 그래서 다양한 종류의 인덱스 형태가 존재하는데, 인덱스 종류는 내가 사용하는 목적에 따라 적절하게 사용하는 것이 좋다.

#### 키에 따른 Index

- 기본 인덱스(Primary Index): 기본키를 포함하는 인덱스(키의 순서가 레코드의 순서를 결정 지음)
- 보조 인덱스(Secondary Index): 기본 인덱스 이외의 인덱스(키의 순서가 레코드의 순서를 의미하지는 않음)

#### 파일 조직에 다른 Index

- 집중 인덱스(Clustered Index): 데이터 레코드의 물리적 순서가 그 파일에 대한 인덱스 엔트리 순서와 동일하게(유사하게) 유지되도록 구성된 인덱스

- 비집중 인덱스(Non-Clustered Index): 집중 형태가 아닌 인덱스

#### 데이터 범위에 따른 인덱스 분류

- 밀집 인덱스(Dense Index): 데이터 레코드 각각에 대해 하나의 인덱스 엔트리가 만들어진 인덱스
- 희소 인덱스(Sparse Index): 레코드 그룹 또는 데이터 블록에 대해 하나의 엔트리가 만들어지는 인덱스

### Index 특징 및 사용 시점

#### Index Table

- 인덱스는 하나의 테이블을 생성해 값을 저장해놓고 사용한다. 그래서 다른 테이블에 의존적인 새로운 테이블이 하나 생성된다는 점에서 무분별한 인덱스 생성은 오히려 성능 저하를 초래할 수 있다.

#### 정렬

- Index Table은 **이진트리 검색**을 사용하기 때문에 기본적으로 정렬되어 있다. 그래서 만약 Index Table이 참조하는 테이블에서 삽입, 삭제, 수정이 자주 일어나게 된다면 Index Table에서는 데이터를 정렬하면서 삽입, 삭제, 수정이 이루어지기 때문에 전체적인 성능 저하를 초래할 수 있다.

#### 범위 스캔

- 인덱스는 키 컬럼 순으로 정렬되어 있기 때문에 특정 값을 찾아다 해당 범위를 넘어서는 값을 만나면 멈춘다.

<br/>

## Clustered Index

- 물리적으로 데이터를 정리하여 Index 테이블은 하나만 존재한다.
- 데이터 페이지가 물리적으로 정렬되어 있기 때문에 순차적 데이터를 접근할 때 편하다.

<br/>

## Non-Clustered Index

- 데이터는 임의적으로 존재하지만 논리적 순서는 인덱스에 의해 지정된다.
- 데이터 페이지가 물리적으로 정렬되어 있지 않기 때문에 인덱스에 의해 찾아가야 한다.

<br/>

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

<br/>

## 참고 링크

> [DB의 Index 기능에 대해 - Nesoy Blog](https://nesoy.github.io/articles/2017-07/DBIndex)
>
> [DB 성능을 위한 Index - Nathan](https://brunch.co.kr/@skeks463/25)
>
> [DB 인덱스의 구조는 어떻게 되어있나요? 인덱스는 언제 적용해야하나요?](https://jeong-pro.tistory.com/114)
>
> [DB Index란?](https://brownbears.tistory.com/57)