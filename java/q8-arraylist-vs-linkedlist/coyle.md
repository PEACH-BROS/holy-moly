# LinkedList vs ArrayList



## ## 답변



LinkedList는 하나의 아이템이 다른 아이템을 가리키고 있는 형태의 List이고 ArrayList는 물리적으로 바로 옆 메모리에 존재하게 하여 아이템들을 관리하는 자료구조입니다. LinkedList의 삽입, 삭제는 O(1)이 소요되나 탐색은 O(N)이 소요됩니다. ArrayList는 탐색은 O(1)이 소요되나 삽입과 삭제는 값을 지우고 다시 물리적으로 옆 메모리에 존재하게 하기위해 아이템들을 옮기는 과정에서 O(N)이 소요됩니다. 그러므로 우린 다량의 삽입과 삭제는 LinkedList를 사용하고 인덱스에 바로 접근하는 경우가 많은 경우에는 ArrayList를 사용하면 좋습니다.



### ArrayList와 비교했을 때 LinkedList의 장/단점

|                                            |                                                 |
| :----------------------------------------: | :---------------------------------------------: |
|                    장점                    |                      단점                       |
|       자료의 삽입과 삭제가 용이하다.       | 포인터의 사용으로 인해 저장 공간의 낭비가 있다. |
| 리스트 내에서 자료의 이동이 필요하지 않다. |              알고리즘이 복잡하다.               |
|   사용 후 기억 장소의 재사용이 가능하다.   |     특정 자료의 탐색 시간이 많이 소요된다.      |
| 연속적인 기억 장소의 할당이 필요하지 않다. |                                                 |







