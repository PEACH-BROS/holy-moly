# 인덱스가 뭐죠?

인덱스는 SortedList 자료구조의 형태를 띄고 있기 때문에 항상 정렬된 상태를 가지고 있다.

- 저장될 때마다 값을 정렬하느라 느리다.
  - CUD 의 처리가 느리다.
- 대신 빠르게 값을 찾을 수 있다.
  - R 의 처리는 빠르다.

결국 인덱스를 생성한다는 것은 CUD의 성능과 R의 성능을 트레이드 오프하는 것이다.

인덱스에 사용되는 알고리즘은 B+-Tree 알고리즘이다.  
여기서 B는 Balanced를 의미한다.

> B-Tree는 최상위 루트 노드, 브랜치 노드, 최하위 리프 노드로 이루어져 있다.  
> 이중 리프 노드에 실제 데이터의 주소값이 적혀있다.

![image](https://user-images.githubusercontent.com/13347548/89715460-aad1cf80-d9e0-11ea-9240-7a191e5788be.png)

> 인덱스의 키값은 정렬되지만 (B-Tree의 특성상) 데이터 파일의 레코드는 그렇지 못하다.  
> INSERT만 발생하면 정렬된 상태일 수 있지만, DELETE가 발생하면 INSERT는 빈공간을 찾아 INSERT하기 때문이다.  
> 그런데 InnoDB의 경우 PK를 클러스터링 하기 때문에 PK기준으로 순차적으로 정렬되서 저장된다.

인덱스의 삭제는 단순히 인덱스의 키를 찾아 삭제하면 끝난다.

**인덱스의 키를 변경**하는 경우 인덱스 키에 해당하는 값을 변경하는 것이 아니라 **인덱스의 키를 삭제후 새로운 키를 추가한다.**

인덱스는 단순히 R 작업에서만 쓰이는 것이 아니며 U, D 작업을 하기 위한 ROW 탐색을 할때도 사용된다.

### 선택도(기수성) - Cardinality

기수성이라는 말은 모든 인덱스 중 유니크한 인덱스의 값을 나타내는 말이다.  
즉, 10개의 인덱스 키가 있다고 했을때, 유니크한 값의 갯수가 4라면 기수성은 4이다.  
이러한 기수성이 높을수록 선택 대상이 줄어들어서 빠르게 검색할 수 있다.

인덱스를 적용하면 무조건 읽기의 성능이 향상될것이라 오해 할 수 있는데 그렇지 않다.  
인덱스를 이용한 읽기 작업의 인덱스를 이용하지 않은 읽기작업보다 비용이 더 들어가기 때문이다.  
따라서 인덱스를 통해 읽어야 하는 레코드의 수가 20~25%를 넘어서면 인덱스를 이용한 읽기 작업이 오히려 더 성능을 떨어 뜨릴수 있다.  
따라서 데이터 베이스 옵티마이저가 이를 판단해서 인덱스를 사용하기도 사용하지 않기도 한다.