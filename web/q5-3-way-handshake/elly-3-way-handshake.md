# 3-way-handshake 란?

~~HTTP~~ TCP 과정에서 클라이언트와 서버가 서로 통신하기 위해 필요한 과정.

서로가 통신할 준비가 됨을 보장한다.

## 통신 과정

![image](https://user-images.githubusercontent.com/19922698/88052829-d017ae80-cb95-11ea-9c7c-3ba74ea0e2ac.png)

![image](https://user-images.githubusercontent.com/19922698/88053806-600a2800-cb97-11ea-8041-cbf8598bfb66.png)



### 1. Client -> Server `TCP SYN`

클라이언트는 서버에 접속을 요청하는 SYN(n) 패킷을 보낸다. 요청 후 서버의 응답을 기다린다. (SYN_SENT)

### 2. Server -> Client `TCP SYN/ACK`

서버는 클라이언트에게 요청을 수락한다는 ACK(m)와 SYN(n+1) 패킷을 보내고, A가 다시 ACK로 응답하길 기다린다. (SYN_RECEIVED)

### 3. Client -> Server `TCP ACK`

클라이언트는 서버에 ACK(m+1)을 보내고, 이제 연결을 성공해 서로 데이터가 오갈 수 있다.



<br/>

`SYN` : 연결을 요청하고 세션을 성립하는데 이용된다.

`ACK` : 받은 시퀀스 번호에 TCP 계층의 길이 또는 양을 더한 것. ACK



<br/>

---

`TCP` : 연결 지향적, session을 만든다.

`UDP` : 비연결 지향적, 최소한의 오류제어 기능만 수행한다.

여기서 TCP가 연결 지향적인 특성을 갖게 해주는 과정이 바로 3-way-handshake 방식이다.

![image](https://user-images.githubusercontent.com/19922698/86619748-1bf32100-bff6-11ea-8d9b-a82c9e1bef73.png)

---

> 참고
>
> https://sleepyeyes.tistory.com/4  
> [https://mindnet.tistory.com/entry/네트워크-쉽게-이해하기-22편-TCP-3-WayHandshake-4-WayHandshake](https://mindnet.tistory.com/entry/네트워크-쉽게-이해하기-22편-TCP-3-WayHandshake-4-WayHandshake)  
> https://real-dongsoo7.tistory.com/73