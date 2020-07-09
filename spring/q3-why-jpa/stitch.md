# 왜 프로젝트에 JPA를 도입하셨나요?

## I Say

JPA를 실무에서 더 많이 사용하니까요!!!라고 하면 안되겠죠? 먼저 JDBC는 connection부터 시작해서 많은 세세한 부분들을 직접 관리해야 했습니다. 코드의 중복도 많이 발생하기 때문에 선택사항에서 제외되었습니다. JDBC의 중복된 코드들을 추상화한 Spring JDBC도 있었습니다. Connection과 RowMapper등이 추상화되어 사용하기 쉬웠으나 그래도 직접 쿼리를 작성하여 관리하는 점은 큰 단점이었습니다. 이런 부분들을 해결해준 기술로는 Spring Data JDBC와 JPA가 있었습니다. 기존에 Spring Data JDBC를 사용했었지만 이번에 JPA가 가지는 객체지향적인 설계와 쿼리, 영속성 컨텍스트 등의 장점을 경험해보고 싶어서 선택했고, 여기에서 좀 더 사용자에게 편리하고 추상화되어있는 Spring Data JPA를 선택하였습니다.



## Persistence

영속성(*persistence*)이란 데이터를 생성한 프로그램이 종료되더라도 사라지지 않는 데이터의 특성을 말한다.

영속성을 갖지 않는 데이터는 단지 메모리에서만 존재하기 때문에 프로그램을 종료하면 모두 잃어버리게 된다. 때문에 파일 시스템, 관계형 데이터베이스 혹은 객체 데이터베이스 등을 활용하여 데이터를 영구하게 저장하여 영속성을 부여한다.



##  SQL Mapper & ORM

Persistence Framework는 SQL Mapper와 ORM으로 나눌 수 있다.

ORM은 데이터베이스 객체를 자바 객체로 매핑함으로써 객체 간의 관계를 바탕으로 SQL을 자동으로 생성해주지만 SQL Mapper는 SQL을 명시해줘야 한다.

ORM은 관계형 데이터베이스의 '관계'를 객체에 반영하자는 것이 목적이라면 SQL Mapper는 단순히 필드를 매핑시키는 것이 목적이라는 점에서 지향점의 차이가 있다.



## JDBC

JDBC는 DB에 접근할 수 있도록 Java에서 제공하는 API이다.

모든 Java의 Data Access 기술의 근간이다. 즉 Java의 모든 Persistence Framework는 내부적으로 JDBC API를 이용한다.

직접 connection을 설정하는 등 세부적인 설정을 사용자가 직접 해줘야 한다.



## MyBatis

개발자가 지정한 SQL, 저장 프로시저 그리고 몇가지 고급 매핑을 지원하는 SQL Mapper이다.

JDBC로 처리하는 상당 부분의 코드와 파라미터 설정 및 결과 매핑을 대신해준다.

장점

- SQL에 대한 모든 컨트롤을 하고자 할 때 적합하다.
- SQL 쿼리들이 매우 잘 최적화되어 있을 때에 유용하다.

단점

-  애플리케이션과 데이터베이스 간의 설계에 대한 모든 조작을 하고자 할 때는 적합하지 않다.
  - 이는 패러다임간의 불일치로 인해 발생하는 문제점이다.



## JPA

Java ORM 기술에 대한 API 표준 명세로 Java에서 제공하는 API이다.

대표적인 구현체로는 Hibernate, EcliipseLink, DataNucleus, OpenJPA, TopLink Essentials 등이 있다.

장점

- 객체지향적으로 데이터를 관리할 수 있기 때문에 비즈니스 로직에 집중할 수 있으며, 객체지향 개발이 가능하다.
- 테이블 생성, 변경, 관리가 쉽다.
- 로직을 쿼리에 집중하기보다는 객체 자체에 집중할 수 있다.
- 빠른 개발이 가능하다.

단점

- 어렵다
- 잘 이해하고 사용하지 않으면 데이터 손실이 있을 수 있다.
- 성능상 문제가 있을 수 있다.



## 참고 자료

> [코즈의 JDBC, SQL MAPPER, ORM](https://www.youtube.com/watch?v=mezbxKGu68Y)
>
> [JDBC, JPA/Hibernate, Mybatis의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html)