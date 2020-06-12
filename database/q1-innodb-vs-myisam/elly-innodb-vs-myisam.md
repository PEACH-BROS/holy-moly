# MySQL의 Storage Engine의 양대산맥, InnoDB vs MyISAM

## InnoDB

- 트랜잭션을 지원한다. (트랜잭션-세이프 스토리지 엔진에 해당)
  - Commit, Rollback, 장애복구, row-level locking(행 단위 lock), 외래키 등 다양한 기능을 지원한다.
- Row-Level Lock(행 단위 Lock)을 사용한다.
  - 변경 작업(INSERT, UPDATE, DELETE) 속도가 빠르다.

- 데이터 무결성이 보장된다.
- 외래키, 제약조건 생성이 가능하다.
- 동시성 제어가 된다.



> InnoDB는 트랜잭션 처리가 필요한 작업을 수행하며, 데이터 입력 및 수정과 같이 변경이 빈번한 높은 퍼포먼스를 요구하는 대용량 사이트에서 효율적이다.





## MyISAM

- 트랜잭션을 지원하지 않는다. (Non-transactional-safe 테이블을 관리한다.)
  - Rollback이 안 된다.

- InnoDB에 비해 별다른 기능이 없다. = 데이터 모델 디자인이 단순하다. = 작업 속도(특히 SELECT)가 InnoDB보다 빠르다.
- Full-text 인덱싱이 가능해 검색하고자 하는 내용에 대해 복합검색이 가능하다.
- 데이터 무결성이 보장되지 않는다. (개발자나 DBA가 처리해야 함)
- Table-level lock을 사용해서 쓰기 작업(INSERT, DELETE)이 느리다.
  - 변경을 많이 요하는 작업에서는 MyISAM을 권하지 않는다.



> MyISAM은 트랜잭션 처리가 불필요하며, SELECT 속도가 빠르므로 주로 조회 작업이 많은 경우에 사용된다.







---

> 참고
>
> https://ojava.tistory.com/25