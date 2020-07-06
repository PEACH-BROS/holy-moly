# 웹 요청-응답 과정

🚀 나의 답변

*나중에 적을게요~ 감사합니다~*



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
18. 최종적으로, www.google.com 홈페이지가 사용자의 브라우저에 보이게 된다.





<br>

전체 흐름 정리

![image](https://user-images.githubusercontent.com/19922698/86619505-a71fe700-bff5-11ea-85c5-df22a5f600bd.png)





---

> **DHCP 서버**

호스트에게 IP주소를 할당해준다. 네트워크에 사용되는 IP 주소를 DHCP 서버가 중앙집중식으로 관리하는 클라이언트-서버 모델을 사용한다.

DHCP 지원 클라이언트는 네트워크 부팅 과정에서 DHCP 서버에 IP주소를 요청하고 이를 얻을 수 있다.

![[image](http://jun.hansung.ac.kr/SWP/intro.html)](https://user-images.githubusercontent.com/19922698/86614732-696b9000-bfee-11ea-8e21-93f2f25bbd5f.png)



> **계층별 프로토콜**

![image](https://user-images.githubusercontent.com/19922698/86619748-1bf32100-bff6-11ea-8d9b-a82c9e1bef73.png)





---

> 참고
>
> [DHCP 서버란](https://jwprogramming.tistory.com/35)  
> [브라우저 URL 창에 "naver.com"이라고 입력했을 시 최초 통신 과정](http://blog.naver.com/PostView.nhn?blogId=tkdldjs35&logNo=221977544533&categoryNo=421&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=search)  
> [웹의 동작 원리-전체 그림](http://tcpschool.com/webbasic/works)  
> [7계층 vs 4계층 그림](http://jun.hansung.ac.kr/SWP/intro.html)

