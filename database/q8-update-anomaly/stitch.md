# 데이터베이스 이상에 대하여

## 이상 : *Anomaly*

좋은 관계형 데이터베이스를 설계하는 목적 중 하나가 정보의 **이상 현상(Anomaly)**이 생기지 않도록 고려해 설계하는 것이다.

데이터베이스를 잘못 설계하면 불필요한 데이터 중복으로 인한 공간낭비를 넘어 부작용을 초래할 수 있다.

이러한 부작용을 이상(Anomaly) 이라고 하는데 이상 현상의 종류로 삽입 이상, 갱신 이상, 삭제 이상이 있다.

이상 현상의 종류로는 **갱신 이상(Modification Anomaly)**, **삽입 이상(Insertion Anomaly)**, **삭제 이상(Deletion Anomaly)**가 있다.

<br/>

## 예시를 통해 살펴본 데이터베이스 이상

| STUDENT_ID | STUDENT_NM | DEPARTMENT   | COURSE_ID | GRADE |
| ---------- | ---------- | ------------ | --------- | ----- |
| 1          | 하이       | 컴퓨터공학과 | CSE001    | A+    |
| 1          | 하이       | 컴퓨터공학과 | CSE002    | A0    |
| 2          | 바이       | 경영학과     | MEC123    | B+    |
| 3          | 헬로       | 기계공학과   | POD012    | C0    |
| 4          | 굿굿       | 시각디자인과 | VDO080    | A0    |

- 한 튜플을 고유하게 식별할 수 있는 속성의 조합은 학번과 과목코드이다.
  - STUDENT_ID, COURSE_ID의 조합을 기본키로 가진다.
- 한 학생은 하나의 DEPARTMENT에만 속할 수 있다.



### 갱신 이상 : *Modification Anomaly*

하이가 컴퓨터공학과에서 전자공학과로 옮기게 될 경우 '컴퓨터공학과'의 갯수는 과목코드의 갯수만큼 있기 때문에 모두 '전자공학과'로 변경해주어야 한다.

중복 튜플 중 일부만 변경하여 데이터가 불일치하게 되는 모순의 문제를 **갱신 이상** 이라고 한다.

### 삽입 이상 : *Insertion Anomaly*

아직 수업을 하나도 수강하지 않은 학생이 있다고 하자. 현재 KEY로는 학번과 과목 코드를 지정해 놓았다.

기본 키로 쓰이는 컬럼은 NULL이 될 수 없으므로 그 학생은 테이블에 추가할 수 없다.

이 학생을 테이블에 삽입하려면 '미수강'과 같은 과목 코드를 새로 만들어서 삽입해야 한다.

새 데이터를 삽입하기 위해 불필요한 데이터도 함께 삽입해야 하는 문제를 **삽입 이상**이라고 한다.



### 삭제 이상 : *Deletion Anomaly*

헬로는 현재 1개의 과목(POD012)만 수강하고 있다. 헬로가 수강 정정기간에 POD012라는 수업이 듣기 싫어져서 드랍하는 경우, 테이블에 반영하기 위해서는 헬로에 대한 행을 모두 삭제하게 된다.

수강취소를 반영하기 위해 학생등록정보를 모두 날리게 되는 것이다.

튜플을 삭제하면 꼭 필요한 데이터까지 함께 삭제되는 데이터 손실의 문제를 **삭제 이상**이라고 한다.

<br/>

## 데이터베이스 이상 해결방법

이러한 데이터베이스 이상이 발생하는 이유는 정규화가 되어 있지 않은 테이블 설계 때문이다.

코딩할 때에도 관심사를 분리하면 코드의 재사용성과 유지보수의 편의성이 높아지는 것처럼, 데이터베이스 설계에서도 비슷한 원칙이 적용된다.

데이터베이스 설계의 경우 관심사를 분리하지 않아 생기는 문제는 코드단에서의 문제보다 훨씬 더 치명적이다.



## 참고 링크

> [데이터베이스 이상(Anomaly)](https://dodo000.tistory.com/19)
>
> [데이터베이스 이상 현상(Anomaly) 개념과 예시](https://wkdtjsgur100.github.io/anomaly/)
>
> [데이터베이스 정규화 - 이상현상 & 함수적 종속성](https://yaboong.github.io/database/2018/03/09/database-anomaly-and-functional-dependency/)
>
> [데이터베이스 이상 현상(Anomaly)](https://1000hg.tistory.com/22)
>
> [이상(Anomaly) 및 정규화(Normalization)](https://ppiyo5.tistory.com/14)
