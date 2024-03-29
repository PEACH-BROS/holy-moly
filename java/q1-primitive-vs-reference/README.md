# 원시타입 VS 참조타입

[코일의 원시타입 VS 참조타입](./coyle.md)

[비밥의 원시타입 VS 참조타입](./bebop.md)

[엘리의 원시타입 VS 참조타입](./elly.md)

---

## 요약

### 원시타입과 참조타입의 저장장소

기본형 데이터 타입은 스택메모리에 저장됩니다.   
참조형 데이터 타입은 힙 메모리에 저장됩니다.

### 참조형 데이터 타입의 사용 유의점

참조타입에서 얕은 복사는 객체의 주소값을 복사하는 것이고 객체에 영향을 줄 수 있습니다. 깊은 복사는 원본으로부터 value 자체만 복사합니다. 예를 들어 `objectA = objectB` 는 얉은 복사로 주소값을 복사하며 객체에 영향을 줄 수 있는 것입니다. 반면에 `objectB = objectA.clone()`은 참조가 끊긴 객체의 값만을 복사합니다.

메소드의 return type이 참조 타입일 경우, 원시 타입과 달리 참조하고 있는 주소가 리턴된다. (원시타입은 값을 리턴)

**즉, 메소드 내에서 선언된 참조 타입 변수는 해당 변수가 리턴되거나, 다른 변수에 할당되는 경우엔 메소드가 끝나도 참조타입의 변수가 살아있을 수 있다. (힙 영역에 저장되기 때문!)**

### 웹 요청에서 원시타입과 참조타입

**원시타입은 기본 값을 가진다**. 반면 **참조타입은 기본 값을 가지지 않고 null** 이 할당된다는 것이다.

이러한 차이점으로 인해 원시타입과 참조타입을 사용할 때 주의해야할 점이 있다.

Java를 이용한 웹 서비스를 운영할 때 Request, Response용 객체를 다룬다면  
**멤버변수는 참조타입을 쓰는 것이 안전하다**고 생각한다.

클라이언트에게 Request 객체를 받을 때 해당 객체에 인스턴스 변수가 원시타입이라면 어떠한 문제가 발생할 수 있을까?

간단한 예로 회원의 수정을 요청하는 Update용 객체가 있다고 가정해보자

```java
public class MemberUpdateRequest{
  private int memberId;
  private String name;
  ...
}
```

위와 같이 `memberId` 가 `Integer`와 같은 참조타입이 아닌 원시타입인 `int`로 되어있다면  
클라이언트에게서 `memberId` 가 누락된 채로 요청을 들어온다면 **기본값이 0인 채로 요청**을 받게 될 수 있다.  
즉, **수정을 요청한 사람이 아닌 `memberId` 가 0인 사람의 정보가 바뀌는 대박 장애가 발생**할 수 있다.

##### DB에 memeberId를 auto increment 로 설정하면 0이아닌 1부터 들어가지 않나?

DB에 `memberId`가 auto increment 로 설정이 되어 있다고 해서 `memberId`가 0인 회원을 insert할 수 없는것은 아니다.  
또한 DB의 auto increment가 1부터 시작한다는 것과 `memberId`가 0인 요청이 발생할 수 있다는 것은 전혀 다른 문제이기도 하다.