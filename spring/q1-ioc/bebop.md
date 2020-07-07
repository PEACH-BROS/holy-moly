# 스프링의 IoC에 대해 설명해보세요.

IoC는 **Inversion of Control**의 약자이며 단어의 뜻 그대로 **제어의 역전**을 말한다.  

일반적으로 객체의 생명주기는 프로그래머에 의해 결정되게 된다.  
대표적으로 객체의 생성은 `new` 키워드를 통해 이루어진다.  
하지만 IoC는 이러한 **객체의 생명주기를 프로그래머가 관리하는 것이 아닌 Spring이( *IoC 컨테이너가* ) 관리하게 하는 것**을 의미한다.  
따라서 객체의 생명주기를 프로그래머가 제어하는 것이 아니라는 의미에서 IoC라는 말을 사용한다.

Spring 에서 Bean( *스프링에서 제어권을 가지는 객체* )을 사용할 때 우리는 Bean을 직접 생성(`new`)하지 않는다.  
Bean이라고 등록만 한다면 **IoC 컨테이너**가 알아서 Bean의 생명주기를 관리하여 생성부터 파괴까지 관리하게 된다.

Spring의 IoC 컨테이너는 ApplicationContext 혹은 BeanFactory를 사용한다.

사실상 BeanFactory를 사용한다고 볼 수 있는데, ApplicationContext가 BeanFactory를 상속받고 있기 때문이다.

가장 핵심적인 기능은 Bean을 만들고 Bean 간의 의존성을 엮고 Bean을 제공하는 것이다.

> 참고

https://jojoldu.tistory.com/28

https://www.youtube.com/watch?v=NOAajiABq6A