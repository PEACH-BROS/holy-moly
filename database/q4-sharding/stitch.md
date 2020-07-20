# Sharding에 대하여

## Partitioning

DBMS가 많은 테이블을 관리하다 보니 느려지는 이슈를 해결하는 방법 중 하나가 파티셔닝이다.

파티셔닝은 큰 테이블이나 인덱스를 관리하기 쉬운 단위로 분리하는 방법을 의미한다.

### 장점

#### 가용성(*Availability*)

물리적인 Partitioning으로 인해 전체 데이터의 훼손 가능성이 줄어들고 데이터 가용성이 향상된다.

#### 관리용이성(*Manageability*)

큰 테이블들을 제거하여 관리를 쉽게 해준다.

#### 성능(*Performance*)

특정 DML과 Query의 성능을 향상시킨다. 주로 대용량 Data Write 환경에서 효율적이다.

많은 Insert가 있는 OLTP 시스템에서 Insert 작업들을 분리된 파티션들로 분산시켜 경합을 줄인다.

### 단점

테이블간의 Join에 대한 비용이 증가한다.

테이블과 인덱스를 별로도 파티션 할 수는 없다. 테이블과 인덱스를 같이 파티셔닝하여야 한다.

### Partitioning 범위

#### Range Partitioning

연속적인 숫자나 날짜 기준으로 파티셔닝을 한다.

손쉬운 관리 기법 제공에 따른 관리 시간을 단축할 수 있다.

우편번호, 일별, 월별, 분기별 등의 데이터에 적합하다.

#### List Partitioning

특정 파티션에 저장될 데이터에 대한 명시적 제어가 가능하다.

분포도가 비슷하며 많은 SQL에서 해당 컬럼의 조건이 많이 들어오는 경우 유용하다.

Multi-Column Partition Key를 제공하기 힘들다.

사람을 북유럽, 아시아, 서유럽 등으로 분리하는 데이터에 적합하다.

#### Composite Partitioning

파티션의 서브 파티셔닝을 말한다.

큰 파이션에 대한 I/O 요청을 여러 파티션으로 분산할 수 있다.

Range Partitioning을 할 수 있는 컬럼이 있지만 파티셔닝 결과 생성된 파티션이 너무 커서 효과적으로 관리할 수 없을 때 유용하다.

Range-List, Range-Hash

#### Hash Partitioning

Partition Key의 Hash 값에 의한 파티셔닝.

Select시 조건과 무관하게 병렬 Degree를 제공한다.

특정 Data가 어느 Hash Partition에 있는지 판단 불가하다.

파티션을 위한 범위가 없는 데이터에 적합하다.

### Partitioning 방법

#### Horizontal Partitioning

데이터의 개수를 기준으로 나누어 파티셔닝 한다.

##### 장점

데이터의 개수가 작아지고 따라서 index의 개수도 작아지게 된다. 자연스럽게 성능이 향상된다.

##### 단점

서버간의 연결 과정이 많아진다.

데이터를 찾는 과정이 기존보다 복잡하기 때문에 latency가 증가하게 된다.

하나의 서버가 고장나게 되면 데이터의 무결성이 깨질 수 있다.

#### Vertical Partitioning

데이터의 컬럼을 기준으로 나누어 파티셔닝 한다.

정규화하는 과정도 이와 비슷하다고 볼 수 있지만 Vertical Partitioning은 이미 정규화된 데이터를 분리하는 과정이다.

자주 사용하는 컬럼들을 분리시켜 성능을 향상시킬 수 있다.

<br/>

## Sharding

같은 테이블 스키마를 가진 데이터를 다수의 데이터베이스에 분산하여 저장하는 방법을 의미한다.

Horizontal Partitioning이라고 볼 수 있다.

### 그렇다면 Sharding이 최선?

샤딩을 적용하면 프로그래밍, 운영적인 복잡도가 더 높아지는 단점이 있다.

가능하면 샤딩을 피하거나 지연시킬 수 있는 방법을 찾는 것이 우선되어야 한다.

### Sharding 원리

- 분산된 데이터베이스에서 데이터를 어떻게 Read할 것인가.
- 분산된 데이터베이스에 데이터를 어떻게 잘 분산시켜서 저장할 것인가?
  - 분산이 잘 되지 않고 한 쪽으로 Data가 몰리게 되면 자연스럽게 Hotspot이 되어 성능이 느려지게 된다.
  - 균일하게 분산하는 것이 중요한 목표이다.

### Sharding  방법

Shard Key를 정의하는 방법에 따라 데이터를 어떻게 효율적으로 분산시키는 것인지가 결정된다.

#### Hard Sharding

데이터베이스 id를 hashing하여 결정한다.

hash의 크기는 cluster안에 있는 node의 개수로 정하게 된다.

간단한 방법이다.

##### 단점

node의 개수가 증가하면 hash key가 변하게 되고, 그에 따라 resharding이 필요하다.

hash key로 분산되기 때문에 공간에 대한 효율을 생각하지 않는다.

#### Dynamic Sharding

Locator Service를 통해 Shard Key를 얻는다.

node의 개수가 증가하면 Locator Service에 shard key를 추가만 하면 된다.

기존의 데이터의 shard key에는 변경이 없다. 확장에 유연한 구조이다.

##### 단점

Data Relocation을 한다면 Locator Service의 shard key table도 일치시켜줘야 한다.

Locator가 성능을 위해 Cache하거나 replication을 하면 잘못된 routing에 의해 데이터를 찾지 못하고 에러가 발생한다.

locator에 의존할 수 밖에 없다.

#### Entity Group

> Hard Sharding과 Dynamic Sharding은 Key-Value 형태를 지원하기 위한 방법이다.

객체로 구성하는 방법이다.

##### 장점

하나의 물리적인 shard에 쿼리를 진행하면 효율적이다.

하나의 shard에서 강한 응집도를 가질 수 있다.

데이터를 자연스럽게 사용자별로 분리되어 저장된다.

사용자가 늘어남에 따라 확장성이 좋은 파티셔닝이다.

##### 단점

cross-partition 쿼리는 single-partition 쿼리보다 consistency의 보장과 성능을 잃는다. 그래서 이런 쿼리들이 자주 실행하지 않도록 해야한다.

## Sharding 정리

Sharding은 피하는 방법을 우선 적용해보고 불가피하다면 적용하자.

trade-off가 존재한다.

- Locator와 sync 해야하는 비용이 필요하다.
- cross-partition 쿼리가 발생할 경우 기존의 쿼리보다 느릴 수 있다.

<br/>

## 참고 링크

>  [Database의 파티셔닝(Partitioning)이란](https://nesoy.github.io/articles/2018-02/Database-Partitioning) 
>
> [Database의 샤딩(Sharding)이란](https://nesoy.github.io/articles/2018-05/Database-Shard)

