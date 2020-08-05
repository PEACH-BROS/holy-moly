내 답변

 Unique key는 key로 row들을 고유하게 분리할 수 있다. Unique key는 여러 개의 키가 합쳐져 이뤄질 수도 있다. 중요한 점은 key 혹은 key들로 row들을 구별해낼 수 있다는 점이다. Primary key는 Unique key의 성질인 식별성을  만족해야한다. 식별성뿐만 아니라 하나의 성질을 더 만족해야하는데 이는 유일성이다. 여러 Unique key 중 하나의 값만 Primary key로 존재할 수 있다는 것이다.

⇒ 식별성 == 유일성

그렇다면 후보키는 유니크 키인가? 맞다. 그럼 후보키랑 유니크 키의 다른 점은 무엇일까 ? 

Unique key ⇒ 중복 방지 + Null 가능

Primary key ⇒ 중복 방지 + Not Null + 오직 하나

Unique key는 제약 조건이다. 제약 조건이란 테이블 단위에서 데이터의 무결성을 보장해주는 규칙이다. 즉, 테이블에 데이터가 입력, 수정, 삭제되거나 테이블이 삭제, 변경될 경우 잘못된 트랜잭션이 수행되지 않도록 결함을 유발할 가능성이 있는 작업을 방지하는 역할을 한다.

Primary key는 목적이 다른 테이블과의 관계 설정에 존재한다.

Unique Index란 뭘까 ? 

- Unique key와 Primary key를 생성하면 자동으로 생성이 된다고 하는데 뭘 말하는 거지 ?
- 그냥 Unique Key 혹은 Primary key를 생성하면 그에 따른 index 파일이 생성된다는 것이다. 색인 파일이므로 탐색의 시간을 줄여준다. [출처]([https://lalwr.blogspot.com/2016/02/db-index.html](https://lalwr.blogspot.com/2016/02/db-index.html))

## 정리

Unique key는 null을 허용하지만 중복을 허용하지 않는 **제약조건**이다. 그러나 PK는 Not null하며 중복을 허용하지 않는다. PK는 오직 하나이며 그에 대한 목적이 다른 테이블과의 관계 설정에 있다. Unique Key와 PK는 모두 자동으로 Unique index를 생성하여 탐색의 시간을 줄여준다.
