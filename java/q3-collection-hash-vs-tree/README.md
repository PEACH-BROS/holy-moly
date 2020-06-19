# hash와 tree의 차이를 설명해보세요.

[비밥의 hash vs tree](bebop.md)

[스티치의 hash vs tree](stitch.md)

[덤벙덤코벙덤벙덤일벙의 hash vs tree](coyle.md)

[엘리의 좌충우돌 hash vs tree 탐색기](elly-collection-hash-vs-tree.md)





## Tree

Search에 좋다. 이진 트리 탐색의 경우 O(log N)이다.

Hash는 아이템이 추가되었을 때, 공간(사이즈)을 늘리고, 리해싱*(아예 hashmap을 새로 할당해버림)*하는 과정이 필요하다.



## HashMap

*자료구조로 HashTable을 사용한다.*



객체의 `hashcode()` 메서드로 hash값을 추출한 뒤 배열에 접근하기 때문에 시간복잡도가 `O(1)`이 된다.

이때 hash값의 충돌이 일어난다면(=아이템 개수가 많아지면) linked list 형태로 이루어진 V를 탐색하기 때문에 시간복잡도는 O(K)이 된다. 특정 hashcode에 연결되어 있는 데이터의 양이 많아지면서 검색속도에 영향을 미친다, worst case의 경우 N개의 데이터 중 하나의 hashcode에 연결된 데이터가 K개일 경우 O(K)의 시간 복잡도를 가진다.

일반적으로 순서를 보장하기 어렵다 - hashcode를 사용하여 데이터를 저장하고 검색하기 때문에 순서를 기본적으로 보장받기 어렵다, 이때의 순서란 **키 값에 대한 순서(정렬)**이다. 입력한 순서가 아님!

HashMap은 동일한 키를 허용하지 않기 때문에 만약 직접 정의한 객체로 키 값을 사용할 경우 `equals()`와 `hashcode()` 를 정의해야 한다.



## TreeMap

내부가 Red-Black Tree로 구현되어 있음.

이진 탐색을 이용하기 때문에 시간 복잡도가 `O(logN)`이 된다.  

**적은 양의 데이터의 경우** 검색 속도가 느리다 - Tree를 탐색해야 하므로 바로 hashcode로 접근 가능한 Hash에 비해 속도가 느리다.

**많은 양의 데이터의 경우** 검색 속도에 이점을 가진다 - N개의 데이터에 대해 O(logN)의 시간 복잡도를 가져서 속도가 빠르다.

순서를 보장한다 - **키 값에 따른 순서(정렬)**을 보장할 수 있다. (ex. 오름차순)

![image](https://user-images.githubusercontent.com/19922698/85050818-a273ca00-b1d1-11ea-965c-030d19d07799.png)

TreeMap은 키로 정렬되기 때문에 키 객체는 비교가 가능해야 한다. 직접 정의한 객체로 키 값을 사용할 경우 **Comparable 인터페이스**를 구현해야 한다.



## LinkedHashMap

내부의 전체적인 구조는 HashMap을 구체화하였다.

단지 Entry에 before, after가 존재한다. 따라서 **키 값에 따른 정렬**은 보장해주지 않지만, 입력한 순서는 보장해준다.





## 어떤 걸 쓰지?

보통 컬렉션 내부에 아이템이 적으면 hash의 성능이 좋고, 아이템이 많으면 tree가 성능이 더 좋은편이다.

Collision이 없는 경우 HashMap의 O(1)은 왠만해서는 O(log N)보다 좋지만 **Collision이 있는 경우는 이야기가 달라진다. 최악의 상황에는 O(N)이 된다.**





> **이슈**
>
> 삽입/삭제 작업이 많은 경우엔 뭘 써야 할까?
>
> ​	해시는 삽입/삭제/검색 최고 O(1), 최악 O(N)  
> ​	트리는 삽입/삭제/검색 항상 O(log N)
>
> **결론 : N의 사이즈에 따라 다르다.**

