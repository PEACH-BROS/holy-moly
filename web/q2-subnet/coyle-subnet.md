### 서브넷에 대해 설명하세요

특정 지역에서 관리되는 IP 영역을 몇개의 영역으로 나누어서 관리하는 것이다.



### 왜 Subnetwork하는가 ?

목적에 따라 호스트 영역을 관리하는 것이 편하기 때문이다. 예를들면 하나의 호스트가 지나치게 많은 트래픽을 차지해서 다른 같은 영역의 호스트까지 영향을 받는 경우, 동일한 클래스 내에 있는 호스트들이라 할지라도 관리상의 목적으로 서로 나누어서 관리하게되면 관리가 편하다.



그 외에도 LAN으로 구성할 때 호스트들이 꽤 넓은 범위에 흩어져 있을 수 있다.이때 LAN으로 확장할 수 있는 길이에는 한계가 있으므로 지역별로 네트웍을 나르게 구성해야한다. 이럴 때 subnetworking으로 가까운 지역의 호스트끼리 서로 묶어서 관리할 수 있다.
https://github.com/PEACH-BROS/holy-moly


네트웍 트래픽이 너무 높아 통신 속도의 저하를 가져올 때 여러개의 서브네트웍으로 분리함으로써 통신 속도를 높일 수 있다.



보안(:12)이 필요한 호스트와 그렇지 않은 호스트 그룹으로 나누고자 할때, 예를 들어 인터넷 서비스를 해야하는 호스트들은 외부로 노출되어도 되지만 R&D 같은경우에는 중요한 기술적인 내용들을 보호해야함으로 별도로 관리되어야 할것이다.



### 서브넷 네트워크 계산

IP주소와 서브넷 마스크를 AND연산하면 서브넷 네트워크를 구할 수 있다.



### 네트워크 영역, 호스트 영역 범위는?

#### **201.222.5.0 IP를 255.255.255.248 서브넷마스크**

11001001 11011110 00000101 00000000 = 201.222.5.0 (IP 주소)

11111111 11111111 11111111 11111000 = 255.255.255.248 (서브넷마스크)

11111111 11111111 11111111 (기본 C클래스가 가질 수 있는 네트워크 영역)

11111 (사용자 지정한 네트워크영역) => 32개의 서브넷 네트워크로 쪼갤 수 있다.

000 (사용자가 지정한 호스트영역) => 8개중 네트워크 주소와 브로드 캐스트 주소를 써야하므로 호스트 IP 개수는 6개이다.

- https://www.joinc.co.kr/w/Site/Network_Programing/Documents/SubNetWorking
- https://limkydev.tistory.com/166