# Network Proxy

### **프록시?**

: 대리. 남을 대신해 일을 처리한다. (= 대신 처리하는 서버)

**클라이언트-서버 사이의 중계 서버**로, 통신을 대리 수행하는 서버.

![image](https://user-images.githubusercontent.com/19922698/90538634-c8a9ec00-e1b9-11ea-9b55-836998b0fcd8.png)

캐시, 보안, 트래픽 분산 등 여러 장점을 가진다.

종류에는 Forward Proxy, Reverse Proxy가 있다.

<br/>

---

### Forward Proxy

일반적으로 말하는 프록시 서버.

Client - Forward Proxy - Internet - Servers

1. **캐싱**

   클라이언트가 요청한 내용을 캐싱한다.

   실제 서버까지 가지 않아도 되기 때문에 불필요한 전송이 줄어든다.

   ![image](https://user-images.githubusercontent.com/19922698/90539261-9351ce00-e1ba-11ea-8e6c-34d47d077f41.png)

   <br/>

2. **익명성**

   클라이언트가 보낸 요청을 감춘다. (클라이언트를 보호)

   서버는 요청을 누가 보냈는지 모른다. 서버가 받은 요청 IP는 Proxy의 IP이기 때문이다.

   ![image](https://user-images.githubusercontent.com/19922698/90539161-73baa580-e1ba-11ea-8c29-51c25d3e8c57.png)

<br/>

---

### Reverse Proxy

Client - Internet - Reverse Proxy - Servers

1. **캐싱**

   Forward Proxy와 동일.

   <br/>

2. **보안**

   서버 정보를 클라이언트로부터 숨김 (서버를 보호)

   클라이언트는 실제 서버를 Reverse Proxy로 착각한다.

   ![image](https://user-images.githubusercontent.com/19922698/90539632-0bb88f00-e1bb-11ea-9131-dce57bc6ef8a.png)

<br/>

3. **Load Balancer**

   : 부하 분산.

   요청이 많을 경우, 하나의 서버로 처리하기 힘들다.

   그럴 경우, 앞에 Load Balancer를 둬서 들어온 요청을 여러 개의 서버에 나눠준다.

   - L4 로드밸런서

     Transport Layer(4계층) (TCP/UDP)에서 로드 밸런싱을 한다.

     `https://www.naver.com` 으로 접근 시 서버 A, 서버 B에게 요청을 나눠줌.

   - L7 로드밸런서

     Applicatin Layer(7계층) (HTTPS/HTTP/FTP)에서 로드 밸런싱을 한다.

     `https://www.naver.com/search` , `https://www.naver.com/mypage` 로 접근 시 담당 서버 A, 서버 B에게 요청을 나눠줌.

   

<br/>

> 참고
>
> https://www.youtube.com/watch?v=YxwYhenZ3BE&list=PLgXGHBqgT2TvpJ_p9L_yZKPifgdBOzdVH&index=4  
> 