# ArrayList vs LinkedList

:rocket: ArrayList는 내부적으로 배열을 들고 있다.

LinkedList는 노드가 이전 노드, 다음 노드를 알고 있다.



### 삽입/삭제

**ArrayList** : 임시 배열을 통해 삽입/삭제를 진행한다. 삽입/삭제 작업이 많이 일어날 경우, 대량의 복사가 일어나서 성능 저하를 일으킬 수 있다. `최악의 경우, O(N)`



최악의 경우 : 첫 번째 index에 삽입할 경우, 뒤로 하나씩 밀린다. 첫 번째 index의 데이터를 삭제할 경우, 뒤의 데이터들이 앞으로 하나씩 복사되어 당겨진다)

![image](https://user-images.githubusercontent.com/19922698/87749640-2900e300-c834-11ea-9409-de523ce49b5c.png)

**LinkedList** : ArrayList보다 훨씬 빠르다. 이전/다음 노드의 참조값만 변경하면 된다. `O(1)`

![image](https://user-images.githubusercontent.com/19922698/87749714-682f3400-c834-11ea-8b3a-a29536802efd.png)



### 조회

**ArrayList** : 각 데이터가 인덱스를 갖고 있어 한 번에 참조가 가능하다. `O(1)`

**LinkedList** : 불리하다. 모든 요소를 탐색한다. `최악의 경우, O(N)`









> 참고
>
> https://www.holaxprogramming.com/2014/02/12/java-list-interface/  
> 