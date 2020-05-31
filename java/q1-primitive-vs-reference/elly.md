## 원시타입 vs 참조타입



![Primitive vs Reference Type in Java](https://1.bp.blogspot.com/-bpbdwrvRSaw/VflqVT6GaxI/AAAAAAAADxU/BSi7bF7ef34/s640/Primitive%2Bvs%2BReference%2BType%2BJava.png)



### 원시 타입

- 흔히 byte, char, short, int, long, float, double, boolean 8개를 말한다.

- 메모리의 스택 영역에 **실제 값**을 저장한다.

  ```java
  int port = 8080; //원시 타입: 디버깅해보면 port에 8080이 들어있다.
  ```

- null이 될 수 없다.

  ❗️Request/Response dto나 DB의 entity에서 원시 타입을 지양하는 이유이다. `int`의 경우, 값이 비어있으면 디폴트로 0이 할당된다. 그럼 값이 0인 경우와 동일한 로직을 수행해버린다.

- `=`으로 할당할 경우, 값이 복사된다. (Call by Value)

  ```java
  int i = 10;
  int j = i;
  j++; //i는 영향을 받지 않는다.
  ```

- `==`으로 비교한다.

- 메소드의 return type이 원시 타입일 경우, 값이 리턴된다.
- 참조 타입보다 적은 메모리를 차지한다.





### 참조 타입

- 배열, 열거 타입, 클래스, 인터페이스 등이 포함된다. (원시 타입을 제외한 나머지)

- 메모리의 힙 영역에 실제 객체가 저장되고, 스택 영역에는 이를 참조하는 참조값, 즉 **힙 영역의 주소**가 저장된다.

  ```java
  int port = 8080; //원시 타입: 디버깅해보면 port에 8080이 들어있다.
  String host = "localhost"; //참조 타입: 디버깅해보면 String@782와 같은 값이 들어있다.~~(그래야 하는데?! )~~
  ```

  ![image](https://user-images.githubusercontent.com/19922698/83361139-682cbf00-a3c1-11ea-8aa3-641aaf3fd92e.png)

- null이 될 수 있다.

  ❗️NPE가 발생할 수 있다.

- `=`으로 할당할 경우, 참조하고 있는 주소가 복사된다. (Call by Reference)

  ```java
  List<String> a = new ArrayList(2);
  List<String> b = a;
  b.add("newElement"); //a에도 "newElement"가 들어간다. 주소를 복사했기 때문에 같은 객체를 가리키고 있기 때문.
  ```

- `==`로 비교할 경우, 주소가 비교된다. `equals()`를 override해서 사용하길 권장한다.
  - 단, null은 객체가 아니라서 equals() 메소드가 없으므로 ==으로 비교할 것.

- 메소드의 return type이 참조 타입일 경우, 마찬가지로 참조하고 있는 주소가 리턴된다. **즉, 메소드 내에서 선언된 참조 타입 변수는 해당 변수가 리턴되거나, 다른 변수에 할당되는 경우 메소드가 끝나도 살아있을 수 있다. (힙 영역에 저장되기 때문!)**

- Object metadata(e.g. object header)를 포함하기 때문에 원시 타입보다 많은 메모리를 차지한다. (int가 차지하는 메모리 < Integer가 차지하는 메모리)



참고 : https://javarevisited.blogspot.com/2015/09/difference-between-primitive-and-reference-variable-java.html