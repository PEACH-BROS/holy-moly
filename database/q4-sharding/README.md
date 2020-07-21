# 샤딩이 뭔지 알아봅시다.

[비밥의 초간단 샤딩](bebop.md)

[스티치의 샤딩](stitch.md)

[코일의 샤딩](coyle.md)

[엘리의 샤딩](elly-sharding.md)



### Partitioning 방법

#### Horizontal Partitioning

데이터의 개수를 기준으로 나누어 파티셔닝 한다.



#### Vertical Partitioning

데이터의 컬럼을 기준으로 나누어 파티셔닝 한다.



## 파티셔닝

파티셔닝은 성능을 위해 테이블을 다시 여러개의 테이블로 쪼개는 행위를 말한다.

### 수직 파티셔닝

파티셔닝은 수직 파티셔닝과 수평 파티셔닝으로 나뉘게 되는데 수직 파티셔닝은 테이블의 컬럼을 분리하여 새로운 테이블로 분리하는 방식이다.

자주 사용하는 컬럼들을 분리시켜 성능을 향상시킬 수 있다.



> 정규화와 수직 파티셔닝의 다른점
>
> - 정규화하는 과정도 이와 비슷하다고 볼 수 있지만 Vertical Partitioning은 이미 정규화된 데이터를 분리하는 과정이다.

![image](https://user-images.githubusercontent.com/13347548/87961313-7f9a4580-caf0-11ea-8bd6-299aea914cc7.png)

### 수평 파티셔닝

샤딩은 수평 파티셔닝에 속한다.

수평 파티셔닝은 데이터를 수평으로 분리하여 테이블의 복제본을 여러개 만들고 수평으로 분리된 1개의 작은 테이블을 샤드라 부른다.  
그리고 데이터를 저장할 때 어떠한 샤드에 저장될지 샤드키를 기준으로 분리하여 저장한다.

![image](https://user-images.githubusercontent.com/13347548/87961386-98a2f680-caf0-11ea-99c6-b0d257d97c26.png)

##### 장점

데이터의 개수가 작아지고 따라서 index의 개수도 작아지게 된다. 자연스럽게 성능이 향상된다.

##### 단점

서버간의 연결 과정이 많아진다.

데이터를 찾는 과정이 기존보다 복잡하기 때문에 latency가 증가하게 된다.

하나의 서버가 고장나게 되면 데이터의 무결성이 깨질 수 있다.



## 수평 파티셔닝을 사용하는 샤딩

즉, 기존 테이블에 1~100까지 row가 있었다면 1~10을 하나의 샤드로 11~20을 다른 하나의 샤드로 분리하여 저장하는 방식이다.

![image](https://user-images.githubusercontent.com/13347548/87961427-a48eb880-caf0-11ea-86e0-f47a6a2df936.png)



## 샤딩의 이점

1. 가용성 : 물리적인 파티셔닝을 통해 전체 데이터의 훼손 가능성이 줄어든다.
2. 관리 용이성 : 큰 Table을 제거하여 관리가 쉽다.
3. 성능 : 특정 Query의 성능이 올라간다. 특히 대용량 Write 작업이 좋아진다.



## 샤딩의 단점

1. 테이블간의 Join에 대한 비용이 증가한다.
   - cross-partition 쿼리가 발생할 경우 기존의 쿼리보다 느릴 수 있다.

2. 테이블과 인덱스를 별도로 파티션 할 수는 없다. 테이블과 인덱스를 같이 파티셔닝하여야 한다.
3. 샤딩을 적용하면 프로그래밍, 운영적인 복잡도가 더 높아지는 단점
4. Locator와 sync 해야하는 비용이 필요하다.
   - Locator는 DB의 id가 어떤 노드(샤드)에 저장되어 있는지를 알려주는 녀석

---



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