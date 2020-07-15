# Singleton 패턴에 대해 알아보자(with exercise)

## Singleton 패턴이란

클래스가 최초 한 번만 메모리를 할당하고(static) 그 메모리에 인스턴스를 만들어 사용하는 디자인 패턴이다.

생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나고 최초 생성 이후에 호출된 생성자는 최초에 생성한 객체를 반환한다. 고정된 메모리 영역을 얻으면서 한 번만 인스턴스를 생성하기 때문에 메모리 낭비를 방지할 수 있다. 또한 전역 인스턴스이기 때문에 데이터 공유가 쉽다.

DBCP(DataBase Connection Pool)처럼 공통된 객체를 여러개 생성해서 사용해야 하는 상황에서 많이 사용한다.



### 일반적인 사용 방법

```java
public class Singleton {
    
    private static Singleton instance = new Singleton();
    
    private Singleton() {
    }
    
    public static Singleton getInstance() {
        if (instance == null) {
            instance = new Singleton();
        }
        return instance;
    }
    
}
```



### Singleton 패턴의 문제점

싱글톤 인스턴스가 너무 많은 일을 하거나 많은 데이터를 공유시킬 경우 다른 클래스의 인스턴스들 간에 결합도가 높아져 `개방 폐쇄 원칙` 을 위배하게 된다.

멀티 쓰레드 환경에서 동기화 처리를 안하면 인스턴스가 두 개 생성된다든지 값의 변경이 의도치 않은 방식으로 이루어진다든지 하는 문제가 발생한다.

위의 '일반적인 사용 방법' 예제에서 `getInstance()` 메서드가 멀티 쓰레드에서 동작하게 되면 두 개의 인스턴스가 생성될 수 있는 문제가 발생한다.



## 올바른 Singleton 패턴 사용 방법

### Lazy initialization

```java
public class LazyInitSingleton {
    
    private static LazyInitSingleton instance;
    
    private LazyInitSingleton() {
    }
    
    public static synchronized LazyInitSingleton getInstance() {
        if (instance == null) {
            instance = new LazyInitSingleton();
        }
        return instance;
    }
    
}
```

`synchronized` 키워드를 사용하여 thread-safe하게 만들었지만 `synchronized` 키워드 특성상 성능저하가 발생한다.



### Lazy initialization + Double-checking locking

```java
public class LazyInitWithDoubleLockingSingleton {
    
    private static LazyInitWithDoubleLockingSingleton instance;
    
    private LazyInitWithDoubleLockingSingleton() {
    }
    
    public static LazyInitWithDoubleLockingSingleton getInstance() {
        if (instance == null) {
            synchronized (LazyInitWithDoubleLockingSingleton.class) {
                if (instance == null) {
                    instance = new LazyInitWithDoubleLockingSingleton();
                }
            }
        }
        return instance;
    }
    
}
```

처음에는 synchronized 키워드를 사용하지 않고 존재 여부 체크 후 존재하지 않는다면 동기화를 시켜서 인스턴스를 생성하므로 thread-safe 하면서 성능저화도 보완하였다. 그러나 이 방법 또한 그리 완벽한 방법은 아니다.



### LazyHolder

```java
public class Singleton {
    
    private Singleton() {
    }
    
    private static class LazyHolder {
        public static final Singleton INSTANCE = new Singleton();
    }
    
    public static Singleton getInstance() {
        return LazyHolder.INSTANCE;
    }
}
```

JVM의 클래스 초기화 과정에서 보장되는 atomic 특성을 이용하여 싱글턴의 초기화 문제에 대한 책임을 JVM에 떠넘긴다. LazyHolder 안에 선언된 instance가 static이기 때문에 클래스 로딩 시점에서 한 번만 호출될 것이며 final을 사용해 다시 값이 할당되지 않도록 만든 방법이다.

**가장 많이 사용하고 일반적인 Singleton 클래스 사용 방법이다.**

이와 비슷한 맥락으로 Java의 Enum 클래스도 Singleton 패턴 중 하나이다(effective java에서 추천하는 방법이긴 하다). 그러나 Context 의존성이 있을 수 있는 환경의 경우(Android) Singleton의 초기화 과정에 Context라는 의존성이 끼어들 가능성이 있고 그때는 Enum 클래스를 활용하기 어렵다. 

LazyHolder 또는 Enum을 상황에 맞게 사용하는 것이 좋을 것이다.

