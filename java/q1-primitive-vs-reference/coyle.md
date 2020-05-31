# 원시타입 VS 참조타입 in Java



> 들어가기 전에 타입(Data type)이란? 
>
> - 해당 데이터가 **메모리에 어떻게 저장**되고, **프로그램에서 어떻게 처리**되어야 하는지를 **명시적**으로 알려주는 것



## 분류

- Primitive type - 숫자는 바이트값
  - byte (1), char(2), short(2), int(4), long(8), float(4), double(8), boolean(1)
- Reference type - 그 외 모든 것
  - class, interface, enum, Array 등



## 저장되는 값의 차이

- Primitive type
  - 실제 값(real value)을 저장
- Reference type
  - 어딘가를 참조하기 위한 값을 저장
  - 즉, **값이 저장되어 있는 곳(Heap)의 주소값**
  - 참조 변수는 C 및 C ++에서 볼 수있는 포인터가 아니라 개체에 대한 핸들 일 뿐이므로 액세스하여 **개체 상태를 변경**할 수 있다.



## 저장되는 장소

- Primitive type
  - 스택 메모리
- Reference type
  - 힙 메모리



### 스택 메모리 vs 힙 메모리 용도 차이

- 스택 메모리
  - 컴파일 시 결정되는 기본형 데이터 타입이 저장되는 공간
  - 지역변수, 매개변수, 리턴값, 참조 변수 등이 저장
  - 메서드 호출될 때 , 메모리에 **FILO**로 하나씩 생성
    - First In Last Out 
  - 메서드 끝날 때 메모리에 LIFO로 하나씩 제거
    - Last In First Out ~~결국 같은 말 아닌가 ?~~
  - 메서드 호출시마다 각 스택 프레임(메소드만의 공간) 생성

- 힙 메모리
  - 런타임 시 결정되는 **참조형 데이터 타입이 저장되는 공간**
  - new 연산자를 통해 생성된 객체가 저장되는 공간



### 스택 메모리 vs 힙 메모리 보존 기간

- 스택 메모리
  - { } 또는 메서드가 끝날 때까지 (끝날 땐 프레임별로 삭제)

- 힙 메모리

  - 객체가 더 이상 안쓰이거나, 명시적인 Null 선언시 **GC 청소 대상**

  



## 발생하는 에러의 종류

- Primitive type
  - 컴파일 시점에 담을 수 있는 크기를 벗어나면 에러를 발생시키는 **컴파일 에러** 발생
- Reference type
  - 문법상으로는 에러가 없지만 실행시켰을 때 에러가 나는 **런타임 에러** 발생



## Null 가용성

- Primitive type
  - 래퍼 클래스 활용
  - 래퍼 클래스는 결국 기본형을 참조형 변수로 변경하는 것
  - 결론적으로 기본형에는 **Null이 존재하지 않는다.**
- Reference type
  - 빈 객체를 의미하는 Null 존재



## 얕은 복사(shallow copy)  vs 깊은 복사(deep copy)

- 얕은 복사
  - `objectA = objectB`의 형태로  **주소값을 복사**하는 방법
  - `objectB` 의 변화에 같은 주소값을 가지고 있는 `objectA` 의 값에 영향을 준다.
- 깊은 복사
  - 원본으로부터 **item 혹은 value 자체**만 복사한다.
  - Clonable 인터페이스 구현 후 `Object ObjectB = objectA.clone();`의 방식으로 복사한다.



### ~~call by Reference vs Value는 다음 주제에 있겠지...?~~



## 



