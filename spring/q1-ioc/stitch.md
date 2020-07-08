# Spring IoC란

## IoC : Inversion of Control

일반적인 의존성에 대한 제어권은 사용하는 객체 스스로가 만들고 결정한다.

```
class OwnerController {
    
    private OwnerRepository repository = new OwnerRepository();

}
```

의존성 역전은 이렇게 객체 자신이 가지게 될 의존성을 스스로 결정하지 않고 외부로 넘겨주는 것이다.

즉 Inversion, 의존성의 제어권이 자신이 아니라 외부로 역전되었기 때문에 의존성 역전이라고 한다.

```
class OwnerController {
    
    private final OnwerRepository repository;
    
    public OwnerController(final OwnerRepository repository) {
        this.repository = repository;
    }
}
```

위의 예시는 `OwnerController` 가 가지는 의존성을 외부에서 주입받는 상황이다.

이렇게 의존성을 외부에서 주입받는 것을 Dependency Injection, 의존성 주입이라고 하는데 이 또한 의존성 역전 중 하나이다.

의존성을 주입받으면 자신이 스스로 의존성을 결정할 수 없고 외부에서 주입을 받아서 결정이 된다. 그렇기 때문에 의존성 주입은 IoC가 되는 것이다.

## 그렇다면 의존성은 어떻게 관리될까?

`OwnerController` 의 경우 생성이 될 때 필수적으로 `OwnerRepository` 를 필요로 한다.

왜냐하면 `OwnerController` 는 생성자가 `OwnerRepository` 를 매개변수로 받는 생성자 하나만 존재하기 때문이다.

따라서 `OwnerController` 가 가지는 의존성인 `OwnerRepository` 를 무조건 주입 받아야만 `OwnerController` 가 생성될 수 있고, 그렇기 때문에 `OwnerRepository` 는 null이 될 수가 없고 NullPointerException이 발생하는 상황도 생기지 않는다.

## Spring에서의 의존성 관리

(우선 Spring의 경우 스스로 객체들을 관리하는 기능을 가지고 있는데 이렇게 스프링에 의해 관리되는 객체를 Bean이라고 부른다)

Spring은 이러한 의존성 주입을 직접 해주는데 IoC Container가 이러한 역할을 수행한다.

Spring이 직접 Bean을 생성하고, 관리하며 각각의 Bean에 필요한 의존성을 직접 제어해준다. 이렇게 생성된 Bean들은 IoC Container에 저장되고 관리된다.

기존의 순서로는 

1. 객체 생성
2. 의존성 객체 생성(클래스 내부에서)
3. 의존성 객체 메서드 호출

이었다면, 스프링에서는

1. 객체 생성
2. 의존성 객체 주입(스스로가 만드는 것이 아니라 제어권을 스프링에게 위임하여 스프링이 만들어 놓은 객체를 주입)
3. 의존성 객체 메서드 호출

이다.

### IoC Container

Bean을 만들고 Bean들의 의존성을 엮어주고, 만들어진 Bean들을 제공한다.

ApplicationContext 혹은 BeanFactory를 사용한다.

- 알아서 의존성을 주입해주기 때문에 실제로 사용할 일이 없다.
- IoC Container를 사용하면 아무런 코드를 넣지 않아도 IoC Container에 등록된 Bean을 가져다 사용 가능하다.

의존성 주입은 IoC Container 안에 들어있는 Bean끼리만 가능하다.