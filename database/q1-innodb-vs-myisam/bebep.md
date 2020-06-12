# InnoDB와 MyISAM의 차이를 설명해주세요.

## MyISAM

MySQL 5.5 버전 이전의 기본 스토리지 엔진이다.

MyISAM은 Table 단위의 락을 지원한다.  

따라서 INSERT, UPDATE, DELETE 보다 SELECT 가 많이 발생하는 환경에서 더 좋은 성능을 나타낸다.  

보통 트래픽이 많이 발생하는 구조에 적합하며, 정적인 데이터 혹은 로그를 다루는 구조에 적합하다.

>  current insert기능이 read시에 insert가 가능하게 하므로 로그 table에 사용될 수 있다.

하지만 다음과 같은 단점들이 존재한다.

- 트랜잭션 지원의 부재
  - ACID 특성을 지켜주지 못한다.
  - 롤백을 못한다.
- 외래키 지원의 부재
  - 데이터 참조 무결성 보장의 부재

## InnoDB

InnoDB는 이전 MyISAM의 단점들을 개선한 스토리지 엔진이다.

트랜잭션을 지원해주고 외래키를 사용할 수 있으며, Row단위의 락을 지원해준다.  
따라서 갱신 위주의 요청이 많이 발생하고, 민감한 데이터(돈)를 다루는 구조에 적합하다.

- 트랜잭션의 지원
  - ACID 특성을 보장
  - 롤백 가능
- 외래키 지원
  - 참조 무결성을 보장해준다.

- Row단위의 락을 지원 
  - MyISAM보다 빠른 INSERT, UPDATE, DELETE 연산을 할 수 있다.

위와 같은 여러가지 기능을 지원해 주기 때문에 시스템 자원을 많이 사용한다.

다만 MyISAM이 Full-Text 인덱스를 지원해주는 반면 InnoDB는 지원해주지 않는다.