# ComponentScan의 종류

### @ComponentScan

`@Component` 어노테이션을 가진 클래스들을 스캔해서 Bean으로 띄운다. `@ComponentScan`이 붙은 클래스가 속한 패키지 전체를 검사한다.

ex. `@Service`, `@Controller`, `@Repository`, `@RestController`



### @EnableAutoConfiguration

결국 Configuration이다. 즉, Bean을 등록하는 자바 설정 파일이다.

![image](https://user-images.githubusercontent.com/19922698/86936383-e75ba280-c178-11ea-814d-4c50d9ebe608.png)

`spring.factories` 내부에 여러 Configuration들이 있고, 조건에 따라 Bean으로 등록한다.

`@EnableAutoConfiguration`에 의해 `spring.factories` 안에 들어있는 수많은 자동 설정들이 조건에 따라 적용되어 수많은 Bean들이 생성된다.



pom.xml 또는 build.gradle에 추가한 jar 의존성에 기초해 Spring Boot가 내부적으로 필요한 빈들을 띄우기 위해 필요하다.

+외부의 설정 파일들을 읽어올 때 유용하다.



SpringBootApplication에는 `@ComponentScan`과 `@EnableAutoConfiguration`이 모두 걸려있다. @ComponentScan을 먼저 한다.

![image](https://user-images.githubusercontent.com/19922698/86932552-7b773b00-c174-11ea-90da-d22fd67574d5.png)







> 참고
>
> https://velog.io/@max9106/Spring-Boot-EnableAutoConfiguration  
> https://stackoverflow.com/questions/35005158/what-is-the-difference-between-componentscan-and-enableautoconfiguration-in-sp