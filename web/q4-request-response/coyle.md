

![](https://t1.daumcdn.net/cfile/tistory/99F099375C124B2D02)

1. 사용자가 브라우저에 도메인 네임을 입력한다.
2. 사용자가 입력한 URL 주소 중 도메인 네임 부분을 DNS 서버에서 검색하고, DNS 서버에서 해당 도메인 네임에 해당하는 IP 주소를 찾아 사용자가 입력한 URL 정보와 함께 전달한다.
3. 페이지 URL 정보와 전달받은 IP 주소는 HTTP 프로토콜을 사용하여 HTTP 요청 메시지를 생성하고, 이렇게 생성된 HTTP 요청 메세지는 TCP 프로토콜을 사용하여 인터넷을 거쳐 해당 IP 주소의 컴퓨터로 전송도니다.
4. 이렇게 도착한 HTTP 요청 메시지는 HTTP 프로토콜을 사용하여 웹 페이지 URL 정보로 변환되어 웹 페이지 URL 정보에 해당하는 데이터를 검색한다.
5. 검색된 웹 페이지 데이터는 또 다시 HTTP 프로토콜을 사용하여 HTTP 응답 메시지를 생성하고 TCP 프로토콜을 사용하여 인터넷을 거쳐 원래 컴퓨터로 전송된다.
6. 도착한 HTTP 응답 메세지는 HTTP 프로토콜을 사용하여 웹 페이지 데이터로 변환되어 웹 브라우저에 출력되어 사용자가 볼 수 있게 된다.



## 세부 과정

### 1. 브라우저의 URL 파싱

> URL은 웹에서 정해진 자원의 주소. 각각의 유일한 URL은 유일한 자원을 가리킨다. 



- https일 경우 예전에 공부했듯 SSL 핸드쉐이크를 거쳐 신뢰할 수 있는 서버인지 확인한다.
- 도메인 이름, 자원 경로, 파라미터들을 파싱한다.



## 2. DNS 서버 검색

> 도메인 네임으로 목적지 IP 주소를 알아오는 과정

- 브라우저는 도메인이 캐시에 들어있는지 확인
  - 크롬에서 DNS 캐시를 보려면 chrome://net-internals/#dns 확인 가능

- 찾지 못하면 브라우저는 검색을 하기 위해 **gethostbyname** 라이브러리 함수를 호출
  - **gethostbyname** 은 DNS 통한 호스트명 확인을 시도하기 전 호스트명이 로컬의 hosts 파일에서 참조될 수 있는지 확인

- **gethostbyname** 이 캐시와 hosts 파일 모두에서 호스트명을 찾지 못하면 네트워크 스택에 정의된 DNS 서버에 요청
- 일반적으로 로컬 라우터나 인터넷 공급자의 캐시 DNS 서버로 보내집니다.



---

1. 로컬 PC 의 hosts 파일 확인
   - PC 자체의 DNS 역할
2. DHCP&ARP
   - DHCP : 동적 호스트 구성 프로토콜의 약자로 호스트의 IP 주소 및 TCP/IP 설정을 클라이언트에 자동으로 제공하는 프로토콜이다.
     - 사용자의 PC는 DCHP서버에서 사용자 자신의 IP 주소, 가장 가까운 라우터의 IP 주소, 가장 가까운 DNS 서버의 IP 주소를 받는다.
   - 이후, ARP 프로토콜을 이용하여 IP 주소를 기반으로 가장 가까운 라우터의 MAC 주소를 알아낸다.

3. IP 정보 수신

   - 2의 과정을 통해 외부와 통신할 준비를 마쳤으므로, DNS Query를 DNS 서버에 송신한다. DNS 서버는 이에 대한 결과로, 웹 서버의 IP 주소를 사용자 PC에 돌려준다. 

   - 사용자 PC는 가장 먼저 지정된 DNS 서버에 DNS Query를 송신한다.
   - 그 후 지정된 DNS 서버는 Root 네임 서버에 www.google.com을 질의하고, Root 네임 서버는 .com 네임서버의 ip 주소를 알려준다.
   - .com 네임 서버에 www.google.com을 질의하면 google.com 네임서버의 ip 주소를 받고, 그곳에 질의를 또 송신하면 www.google.com의 IP 주소를 수신하게 된다.

   > 이와 같이 여러번 왔다갔다 하는 이유는, 도메인의 계층화 구조에 따라 DNS 서버도 계층화 되어있기 때문이다. 이렇게 계층화되어 있으므로 도메인의 가장 최상단, 즉 가장 뒷쪽(.com, .kr 등등)을 담당하는 DNS 서버는 전세계에 13개 뿐이다.

![](https://t1.daumcdn.net/cfile/tistory/267BCC405870914920)



4. 웹 서버 접속

- 이제 웹 서버의 IP 주소까지 알았다. HTTP Request를 위해, TCP socket을 개방하고 연결한다. 이 과정에서 3-Hand-Shaking이 일어난다. TCP 연결에 성공하면, Http Request가 TCP Socket을 통해 보내진다. 이에 대한 응답으로 웹 페이지의 정보가 사용자의 PC로 들어온다.

[https://wangin9.tistory.com/entry/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%97%90-URL-%EC%9E%85%EB%A0%A5-%ED%9B%84-%EC%9D%BC%EC%96%B4%EB%82%98%EB%8A%94-%EC%9D%BC%EB%93%A4-1URL%EC%9D%84-%ED%95%B4%EC%84%9D%ED%95%9C%EB%8B%A4](https://wangin9.tistory.com/entry/브라우저에-URL-입력-후-일어나는-일들-1URL을-해석한다)



- https://preamtree.tistory.com/35