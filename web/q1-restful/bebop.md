# 비밥, RESTful 에 대해 설명해주세요.

분산 하이퍼 미디어 시스템(웹과 같은)을 위한 아키텍쳐 스타일이다.

쉽게 말하면 URI 와 Http Method의 조합으로 자원의 위치와 제어를 표현하는 방법이다.  
URI에 표현되는 자원은 계층을 나타내도록 나열을 한다.  
계층을 표현할때는 자원 사이에 `/` 을 사용한다.

자원에 대한 요청을 나타내는 방법은 Http Method를 이용한다.

```
GET : 자원에 대한 조회를 요청한다.

POST : 자원에 대한 생성을 요청한다.

PUT : 자원에 대한 모든 정보를 가지고 수정을 요청하나 수정 대상이 없으면 생성을 할 수 있음을 의미한다.

PATCH : 자원에 대한 일부 정보를 가지고 수정을 요청한다.

DELETE : 자원의 제거를 요청한다.

OPTIONS : 해당 자원에 대해 가능한 method 정보를 요청한다.
```

## 매운맛 RESTful !

정석대로 빡빡하게 RESTful 을 만족하려면 여러가지 조건을 갖추어야 한다.

---

### Client-Server

자원의 제공은 Server가 Client는 사용자 인증이나 컨텍스트를 관리하는 구조로  
역할이 분리되어 의존성이 적은 구조를 의미한다.

### Stateless

상태정보가 없기 때문에 똑같은 요청이 들어와도 독립적인 요청으로 인식한다.

### Cache

HTTP가 가진 캐시 기능을 사용할 수 있어야한다.

### Uniform Interface

- 자원이 URI로 식별되어야 한다.
- 자원을 제어하는 행동(생성, 수정, 삭제)은 HTTP Method를 이용해서 달성해야 한다.
- 메세지는 스스로를 표현할 수 있어야한다.
  - 메세지의 URI는 있지만 HOST가 빠져있는 경우 X
  - 요청, 응답 메세지의 body의 content-type이 없는 경우 X
- 애플리케이션의 상태는 HyperLink를 이용해 전이되어야한다.
  - Link Header를 이용해서 현재 메세지와 연결되어 있는 다른 자원을 가르켜 주어야한다.

### Layerd System

REST 서버는 계층 구조로 구성될수 있음을 의미한다.

이러한 구조에 Proxy, Load Balacing, GateWay 등이 들어갈 수 있다.

### Code-on-demand (Optional)

서버에서 코드를 클라이언트에게 보내서 실행시킬수 있어야한다. (ex.자바스크립트)

---

REST가 Uniform Interface를 필요로 하는 이유는 Server와 Client의 독립적인 진화를 위해서이다.

---

> 참고  
>
> https://www.youtube.com/watch?v=RP_f5dMoHFc  
> https://meetup.toast.com/posts/92

