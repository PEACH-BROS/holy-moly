## IoC란?

Inversion of Control의 약자. **의존성을 밖에서 주입받는다.**



일반적으로 내가 필요한 의존성은 내가 만든다. (제어권을 내가 갖고 있다)

```java
public class StationService {
  private StationRepository stationRepository = new StationRepository();
}
```

하지만 이를 밖에서 주입받는다. (제어권이 넘어갔다. = 제어권이 역전되었다.)

```java
public class StationService {
	private StationRepository stationRepository;
	
	public StationService(StationRepository stationRepository) {
		this.stationRepository = stationRepository;
	}
	
	//stationRepository를 사용한다.
}
```



✅ **DI (Dependency Injection)도 일종의 IoC이다.**

```
As a result I think we need a more specific name for this pattern. Inversion of Control is too generic a term, and thus people find it confusing. As a result with a lot of discussion with various IoC advocates we settled on the name Dependency Injection.
```

IoC 개념은 너무 일반적인 용어라 사람들을 헷갈리게 한다. 이에 우리는 DI를 정의하게 되었다.



마틴 파울러가 제시한 3가지 의존성 주입 패턴

- 생성자 주입

- setter 주입

- 인터페이스를 통한 주입: 의존성을 주입하는 함수를 포함한 인터페이스를 작성하고, 이 인터페이스를 구현하도록 함으로써 실행 시 이를 통해 의존성을 주입한다.

  ```java
  public interface StationDI {
    	void addDependency(StationRepository stationRepository);
  }
  
  public class StationService implements StationDI {
    	private StationRepository stationRepository;
    
    	@Override
    	public void addDependency(StationRepository stationRepository) {
          this.stationRepository = stationRepository;
      }
  }
  ```

  



### IoC의 장점

1. 필드 값이 null이 될 수 없다. (최소한 NPE를 막아준다.)
2. 테스트하기 용이하다.





## IoC 컨테이너

빈의 생명주기를 관리하고, 빈들 사이의 의존성을 엮어준다.

`@Component`, `@Bean`이 붙은 애들을 빈으로 띄운다.





> 참고
>
> [마틴 파울러의 IoC와 DI](https://black-jin0427.tistory.com/194)
> 백기선의 예제로 배우는 스프링 프레임워크 강의