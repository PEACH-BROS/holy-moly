# 주소창에 https://www.google.com을 입력했을 때

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



> 참고

http://blog.naver.com/PostView.nhn?blogId=tkdldjs35&logNo=221977544533&categoryNo=421&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=search

https://github.com/SantonyChoi/what-happens-when-KR

[책 : 그림으로 배우는 HTTP & Network](http://www.yes24.com/Product/Goods/15894097)

