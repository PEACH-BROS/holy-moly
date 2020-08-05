# TCP vs UDP

[엘리의 TCP vs UDP](elly-tcp-vs-udp.md)

[비밥의 TCP vs UDP](bebop.md)

[스티치의 TCP vs UDP](stitch.md)

---

 `TCP`와 `UDP`는 TCP/IP의 **전송계층(4계층)에서 사용되는 프로토콜**이다.

<br/>

## TCP(Transmission Control Protocol)

데이터의 순서를 보장(제어)해야 하기 때문에 패킷에 순서를 주어야하고 재조립을 해야한다. 

Full-Duplex(동시에 양방향으로 왔다갔다 가능), Point-to-Point(1:1로 연결 가능)



❗️ 데이터를 송신하는 과정에서 제대로 송신이 되었는지 계속해서 확인(ex. `Sliding Window 알고리즘`)하고 제어 해야한다.

**->** **흐름 제어가 가능해진다.**

![image](https://user-images.githubusercontent.com/13347548/89293954-bdbf6980-d699-11ea-961d-21ff9527a31f.png)

![image](https://user-images.githubusercontent.com/19922698/89294740-e3993e00-d69a-11ea-97e0-ce1606b1a183.png)

<br/>

## UDP(User Datagram Protocol)

ex. DNS -> 브로드캐스트로 쏴버림

소켓 대신 IP를 기반으로 데이터 전송

서버와 클라이언트는 1대 1, 1대 N, N대 M등으로 연결

![image](https://user-images.githubusercontent.com/19922698/89294776-ef850000-d69a-11ea-8ce6-cd6fa7ec3de6.png)

<br/>

---

| TCP (Transmission Control Protocol)                          | UDP (User Datagram Protocol)                                 |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| **연결형** 프로토콜 (서버:클라이언트가 1:1)                  | **비연결형** 프로토콜 (서버, 클라이언트 소켓의 구분이 없다)  |
| **가상 회선 방식** (발신지와 수신지를 연결해 패킷을 전송한다.) | **데이터그램 방식** (패킷을 서로 다른 경로로 전송한다.)      |
| 3-way-handshaking으로 연결하고, 4-way-handshaking으로 해제한다. | 연결을 설정/해제하는 과정이 없음. 각 패킷은 독립적으로 처리된다. |
| 연결형 서비스 -> 높은 신뢰성 보장(전송 순서 보장)            | **신뢰성 보장 X** (전송 순서 바뀔 수 있음)                   |
| 데이터의 흐름/혼잡 제어도 한다 -> **속도가 느리다.**         | 흐름/혼잡 제어 없다 -> **속도가 빠르고** 네트워크 부하가 적다. |
| TCP 패킷 : segment                                           | UDP 패킷 : datagram                                          |
| *HTTP, 메일, 파일 전송*같은 신뢰성이 보장되어야 하는 서비스에 주로 사용된다. | *실시간 스트리밍*, *DNS* 등 신뢰성보다는 연속성이 중요한 서비스에 주로 사용된다. |

<br/>

---

### 🤓 소켓

무조건 연결 기반.

TCP는 서버의 소켓 기반으로 통신하고,

UDP는 소켓 대신 IP 기반으로 데이터를 전송한다.

<br/>

