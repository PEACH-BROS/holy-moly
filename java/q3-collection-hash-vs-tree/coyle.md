### hash collection vs tree collection 

해쉬 콜렉션은 해쉬를 사용하여 자료구조를 탐색하는 구조이고 트리 콜렉션은 트리 구조로 자료가 이루어져 있다. 



### 해쉬 

- 검색 성능 O(1)
- ~~*데이터의 양에  관계 없이 바른 성능을 기대할 수 있다.*~~ **- 구라**



### 해쉬의 오버 플로 해결 방법

- 선형 조사 방법 : 해시 주소에 데이터가 채워져 있을 경우 옆자리를 확인
- 링크드 리스트 사용 : 같은 해쉬 주소를 갖는 데이터를 배열이 아니라 링크드 리스트로 구현
  - 오버플로가 발생해도 다른 해쉬 주소로 이동하지 않고 새로운 노드를 생성해 연결
- 리해싱 : 다른 해시 함수를 사용해서 새로운 해시 주소를 생성하는 것
  - 함수의 성능에 따라 알고리즘의 성능이 달라질 수 있다.



자바의 해쉬맵은 해쉬 테이블을 사용하여 구현되어있고 자바의 트리맵은 레드 블랙 트리로 구현되어있다.







### 레드 블랙 트리란? (해쉬와 비교)

- 순서가 있는 자료 (해쉬는 순서 X)
- 일관성있는 퍼포먼스 (해쉬는 rehashing 될 때 비정상적인 시간 발생)
- 해쉬는 페이지 폴트를 일으킬 수 있다. (페이지 간 전환이 많아지면 느려진다.)
- 연속된 삽입간에 지역성을 유지하기 쉽다. 더 적은 I/O가 발생한다. (해쉬 테이블은 효율적인 look up을 위해 버켓 안의 모든 요소를 스왑할 수 있다.)
- 해쉬는 테이블 리사이징 이슈 : 갑자기 성능이 다운된다.
- 해쉬는 메모리 낭비가 있을 수 있다. (이건 해쉬의 구현에 따른 차이가 있다)
- 트리는 부정확한 검색에 사용될 수 있다. ('a'로 시작되는 모든 것, 범위 검색)



자바를 사용해 개발한다면 대부분의 경우 순서가 필요하면 **TreeMap**, 아니면 **HashMap** 을 쓰는 것이 좋다.



https://aroundck.tistory.com/5945



Tree는 Search에 좋다. 이진 트리 탐색의 경우 O(log N)이다.

그래서 **DBMS, 검색엔진 등에서 빠른 Search를 위해 활용**한다.



B-TREE는 노드의 삽입, 삭제 후에도 균형 트리 유지로 균등한 응답 속도를 보장한다.

그러나 삽입, 삭제 시 트리의 균형을 유지하기 위해 재분배, 합병 등의 복잡한 연산이 필요하다. (이 과정이 없으면 한 쪽으로 Tree 의 Height가 커지면서 효율이 떨어질 수 있다. 균형잡힌 B-TREE가 O(log N)을 보장한다.)



해쉬 맵의 경우 Hash Function을 사용해서 data를 assign 해야할 장소를 지정해서 해당 장소에 값을 assign한다.

만약 HashMap의 사이즈가 100이라면 hash funcion은 0~99 값을 생성해서 값이 저장될 장소를 지정한다.

HashMap의 사이즈가 적절히 선정된 경우에는 Insert, Look up은 O(1)이다. (Collision이 별로 없는 경우 )



Collision이 없는 경우 HashMap의 O(1)은 왠만해서는 O(log N)보다 좋지만 **Collision이 있는 경우는 이야기가 달라진다. 최악의 상황에는 O(n)이 된다.**

따라서 **Tree에 비해 HashMap의 경우 Data의 사이즈의 추측도 필요하다.**



실제 상용 HashMap 구현을 보면 충돌이 많이 일어나면 HashMap의 size up을 시도하는데 여기서 발생하는 성능 이슈도 무시할 순 없다. 기본적으로 모든 element rehash되어 재삽입이 되어야 하기 때문에 O(n)의 시간 복잡도가 발생한다.



또한 Collision이 없어도 HashMap의 경우 Tree 보다 더 많은 메모리를 소모할 수 있다.



마지막으로 B-Tree의 경우 Sort가 되어 있지만, HashMap의 경우 Sorting이 안 되어 있어 Sorting 관련 로직이 필요한 경우 Tree가 더 빠를 수 있다.



### 결론 

메모리가 충분하며 Data양 예측이 가능하고 수정이 그렇게 많지 않은 경우에는 O(1)의 search & insertion을 보장하는 HashMap

그렇지 않다면 O(log N)의 Tree다.

