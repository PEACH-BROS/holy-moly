# ArrayList와 LinkedList의 차이를 설명해보세요.

[엘리의 ArrayList vs LinkedList](elly-arraylist-vs-linkedlist.md)

[코일의 ArrayList vs LinkedList](coyle.md)

[비밥의 ArrayList vs LinkedList](bebop.md)

---

## 내부 구현 자료구조

:rocket: ArrayList는 내부적으로 배열을 들고 있다.

LinkedList는 노드가 이전 노드, 다음 노드를 알고 있다.

## 성능 차이

### 삽입/삭제

**ArrayList** : 임시 배열을 통해 삽입/삭제를 진행한다. 삽입/삭제 작업이 많이 일어날 경우, 대량의 복사가 일어나서 성능 저하를 일으킬 수 있다. `최악의 경우, O(N)`

#### Java8 ArrayList `add(E e)` 코드

```java
    private void grow(int minCapacity) {
        // overflow-conscious code
        int oldCapacity = elementData.length;
        int newCapacity = oldCapacity + (oldCapacity >> 1);
        if (newCapacity - minCapacity < 0)
            newCapacity = minCapacity;
        if (newCapacity - MAX_ARRAY_SIZE > 0)
            newCapacity = hugeCapacity(minCapacity);
        // minCapacity is usually close to size, so this is a win:
        elementData = Arrays.copyOf(elementData, newCapacity);
    }
```

위와 같이 기존 수용량을 넘어서면 newCapacity를 생성해서 배열의 크기를 늘리고 복사한다.

최악의 경우`O(N)` : 첫 번째 index에 삽입할 경우, 뒤로 하나씩 밀린다. 첫 번째 index의 데이터를 삭제할 경우, 뒤의 데이터들이 앞으로 하나씩 복사되어 당겨진다)

![image](https://user-images.githubusercontent.com/19922698/87749640-2900e300-c834-11ea-9409-de523ce49b5c.png)

**LinkedList** : ArrayList보다 훨씬 빠르다. 이전/다음 노드의 참조값만 변경하면 된다. `O(1)`

![image](https://user-images.githubusercontent.com/19922698/87749714-682f3400-c834-11ea-8b3a-a29536802efd.png)

### 조회

**ArrayList** : 각 데이터가 인덱스를 갖고 있어 한 번에 참조가 가능하다. `O(1)`

**LinkedList** : 불리하다. 모든 요소를 탐색한다. `최악의 경우, O(N)`



## ArrayList와 비교했을 때 LinkedList의 장/단점

|                    장점                    |                      단점                       |
| :----------------------------------------: | :---------------------------------------------: |
|       자료의 삽입과 삭제가 용이하다.       | 포인터의 사용으로 인해 저장 공간의 낭비가 있다. |
| 리스트 내에서 자료의 이동이 필요하지 않다. |              알고리즘이 복잡하다.               |
|   사용 후 기억 장소의 재사용이 가능하다.   |     특정 자료의 탐색 시간이 많이 소요된다.      |
| 연속적인 기억 장소의 할당이 필요하지 않다. |                                                 |

**ArrayList는 읽기 작업에 강점**을 가지는 반면 **추가/삭제 작업에 약점**을 가지는 구현체이다.  
**LinkedList는 추가/삭제 작업에 강점**을 가지는 반면 **읽기 작업에 약점**을 가지는 구현체이다.

