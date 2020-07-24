### 3-way-handshake란?



신뢰성있는 TCP 통신을 하기 위해 서로통신 가능한 상태인지 확인하는 절차로 클라이언트가 syn를 보내면 서버에서 syn와 ack를 보내고 마지막으로 클라이언트가 syn를 보내 세션 수립하는 과정입니다.



### **TCP 3-way Handshake 란?**

**TCP 3 Way Handshake는 TCP/IP프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전에** 

**먼저** **정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다..**



### 과정

![](https://t1.daumcdn.net/cfile/tistory/225A964D52F1BB6917)



![](https://t1.daumcdn.net/cfile/tistory/99AE67425B32FEE81B)



2에서 서버가 syn/ack를 보낼 때 ack에 1에서 클라이언트가 보낸 syn + 1하여 되돌려 준다. 이와 마찬가지로 3에서 client는 ack에 2에서 전달받은 syn에 + 1을 해서 돌려준다.



