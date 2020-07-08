# 스프링의 IoC에 대해 설명해보세요.

[엘리의 IoC](elly-ioc.md)

[비밥의 IoC](bebop.md)

[스티치의 IoC](stitch.md)

## IoC란

IoC는 **Inversion of Control**의 약자이며 단어의 뜻 그대로 **제어의 역전**을 말한다.  

일반적으로 객체의 생명주기는 프로그래머에 의해 결정되게 된다.  
대표적으로 객체의 생성은 `new` 키워드를 통해 이루어진다.  
하지만 IoC는 이러한 **객체의 생명주기를 프로그래머가 관리하는 것이 아닌 Spring이( *IoC 컨테이너가* ) 관리하게 하는 것**을 의미한다.  





✅ **DI (Dependency Injection)도 일종의 IoC이다.**

```
As a result I think we need a more specific name for this pattern. Inversion of Control is too generic a term, and thus people find it confusing. As a result with a lot of discussion with various IoC advocates we settled on the name Dependency Injection.
```

IoC 개념은 너무 일반적인 용어라 사람들을 헷갈리게 한다. 이에 우리는 DI를 정의하게 되었다. - 마틴 파울러의 글



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





## 스프링의 IoC를 사용하면

기존의 순서로는 

1. 객체 생성
2. 의존성 객체 생성(클래스 내부에서)
3. 의존성 객체 메서드 호출

이었다면, 스프링에서는

1. 객체 생성
2. 의존성 객체 주입(스스로가 만드는 것이 아니라 제어권을 스프링에게 위임하여 스프링이 만들어 놓은 객체를 주입)
3. 의존성 객체 메서드 호출





## IoC Container

Bean을 만들고 Bean들의 의존성을 엮어주고, 만들어진 Bean들을 제공한다.

ApplicationContext 혹은 BeanFactory를 사용한다.

- 알아서 의존성을 주입해주기 때문에 실제로 사용할 일이 없다.
- IoC Container를 사용하면 아무런 코드를 넣지 않아도 IoC Container에 등록된 Bean을 가져다 사용 가능하다.

의존성 주입은 IoC Container 안에 들어있는 Bean끼리만 가능하다.