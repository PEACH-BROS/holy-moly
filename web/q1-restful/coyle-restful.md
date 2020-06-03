## RESTful이 뭔지 설명해주세요
  RESTful은 일종의 규약으로 자원에 대한 사용을 명시적으로 드러내는 url 설계법입니다.
 자원(URI), 행위(HTTP Method), 표현의 구성을 가지고 있습니다. 즉, URI로 어떤 자원에 접근할 것인지를 명시적으로 드러내고, HTTP Method로 어떤 행위를 할 것인지를 표현합니다.
 예를 들자면, `GET stations/1` 의 경우 stations의 1이라는 컬럼을 가진 데이터를 가져오라는 표현이 됩니다.
 특징으로는 URI로 지정한 리소스에 대한 조작을 통일된 인터페이스로 수행합니다. 이로써 REST API 메시지만 보고도 이를 쉽게 이해할 수 있습니다. 
 또한, REST 서버는 다중 계층으로 구성될 수 있으며 보안, 로드밸런싱, 암호화 계층을 추가해 구조상의 유연성을 둘 수 있고 PROXY, 게이트 웨이 같은 네트워크 기반의 중간 매체를 사용할 수 있게 한다.
 ~~그외 무상태성, 캐시 가능, Client-Server 구조는 모두 Http의 특징 아닌가?~~
 
## HTTP와 RESTful의 차이점은 ?
  HTTP는 웹 환경에서 정보를 송수신할 때 사용하는 약속이고, REST는 소프트웨어 아키텍처입니다.
 REST에 반드시 HTTP가 필요한 것은 아닙니다.다른 프로토콜로도 이용 가능합니다.
 REST는 소프트웨어 아키텍처(설계 지침, 원리 등등)고 REST에서 클라이언트-서버 간 통신 시 HTTP를 사용한 것입니다.
 
## 어떤 메서드가 있나요 ?
  GET은 리소스 조회, POST는 엔티티 추가, PUT은 리소스 수정, DELETE는 리소스 삭제가 있습니다. 
 이 외에도 PATCH 라는 HTTP Method도 존재합니다. PUT이 해당 자원의 전체를 교체하는 의미를 지니는 대신, PATCH는 일부를 변경한다는 의미를 지니기 때문에 최근 update 이벤트에서 PUT보다 더 의미적으로 적합하다고 평가받고 있습니다.
 OPTIONS, HEAD의 METHOD들도 존재합니다.
 
### 출처
 - https://meetup.toast.com/posts/92
 - https://spoqa.github.io/2012/02/27/rest-introduction.html
 - https://sanghaklee.tistory.com/61
 - https://sanghaklee.tistory.com/57
