# 3-way Handshake에 대하여

## TCP 3-way Handshake

TCP/IP 프로토콜을 이용해서 통신을 하는 응용프로그램이 데이터를 전송하기 전에 먼저 정확한 전송을 보장하기 위해 상대방 컴퓨터와 사전에 세션을 수립하는 과정을 의미한다.

3-way Handshake를 통해 양쪽 모두 데이터를 전송할 준비가 되었다는 것을 보장하고, 실제로 데이터 전달이 시작하기 전에 한 쪽이 다른 쪽의 준비 상태를 알 수 있도록 한다.

양쪽 모두 상대편에 대한 초기 순차 일련번호를 얻을 수 있도록 한다.

![3-way Handshake](https://t1.daumcdn.net/cfile/tistory/225A964D52F1BB6917)

### 과정

#### Step 1

클라이언트는 서버에 접속을 요청하는 SYN 패킷을 보낸다. 이때 클라이언트는 SYN를 보내고 SYN/ACK 응답을 기다리는 SYN_SENT 상태가 된다.

#### Step 2

 서버는 SYN 요청을 받고 클라이언트에게 요청을 수락한다는 ACK와 SYN flag가 설정된 패킷을 발송하고 클라이언트가 다시 ACK로 응답하기를 기다린다. 이때 서버는 SYN_RECEIVED 상태가 된다.

#### Step 3

클라이언트는 서버에게 ACK를 보내고 이후부터는 연결이 이루어지고 데이터가 오가게 된다. 이때의 서버 상태는 ESTABLISHED이다.

## TCP 4-way Handshake

3-way Handshake가 TCP의 연결을 초기화 할 때 사용한다면, 4-way Handshake는 세션을 종료하기 위해 수행되는 절차이다.

![4-way Handshake](https://t1.daumcdn.net/cfile/tistory/2152353F52F1C02835)

### 과정

#### Step 1

클라이언트가 연결을 종료하겠다는 FIN 플래그를 전송한다.

#### Step 2

서버는 일단 확인 메시지를 보내고 자신의 통신이 끝날 때까지 기다리는데 이 상태가 TIME_WAIT 상태이다.

#### Step 3

서버가 통신이 끝났으면 연결이 종료되었다고 클라이언트에게 FIN 플래그를 전송한다.

#### Step 4

클라이언트는 확인했다는 메시지를 보낸다.

