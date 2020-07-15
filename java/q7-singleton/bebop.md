# Singleton 패턴에 대해 알아보자(with exercise)

JVM에 클래스의 인스턴스 객체를 하나만 만들어 두고 계속해서 재사용하는 디자인 패턴이다.



성능이 영 좋지 않은 싱글톤 패턴 구현 방법 1

```java
class NotGoodPattern {
  private static Object instance;
  
  public static synchronized Object getInstance(){
    if(instance == null) {
      instance = new Object();
    }
    return instance;
  }
}
```

`synchronized` 를 붙히고 지연 초기화(사용 할 때 객체를 생성하는)를 하는 간단한 싱글톤 객체를 구현했다.  
근데 성능이 좋지 못하다.

위 코드에서 병목이 생기는 부분은 `getInstance()` 자체에 접근하는 것이 동기화가 걸리기 때문이다.  
조금 더 세밀하게 동기화 블럭을 잡아줘서 성능을 개선 시키면 다음과 같다.

```java
class StillNotGoodPattern {
  private static final Object lock = new Object();
  private static volatile Object instance;
  
  public static Object getInstance(){
    if(instance == null) {
    	synchronized(lock) {
	    	if(instance == null) {
      		instance = new Object();
	    	}      
  	  }
    }
    return instance;
  }
}
```

`volatile` 을 이용해 캐시된 결과가 아닌 메인 메모리에서 가장 최신의 `instance` 상태를 읽어오고 비공개 락 객체인 `lock`을 이용해서 동기화 블럭(지연 초기화 부분에 대해)을 조금 더 세밀하게 잡아줌으로써 성능 병목을 개선하였다.  
그래도 여전히 영 좋지 못한 성능이다.  
`synchronized` 는 성능상 좋지 못하고 `volatile` 로 인해 메인 메모리에 접근하는 것도 성능상 좋지못하다.

계속해서 지연 초기화를 하려고 하니 `synchronized` 와 `volatile` 을 사용했고, 이로인해 성능상 이슈가 발생했다.

어지간한 상황에서는 지연 초기화를 안하는 것이 더 낫다.

```java
class GoodPattern {
  private static final Object instance = new Object();
  
  public static Object getInstance() {
    return instance;
  }
}
```

static final 필드로 정의된 instance는 JVM이 클래스 로딩을 할때 바로 메모리(메소드 영역)에 적재된다.  
즉, 지연 초기화를 하지 않기 때문에 동기화에 대해 생각할 필요가 없다.

일반적인 싱글톤 객체를 구현하는 방식이다.  

그렇다면 더! 좋은 방법은 무엇일까?

enum을 사용하는 것이다.

```java
enum Singletones {
  ONE,
  TWO,
  THREE
}
```

자바 1.5버전 부터 enum이 생겼다.

개발자가 직접 싱글톤을 구현하는 방법 중에 가장 확실한 방법중 하나이다.  
위 static final 필드로 구현한 싱글톤도 좋긴 하지만 개발자가 실수로 final을 안붙히는 실수를 한다면 싱글톤이 깨질 우려가 있다.  
하지만 enum은 자바에서 싱글톤을 보장해준다.

