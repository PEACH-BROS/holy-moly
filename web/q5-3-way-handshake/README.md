# 3-way-handshake에 대해 아는 것을 다 말하라.

[엘리의 3-way-handshake](elly-3-way-handshake.md)

[비밥의 3-way-handshake](bebop.md)

[코일의 3-way-handshake](super-coyle.md)

[스티치의 3-way-handshake](stitch.md)



### 3-way-handshake : 연결 성립

TCP은 연결 지향적인 프로토콜이기 때문에 연결의 성립과정이 필요하다.

3-way-handshake 라는 말처럼 3번의 통신으로 연결의 성립을 하는 것을 의미한다.

1. 클라이언트가 서버에게 접속을 요청하는 패킷(`SYN(N)`) 패킷을 보낸다.
2. 서버는 클라이언트의 요청 패킷(`SYN(N)`)을 받고 요청을 수락한다는 패킷(`ACK(N+1)`, `SYN(M)`)
3. 클라이언트는 서버의 수락 패킷(`ACK(N+1)`, `SYN(M)`)을 받고 수락 패킷을 받았다는 응답(`ACK(M+1)`)을 서버에게 보냄으로써 연결을 성립한다.

![image](https://user-images.githubusercontent.com/13347548/88076871-9060bf00-cbb5-11ea-89d2-fcc528fc4715.png)

### 4-way-handshake : 연결 해제

3-way-handshake가 3번의 통신으로 연결을 성립했다면  
4-way-handshake는 4번의 통신으로 연결을 해제한다.

1. **클라이언트가 연결 종료 플래그(`FIN`)를 전송**한다.
2. **서버는** 클라이언트의 종료 플래그(FIN)를 받고 **확인 메세지(`ACK`)를 전송**한다.
   1. 데이터를 모두 전송 할 때까지 잠시 `time out`이 된다.
3. 통신이 끝나면 **서버는 연결 종료 플래그(`FIN`)를 클라이언트에게 전송**한다.
4. **클라이언트는** 연결 종료 플래그(`FIN`)를 **확인했다는 메시지(`ACK`)를 서버에게 전송**한다.
5. 클라이언트의 확인 메세지를 받은 서버는 연결되어 있던 소켓을 close한다.
6. 클라이언트는 아직 서버로부터 받지 못한 데이터를 대비해 일정 시간 세션을 살려두고 잉여 패킷을 기다린다(`time wait`).

![image](https://user-images.githubusercontent.com/13347548/88078745-c69f3e00-cbb7-11ea-9f3e-caf33a096573.png)
