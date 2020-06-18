

# hash와 tree



크게 **탐색**과 **정렬(순서 보장)** 에서 차이가 난다.



## HashMap

HashMap은 내부가 **Node<K, V>의 배열**(`Node<K, V>[] table;`)로 되어 있다.

![image](https://user-images.githubusercontent.com/19922698/85052092-5590f300-b1d3-11ea-8bc7-a7e3cbd53e05.png)

HashMap은 값을 저장할 때 받아온 key, value 값과 해당 key를 hashing한 값을 함께 저장한다.

![image](https://user-images.githubusercontent.com/19922698/85049240-5e7fc580-b1cf-11ea-911f-d0e49a2bdfd6.png)

HashMap은 탐색 시 key의 hash값을 사용해 배열에 접근한다. 따라서, **탐색 시, 시간 복잡도가 O(1)이다.**

![image](https://user-images.githubusercontent.com/19922698/85048746-ba961a00-b1ce-11ea-8821-0a3878f991b0.png)





## TreeMap

TreeMap은 내부가 **Entry<K, V>의 트리**(`Entry<K, V> root;`)로 되어 있다.

![image](https://user-images.githubusercontent.com/19922698/85052235-8a04af00-b1d3-11ea-8166-fa98c1afe0b3.png)



TreeMap은 값을 저장할 때 **key 값을 기준으로 정렬**하는 과정을 거친다. ![image](https://user-images.githubusercontent.com/19922698/85050302-d69abb00-b1d0-11ea-90f2-017b7597ef59.png)

![image](https://user-images.githubusercontent.com/19922698/85051010-e9fa5600-b1d1-11ea-8c76-fc631897356b.png)

아래와 같이 key값을 기준으로 정렬된다.

![image](https://user-images.githubusercontent.com/19922698/85050818-a273ca00-b1d1-11ea-965c-030d19d07799.png)





또, **레드-블랙 트리의 탐색/삽입/삭제 시간 복잡도는 O(log n)이다.**

따라서, 특정 노드(Entry)에 접근하기 위해서는 O(log n)의 시간 복잡도가 필요하다.







> 참고
>
> https://12216715011126.tistory.com/55  

