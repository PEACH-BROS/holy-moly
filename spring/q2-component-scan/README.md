# 컴포넌트 스캔에 대해 설명해주세요.

[엘리의 컴포넌트 스캔](elly-component-scan.md)

[비밥의 컴포넌트 스캔](bebop.md)

[스티치의 컴포넌트 스캔](stitch.md)



## @ComponentScan

컴포넌트라 불리는 스프링 Bean 객체들은 다음과 같은 어노테이션을 가지고 있다.

@Component

- @Controller

- @Service
- @Repository
- @Configuration

스프링 부트 어플리케이션이 실행 될 때 Bean을 생성하기 위해서 컴포넌트를 찾는 컴포넌트 스캔이 필요하다.

default로 Stereotype을 스캔해서 등록해 주는데 `@ComponentScan` 의 use-default-filters 값을 false로 두면 default annotation을 스캔하지 않는다.

basePackage의 값으로 패키지 정보를 주면 해당 패키지를 시작으로 컴포넌트 스캔을 동작시킨다.

basePackageClasses의 값으로 클래스 정보를 주면 해당 클래스가 포함된 패키지를 시작으로 컴포넌트 스캔을 동작시킨다. 이 방법이 basePackage보다 더 type safe한 방법이다.

include-filter 옵션을 사용하면 해당 annotation을 스캔 대상에 포함할 수 있고, exclude-filter 옵션을 사용하면 해당되는 annotation을 스캔 대상에서 제외할 수 있다.

### `@ComponentScan` 에 따른 충돌

- 동일한 이름의 Bean이 스캔된다면 BeanDefinitionStoreException이 발생하게 된다.
- 수동으로 Bean을 등록하는 경우 ComponentScan의 대상보다 우선시된다.



## @EnableAutoConfiguration

결국 Configuration이다. 즉, Bean을 등록하는 자바 설정 파일이다.

![elly hello 1](https://user-images.githubusercontent.com/19922698/86936383-e75ba280-c178-11ea-814d-4c50d9ebe608.png)

`spring.factories` 내부에 여러 Configuration들이 있고, 조건에 따라 Bean으로 등록한다.

`@EnableAutoConfiguration`에 의해 `spring.factories` 안에 들어있는 수많은 자동 설정들이 조건에 따라 적용되어 수많은 Bean들이 생성된다.

pom.xml 또는 build.gradle에 추가한 jar 의존성에 기초해 Spring Boot가 내부적으로 필요한 빈들을 띄우기 위해 필요하다.

+ 외부의 설정 파일들을 읽어올 때 유용하다.

SpringBootApplication에는 `@ComponentScan`과 `@EnableAutoConfiguration`이 모두 걸려있다. @ComponentScan을 먼저 한다.

![elly hello 2](https://user-images.githubusercontent.com/19922698/86932552-7b773b00-c174-11ea-90da-d22fd67574d5.png)



## @ComponentScan

말 그대로 `@Component` 어노테이션을 찾는 어노테이션이다. Filter를 명시하여 추가할 컴포넌트, 제외할 컴포넌트를 지정할 수 있으며 컴포넌트를 찾을 위치를 지정할 수 있다.



## 순서

스프링 부트 어플리케이션이 실행되면 먼저 `@ComponentScan`으로 정의한 Bean을 등록한다.
이후에 `@EnableAutoConfiguration`으로 추가적인 Bean을 등록하게 된다.

따라서 **`@ComponentScan`으로 등록한 빈이 `@EnableAutoConfiguration`으로 등록하는 빈에 의해 덮어씌워질 수 있다.**
이러한 문제점을 해결하기 위해서는 `@ConditionalOnMissingBean` 과 같은 **Condition에 따라 빈을 생성하는 정책을 정할 수 있는 어노테이션을 이용하여 해결**해야 한다.
