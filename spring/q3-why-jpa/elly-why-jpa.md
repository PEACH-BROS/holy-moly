# 왜 프로젝트에 JPA를 도입하셨나요?

- **SQL 중심 개발에서 객체 중심 개발이 가능해진다.**

  SQL 중심적인 개발에서는 지루한 코드가 무한 반복된다. CRUD가 반복되고, 자바 객체를 SQL로, SQL을 자바 객체로 변환하는 과정이 반복된다.

  예를 들어, 회원 객체에 나이 정보를 추가하고 싶을 때, Member 클래스에 `private int age;`를 추가하면 관련된 모든 쿼리에 age를 추가해야 한다.

- **RDB와 객체 지향 프로그래밍 간의 패러다임 불일치 문제를 해결할 수 있다.**

  객체지향 프로그래밍은 객체들 간의 협업을 중심으로 하는 프로그래밍이고, RDB(관계형 데이터베이스)는 데이터를 잘 정규화해서 보관하는 것이 목표이다.

  대표적으로 객체에는 상속 관계가 있고, RDB에는 상속 관계가 없다.

  패러다임이 다른 두 가지를 억지로 매핑하려다 보니 불필요한 매핑 과정이 너무 많이 발생한다.

- **영속성 컨텍스트를 제공한다.**

  영속성 컨텍스트는 버퍼 역할을 한다. 영속된 엔티티를 조회하면 DB에 SELECT 쿼리를 날릴 필요 없이, 영속성 컨텍스트 안에서 바로 조회가 가능하다. + `lazy loading`이나 `dirty checking` 기능도 제공한다.

  *성능이 눈에 띄게 향상되는지는 의문이다.*





> 그림 자료

![image](https://user-images.githubusercontent.com/19922698/87153314-3d078a80-c2f2-11ea-94ab-26ea3db38959.png)

JPA는 애플리케이션과 JDBC API 사이에서 동작한다.

개발자가 JPA를 사용하면, JPA 내부에서 JDBC API를 호출해 DB 와 통신한다.



Hibernate는 JPA의 구현체이다.

ex. JPA에서 정의하고 있는 `EntityManager` 인터페이스를 Hibernate는 구현하고 있다.

```java
package javax.persistence;

import ...

public interface EntityManager {

    public void persist(Object entity);

    public <T> T merge(T entity);

    public void remove(Object entity);

    public <T> T find(Class<T> entityClass, Object primaryKey);

    // More interface methods...
}
```





![image](https://user-images.githubusercontent.com/19922698/87174678-810a8780-c312-11ea-9786-9fd3928eebb1.png)





### MyBatis?

SQL Mapper의 일종. JDBC를 좀 더 편하게 사용할 수 있도록 객체를 SQL 구문을과 자바 메소드와 매핑해준다.

SQL Mapper는 직접 SQL을 명시해줘야 한다.

반복적인 코드 문제는 해결되지 않는다.





> 참고
>
> https://gmlwjd9405.github.io/2019/08/03/reason-why-use-jpa.html  
> Jason의 JPA 핸즈온  
> https://suhwan.dev/2019/02/24/jpa-vs-hibernate-vs-spring-data-jpa/  
> https://gmlwjd9405.github.io/2018/12/25/difference-jdbc-jpa-mybatis.html