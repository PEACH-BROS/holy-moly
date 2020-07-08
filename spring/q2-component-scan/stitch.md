# Component Scan에 대하여

## `@ComponentScan`

- Stereotype Annotation이 붙은 Bean들을 자동으로 스캔해서 등록해준다.

  > Stereotype Annotation의 종류로는 `@Component`, `@Repository`, `@Service`, `@Controller`, `@Configuration` 이 있다.
  >
  > 여기서 Stereotype은 간략히 말해, 고정된 또는 일반적인 패턴을 의미한다.

- default로  Stereotype을 스캔해서 등록해 주는데 `@ComponentScan` 의  use-default-filters 값을 false로 두면 default annotation을 스캔하지 않는다.

- basePackage의 값으로 패키지 정보를 주면 해당 패키지를 시작으로 컴포넌트 스캔을 동작시킨다.

- basePackageClasses의 값으로 클래스 정보를 주면 해당 클래스가 포함된 패키지를 시작으로 컴포넌트 스캔을 동작시킨다. 이 방법이 basePackage보다 더 type safe한 방법이다.

- include-filter 옵션을 사용하면 해당 annotation을 스캔 대상에 포함할 수 있고, exclude-filter 옵션을 사용하면 해당되는 annotation을 스캔 대상에서 제외할 수 있다.

### `@ComponentScan` 에 따른 충돌

- 동일한 이름의 Bean이 스캔된다면 BeanDefinitionStoreException이 발생하게 된다.
- 수동으로 Bean을 등록하는 경우 ComponentScan의 대상보다 우선시된다.

### `@ComponentScan` 대상 패키지가 아닌 객체를 Bean으로 등록하는 방법

- `ApplicationContextInitalizer<GenericApplicationContext>` 를 사용하면 패키니 밖에 있는 객체도 Bean으로 등록할 수 있다.

- 해당 방법은 Functional한 Bean 등록 방식이다.

  ```java
  public static void main(String [] args) {
  
      @Autowired
      MyService myService;
   
      public static void main(String[] args) {
          var app = new SpringApplication(Demospring52Application.class);
   
          app.addInitializers(
              (ApplicationContextInitializer<GenericApplicationContext>) ctx -> {
                  ctx.registerBean(MyService.class);
                  ctx.registerBean(ApplicationRunner.class, () -> args1 ->
                                   System.out.println("Hello World!!"));
              }
          );
          app.run(args);
      }
  } 
  ```

<br/>

## `@EnableAutoConfiguration`

- Bean을 등록하는 자바 설정 파일이다.
- spring.factories 안에 들어있는 수많은 자동 설정들이 조건에 따라 적용되어 수많은 Bean들이 생성된다.
- `@ComponentScan` 으로 등록한 Bean이 `@EnableAutoConfiguration` 으로 등록하는 Bean에 의해 덮어씌워질 수 있다.
  - `@ConditionalOnMissingBean`  과 같이 Condition에 따라 Bean으로 등록하는 annotation으로 해결할 수 있다.

<br/>

## 참고 자료

> [ComponentScan과 Function을 사용한 빈 등록 방법](https://atoz-develop.tistory.com/entry/Spring-Component-Scan과-Function을-사용한-빈-등록-방법)