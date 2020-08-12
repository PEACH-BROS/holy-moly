# 데이터베이스 이상

## 데이터베이스 이상(Amonaly)?

*잘못된 DB 설계*에 의한 데이터 중복으로 *공간 낭비*를 넘어 *부작용*을 초래하는 현상.

- 삽입 이상
- 갱신 이상
- 삭제 이상

들이 존재한다.

<br/>

![image](https://user-images.githubusercontent.com/19922698/90041763-b6850500-dd04-11ea-892a-c21eb7505a6d.png)

<br/>

### 삽입 이상 *Insertion Anomaly*

>  불필요한 데이터의 저장 없이는 어떤 정보를 저장하는게 불가능한 문제.

아직 수업을 수강하지 않은 학생을 추가하려면 COURSE_ID가 존재하지 않는다.

기본키에 null을 넣을 수는 없으므로, '미수강' 과목 코드를 새로 만들어 삽입해야 한다.

<br/>

### 갱신 이상 *Update Anomaly*

>  반복된 데이터 중 일부만 갱신해서 데이터가 불일치하게 되는 문제.

야봉이 컴퓨터공학부 -> 음악학부 로 이전한 경우, 3개의 튜플이 모두 바뀌어야 한다.

(컴퓨터공학부의 갯수가 과목 갯수만큼 존재한다.) 이 때, 2개만 바뀌면 야봉은 컴공인지 음악인지 알 수 없다.

<br/>

### 삭제 이상 *Deletion Anomaly*

>  꼭 필요한 데이터를 삭제하지 않고서는 어떤 정보를 삭제하는 것이 불가능한 문제.

모찌는 1개의 과목만 수강하고 있다. 수강 정정 기간에 MEC011101 수업을 드랍한 경우, 위 테이블에 반영하기 위해서는 해당 튜플을 모두 삭제해야 한다. 모찌가 경영햑부라는 정보도 날라가게 되는 것이다.



<br/>

## 🤔 왜 이상 현상이 발생할까?

설계가 잘못되었다!

테이블 설계가 정규화 되어있지 않다!

DB 설계 시 관심사를 분리하지 않아 생기는 문제는 코드 상에서보다 훨씬 치명적이다.

<br/>

## 🤔 어떻게 하면 정규화된 테이블을 만들 수 있을까?

**핵심: 함수적 종속성**

> 하나의 릴레이션에는 하나의 함수적 종속성만이 존재하도록 한다.

X -> Y

X가 Y를 결정한다. Y는 X에 종속된다.

<br/>

Example 1.

![image](https://user-images.githubusercontent.com/19922698/90042485-b5a0a300-dd05-11ea-8cf0-0a1b1075f4fb.png)

<br/>

Example 2.

![image](https://user-images.githubusercontent.com/19922698/90042532-c6e9af80-dd05-11ea-966f-bd90e1f4f0a3.png)







> 참고
>
> https://yaboong.github.io/database/2018/03/09/database-anomaly-and-functional-dependency/  
> https://wkdtjsgur100.github.io/anomaly/  
> 