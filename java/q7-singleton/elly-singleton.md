# 싱글톤이 뭐죠? 만들어보세요

프로그램 전체에서 단 하나만 존재하는 객체를 의미한다.



```java
public class Car {

    private String name;

    private Car(String name) {
        this.name = name;
    }

    private static Car CAR = new Car("자동차1");

    public static Car getInstance() {
        return CAR;
    }
}

```

이런 식으로 만들 수 있다.

하지만 위와 같은 예시는 메모리 누수(Memory Leak, 필요하지 않은 메모리를 계속 점유하고 있는 현상)를 일으킨다.



그래서 Lazy Initialization 방법이 있다.

```java
public class Car {

    private String name;

    private Car(String name) {
        this.name = name;
    }

    private static Car CAR;

    public static Car getInstance() {
      	if (CAR == null) {
          	CAR = new Car("자동차1");
        }
        return CAR;
    }
}
```

얘도 별로 좋지 않다.

Reflection을 통해 얼마든지 새 객체를 만들 수 있다.

(첫 번째 참고자료 참고)



---

### Thread-safe한 싱글톤



1. public static **synchronized** Car getInstance() {...}

   : 별로다. 여러 스레드에서 동시에 접근할 때 불필요하게 기다려야 한다. 왜냐하면 이미 CAR의 값이 초기화된 경우, 불필요하게 기다려야 하기 때문이다.

2. private static **volatile** Car CAR;

   : 다른 쓰레드에서 작업한 캐싱된 값이 아닌, 메모리에서 가장 최신의 값을 가져올 수 있다.

---

### Enum으로 된 싱글톤

```java
public enum SingletonEnum {
		CAR
		
		SingletonEnum() {
		}
}
```

요 놈이 짱이다. 자바에서 싱글톤을 보장해준다.







> 참고
>
> https://medium.com/@kevalpatel2106/how-to-make-the-perfect-singleton-de6b951dfdb0