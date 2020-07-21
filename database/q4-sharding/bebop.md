# 샤딩이 뭔지 알아봅시다.

먼저 파티셔닝에 대해 짚고 넘어가자

파티셔닝은 성능을 위해 테이블을 다시 여러개의 테이블로 쪼개는 행위를 말한다.

### 수직 파티셔닝

파티셔닝은 수직 파티셔닝과 수평 파티셔닝으로 나뉘게 되는데 수직 파티셔닝은 테이블의 컬럼을 분리하여 새로운 테이블로 분리하는 방식이다.

![image](https://user-images.githubusercontent.com/13347548/87961313-7f9a4580-caf0-11ea-8bd6-299aea914cc7.png)

### 수평 파티셔닝

샤딩은 수평 파티셔닝에 속한다.

수평 파티셔닝은 데이터를 수평으로 분리하여 테이블의 복제본을 여러개 만들고 수평으로 분리된 1개의 작은 테이블을 샤드라 부른다.  
그리고 데이터를 저장할 때 어떠한 샤드에 저장될지 샤드키를 기준으로 분리하여 저장한다.

![image](https://user-images.githubusercontent.com/13347548/87961386-98a2f680-caf0-11ea-99c6-b0d257d97c26.png)

즉, 기존 테이블에 1~100까지 row가 있었다면 1~10을 하나의 샤드로 11~20을 다른 하나의 샤드로 분리하여 저장하는 방식이다.

![image](https://user-images.githubusercontent.com/13347548/87961427-a48eb880-caf0-11ea-86e0-f47a6a2df936.png)

이렇게 샤딩을 하면 보통 다음과 같은 이점이 있다.

1. 가용성 : 물리적인 파티셔닝을 통해 전체 데이터의 훼손 가능성이 줄어든다.
2. 관리 용이성 : 큰 Table을 제거하여 관리가 쉽다.
3. 성능 : 특정 Query의 성능이 올라간다. 특히 대용량 Write 작업이 좋아진다.

장점이 있는 만큼 단점도 존재하는데 이러한 장점과 단점은 샤드 키를 만드는 전략에 따라 서로 다르다.

```
샤딩 종류
Hash Sharding

Dynamic Sharding

Entity Group

Pitfall
```

종류에 따른 장단점이 궁금하면 각자 찾아보자 ^^...


