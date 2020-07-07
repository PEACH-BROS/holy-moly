# 주소창에 https://www.google.com을 입력했을 때

[스티치의 웹 요청-응답](stitch.md)

[엘리의 웹 요청-응답](elly-request-response.md)

[비밥의 웹 요청-응답](bebop.md)

[코일의 웹 요청-응답](coyle.md)



---



### 웹 요청 - TCP/IP 계층 별 동작 과정



**애플리케이션 계층**에서 **DNS를 통해 IP 주소를 알아낸다.**

www.google.com 은 IP 주소를 사람이 이해하기 쉬운 도메인 네임으로 바꾼 형태이다.

따라서 www.google.com이 **실제 어떠한 IP를 가리키는지 DNS를 통해서 알아내야한다.**

하지만 매번 DNS 서버를 검색하는 것은 비효율적이기 때문에 **캐시되어 있는 정보(브라우저 캐시)를 먼저 확인**한다.  
만약 브라우저 캐시에 정보가 없다면 **hosts 파일을 참조**한다.

**트랜스포트 계층**에서 **IP주소를 얻어 HTTP 요청을 보내기 위한 준비 작업을 한다.**

요청은 TCP 세그맨트로 제작된다. 목적지 포트와 출발지 포트정보 등이 추가되어 제작된다.

그리고 네트워크 계층으로 전송한다.

이러한 전송 과정은 TCP HandShake 혹은 TLS HandShake를 이용한다.

**네트워크 계층**에서**HTTP 메세지를 TCP 세그먼트 패킷으로 분리하여 IP그램으로 캡슐화해 전송한다.**

네트워크 계층에서는 **IP 헤더 정보**를 전달받은 TCP 세그먼트 패킷에 추가하여 IP그램으로 캡슐화 한다.

그리고 데이터링크 계층으로 전송한다.

**데이터링크 계층**에서 **ARP를 이용하여 IP에 해당하는 MAC주소를 찾아 전송한다.**

이때 라우팅 테이블을 참고하여 목적지 웹 서버가 있는 라우터에 패킷을 전달한다.

패킷을 전달 받은 웹 서버는 그에 해당하는 응답을 역 캡슐화 하면서 요청을 한 클라이언트에게 다시 되돌려준다.

# 웹 요청

1. PC를 사용하기 위해 물리적인 랜선을 연결한다.

2. PC는 자신의 IP를 받기 위해 DHCP 서버를 찾는다.

   한 서브넷에 속하는 모든 네트워크 구성원들에게 DHCP 서버를 찾는다는 **DHCP Discover 메시지**를 보낸다.

3. DHCP 서버는 클라이언트에게 자신이 DHCP 서버라는 것을 알려주고, **IP를 할당해준다.** (가장 가까운 라우터의 IP, 가장 가까운 DNS 서버 IP 등을 함께 알려준다)

4. IP 할당 성공! 구글 도메인(www.google.com)에 접속하기 위해 브라우저를 켜고 URL을 입력한다.

5. 도메인은 IP가 아닌 www.google.com이라는 도메인 이름으로 되어 있어, 도메인에 대한 IP가 필요하다. 가장 먼저 도메인 이름에 해당하는 IP를 찾기 위해 **브라우저 캐시**(`chrome://net-internals/#dns`)를 참조한다.

6. 브라우저 캐시에 없으면 **hosts 파일**(운영체제가 호스트 이름을 IP주소에 맵핑할 때 사용하는 컴퓨터 파일. 플레인 텍스트 파일이다.)을 참조한다.

7. hosts 파일에도 없으면 DNS 서버를 조회한다. `Local DNS 서버`에 www.google.com를 요청한다.

   ![image](https://user-images.githubusercontent.com/19922698/86616813-5c03d500-bff1-11ea-9baa-9bc09c0973d4.png)

8. `Local DNS 서버`에 www.google.com에 대한 정보가 없을 시, `Root DNS 서버`에 요청한다.

9. `Root DNS 서버`에 www.google.com에 대한 정보가 없을 시, `Local DNS 서버`에게 com DNS 서버 정보를 준다.

10. 정보를 받은 `Local DNS 서버`는 `com DNS 서버`에게 www.google.com을 요청한다.

11. `com DNS 서버`에 www.google.com에 대한 정보가 없을 시, `Local DNS 서버`에게 `google.com DNS` 정보를 제공한다.

12. 정보를 받은 `Local DNS 서버`는 `google.com DNS` 에게  www.google.com을 요청한다.

13. `google.com DNS`는 `Local DNS 서버`에게 www.google.com에 대한 IP를 제공한다.

14. `Local DNS 서버`는 클라이언트에게 www.google.com 도메인에 대한 IP를 제공한다.

15. 정보를 받은 클라이언트는 www.google.com 사이트에 접속을 요청하는 **HTTP REQUEST 메시지**를 보낸다.

16. 구글 홈페이지는 HTTP(80)가 아닌 HTTPS(443)을 이용한다. 따라서 TLS(SSL) Handshaking 과정을 거친다.

![image](https://user-images.githubusercontent.com/19922698/86617880-e993f480-bff2-11ea-9313-03995a641095.png)

17. 이후 클라이언트가 요청한 NAVER의 첫 페이지 데이터를 **HTTP RESPONSE**로 응답한다.
18. 최종적으로, www.google.com 홈페이지가 사용자의 브라우저에 보이게 된다





## 스티치의 입장

주소창에 `https://www.google.com` 을 입력하면 DNS 서버로 가서 해당 주소에 맞는 서버 ip를 받아온다.

그러면 해당하는 ip로 페이지 요청이 날라간다. 요청에는 응답을 받을 사용자의 ip주소가 담겨있어서 서버에서는 페이지 정보를 다시 해당 사용자 ip로 날려준다.