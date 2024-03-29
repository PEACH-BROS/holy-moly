### InnoDB와 MyISAM의 차이를 설명해주세요.

그게 도대체 뭐죠...? 

DB 스토리지 엔진이래요~

### DB 스토리지 엔진은 뭐야 ? 
스토리지 엔진은 DB에서 데이터를 어떠한 방식으로 저장하고 접근할 것인지에 대한 기능을 제공한다.
스토리지 엔진의 특성에 따라 데이터 접근이 얼마나 빠른지, 얼마나 안정적인지, 트랜잭션 등의 기능을 제공하는지 등의 차이점이 발생한다.

### MyISAM vs InnoDB
 트랜잭션 필요, 대용량의 데이터를 다루기 위해서는 InnoDB가 효율적이고, 트랜잭션 처리 X에 읽기 전용이 많은 서비스일 경우 MyISAM 엔진이 효율적이다.

### MyISAM
 MyISAM는 항상 테이블에 ROW COUNT를 가지고 있기 때문에 SELECT count(*) 명령 시 빠르고, SELECT 명령시에도 빠른 속도를 자랑한다. 또한 풀 텍스트 인덱스를 지원하는데  여기서 풀텍스트 인덱스는 아주 긴 글에서도 id로 쓰일 값을 지정해서 인덱스로 가지고 있다가 조회할 때 그 id로 비교하는 것이다.
 단점으로는 row level locking 을 지원하지 못해, CRUD 시 해당 Table 전체에 lock이 걸린다. (row의 수가 커지면 커질수록 속도는 느려진다.)

### MyISAM (요약)
 - MySQL의 기본 스토리지 엔진 
 - Full-Text 인덱스를 지원하며 특정 인덱스에 대해 메모리 캐쉬를 지원한다.
 - 트랜잭션은 미지원
 - 테이블 레벨의 락을 지원
	 - 잦은 변경 및 삭제에는 좋은 서능이 나오지 못하나 데드락 발생은 예방 가능

### Inno DB
 row level locking이 지원된다. 그렇기 때문에 트랜 잭션 처리가 필요한 대용량 데이터에 유리한 점이 있다. 예를 들자면 사용자의 CRUD가 많은 서비스에 유리할 것 같다. 트랜잭션, 복구 기능이 가능하다.

단점으로는, MyISAM의 장점인 풀텍스트 인덱스를 지원하지 못한다.

 

### InnoDB 유리
 - 대용량 데이터 컨트롤
 - 트랜잭션 관리 필요
 - 복구 필요
 - **정렬등의 구문**이 들어가는 경우

### MyISAM 유리
 - **읽기 위주**의 작업만 필요한 경우
 - 전문 검색이 필요한 경우
 - 트랜잭션이나 복구 등이 필요 없을 경우
 - 한번에 대량의 데이터를 입력하는 배치성 테이블
