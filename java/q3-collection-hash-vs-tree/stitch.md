# Hash와 Tree에 대하여

## I Think...

### HashMap

- 자료구조로 Hash를 사용한다.
- 검색하는 속도가 빠르다 - hashcode를 사용하여 데이터를 저장하고 검색하기 때문에 시간이 짧다, best case의 경우 O(1)의 시간 복잡도를 가진다.
- 하지만 데이터의 개수가 늘어나면 검색하는 시간이 늘어난다 - 특정 hashcode에 연결되어 있는 데이터의 양이 많아지면서 검색속도에 영향을 미친다, worst case의 경우 N개의 데이터 중 하나의 hashcode에 연결된 데이터가 K개일 경우 O(K)의 시간 복잡도를 가진다.
- 일반적으로 순서를 보장하기 어렵다 - hashcode를 사용하여 데이터를 저장하고 검색하기 때문에 순서를 기본적으로 보장받기 어렵다, 이때의 순서란 키 값에 대한 순서(정렬)이다. 입력한 순서 X

### TreeMap

- 자료구조로 Tree를 사용한다.
- 적은 양의 데이터의 경우 검색 속도가 느리다 - Tree를 탐색해야 하므로 바로 hashcode로 접근 가능한 Hash에 비해 속도가 느리다.
- 많은 양의 데이터의 경우 검색 속도에 이점을 가진다 - N개의 데이터에 대해 O(logN)의 시간 복잡도를 가져서 속도가 빠르다.
- 순서를 보장한다 - 키 값에 따른 순서(정렬)을 보장할 수 있다.



## Study

### HashMap

- HashMap은 동일한 키를 허용하지 않기 때문에 만약 직접 정의한 객체로 키 값을 사용할 경우 `equals()`와 `hashcode()` 를 정의해야 한다.
- 데이터의 양이 많지 않을 경우 Map에서 자주 사용되는 get() 메서드의 경우 O(log1)의 시간 복잡도로 사용할 수 있으므로 순서(정렬)을 보장받을 필요가 없으면서 자주 검색할 경우 사용하기 좋다.



### TreeMap

- TreeMap은 키로 정렬되기 때문에 키 객체는 비교가 가능해야 한다. 직접 정의한 객체로 키 값을 사용할 경우 **Comparable 인터페이스**를 구현해야 한다.
- 내부가 Red-Black Tree로 구현되어 있음.
- 일관적인 퍼포먼스를 보여주기 좋음.



### LinkedHash

- 내부의 전체적인 구조는 HashMap을 구체화하였다.
- 단지 Entry에 before, after가 존재한다. 따라서 정렬(키 값에 따른)은 보장해주지 않지만 입력한 순서는 보장하여 준다.



## 참고 링크

> [HashMap vs TreeMap vs LinkedHashMap](https://bestalign.github.io/2015/09/20/Java-Map-types-comparison/)
>
> [HashMap, TreeMap 그리고 LinkedHashMap의 차이](https://tomining.tistory.com/168)

