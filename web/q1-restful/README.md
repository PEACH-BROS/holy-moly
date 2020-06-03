# RESTful 에 대해 설명해주세요.

[엘리의 RESTful](elly-restful.md)

[비밥의 RESTful](bebop.md)

[코일의 RESTful](coyle-restful.md)



## REST?

일반적으로 HTTP를 사용해서 필요한 자원에 접근하는 방식이다. 자원은 DB에 저장된 데이터는 물론 이미지, 문서, 파일 등을 모두 포함한다.



## HTTP와 RESTful의 차이점은 ?

REST는 어떤 프로토콜과도 독립적이다. HTTP에 연결될 필요가 없다. 하지만 대부분의 일반적인 REST 구현에서 애플리케이션 프로토콜로 HTTP를 사용한다.

REST는 규칙들의 집합일 뿐이다.



```
GET : 자원에 대한 조회를 요청한다.

POST : 자원에 대한 생성을 요청한다.

PUT : 자원에 대한 모든 정보를 가지고 수정을 요청하나 수정 대상이 없으면 생성을 할 수 있음을 의미한다.

PATCH : 자원에 대한 일부 정보를 가지고 수정을 요청한다.

DELETE : 자원의 제거를 요청한다.

OPTIONS : 해당 자원에 대해 가능한 method 정보를 요청한다.
```

PUT은 uri에 명시된 자원을 업데이트할 때 주로 쓰인다.

PATCH 또한 업데이트 시에 쓰이는데, PUT이 전체 수정에 유리하다면, PATCH는 부분 수정에 유리하다. 자원에서 변경이 필요한 부분만 보내면 해당 자원을 찾아 업데이트를 해주지만, PUT 요청으로 변경할 부분만 보낸다면 해당 자원을 찾지 못하고 새로운 자원을 생성한다.





## RESTful하지 못한 예?

- CRUD 기능을 모두 POST로만 처리하는 api

- uri에 자원과 id 외의 정보(ex. 행위)가 들어가는 경우. student 하나의 이름을 변경하는 api의 경우 `PUT /students/updateName`이 아니라, `PUT /students/:id/name`이 되어야 한다.

- REST를 사용하면서 제일 지키지 못하는 부분이 **Uniform Interface** :

  - HATEOAS: 애플리케이션의 상태는 HyperLink를 이용해 전이되어야한다.

    - Link Header를 이용해서 현재 메세지와 연결되어 있는 다른 자원을 가르켜 주어야한다.

  -  Self Descriptive Message: 메세지는 스스로를 표현할 수 있어야한다.

    - 메세지의 URI는 있지만 HOST(실제 자원을 가지고 있는 곳)가 빠져있는 경우,
    - 요청, 응답 메세지의 body의 content-type이 없는 경우, Self Descriptive하지 못하다.  

      

      

### 🤔 상태 코드가 전부 200 OK인 경우도 restful하지 못할까?

상태 코드는 meta data라고 한다. 이걸 이용해 응답의 성공 여부를 판단하는데, 이 상태 코드가 아닌 response body에 응답 코드를 노출시켜서 어플리케이션의 흐름을 제어하는 건 RESTful하지 못하다.

**= 상태 코드가 무의미해지면 restful하지 못하다.**









> 참고문서

https://www.youtube.com/watch?v=RP_f5dMoHFc
https://meetup.toast.com/posts/92
https://docs.microsoft.com/ko-kr/azure/architecture/best-practices/api-design  
https://spoqa.github.io/2012/02/27/rest-introduction.html  
https://sanghaklee.tistory.com/61  
https://sanghaklee.tistory.com/57