# TCP vs UDP

### TCP와 UDP?

4계층(Transport Layer)에서 데이터를 보내게 위해 사용하는 프로토콜

![image](https://user-images.githubusercontent.com/19922698/89292268-09bcdf00-d697-11ea-9890-ca1d99d671bb.png)<br/>

![image](https://user-images.githubusercontent.com/19922698/89294857-09bede00-d69b-11ea-9c82-4e336be2a261.png)

| TCP (Transmission Control Protocol)                          | UDP (User Datagram Protocol)                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **연결형** 프로토콜 (서버:클라이언트가 1:1)                  | **비연결형** 프로토콜 (서버, 클라이언트 소켓의 구분이 없다)  |
| **가상 회선 방식** (발신지와 수신지를 연결해 패킷을 전송한다.) | **데이터그램 방식** (패킷을 서로 다른 경로로 전송한다.)      |
| 3-way-handshaking으로 연결하고, 4-way-handshaking으로 해제한다. | 연결을 설정/해제하는 과정이 없음. 각 패킷은 독립적으로 처리된다. |
| 연결형 서비스 -> 높은 신뢰성 보장(전송 순서 보장)            | **신뢰성 보장 X** (전송 순서 바뀔 수 있음)                   |
| 데이터의 흐름/혼잡 제어도 한다 -> **속도가 느리다.**         | 흐름/혼잡 제어 없다 -> **속도가 빠르고** 네트워크 부하가 적다. |
| TCP 패킷 : segment                                           | UDP 패킷 : datagram                                          |
| *HTTP, 메일, 파일 전송*같은 신뢰성이 보장되어야 하는 서비스에 주로 사용된다. | *실시간 스트리밍* 등 신뢰성보다는 연속성이 중요한 서비스에 주로 사용된다. |

<br/>



### 흐름 제어와 혼잡 제어?

흐름 제어 : 데이터 송신 - 수신 간의 데이터 처리 속도 조절. 수신자의 버퍼 오버플로우를 방지한다.

혼잡 제어 : 네트워크 내의 패킷 수가 넘치지 않게 조절.



<br/>

### TCP의 전송 방식



![image](https://user-images.githubusercontent.com/19922698/89294740-e3993e00-d69a-11ea-97e0-ce1606b1a183.png)

<br/>

### UDP의 전송 방식

![image](https://user-images.githubusercontent.com/19922698/89294776-ef850000-d69a-11ea-8ce6-cd6fa7ec3de6.png)

![image](https://user-images.githubusercontent.com/19922698/89293495-f90d6880-d698-11ea-95c2-4144abb3b664.png)

<br/>

### TCP가 높은 신뢰성을 보장하는 이유는?





> 참고
>
> https://mangkyu.tistory.com/15  
> [https://velog.io/@hidaehyunlee/TCP-%EC%99%80-UDP-%EC%9D%98-%EC%B0%A8%EC%9D%B4](https://velog.io/@hidaehyunlee/TCP-와-UDP-의-차이)