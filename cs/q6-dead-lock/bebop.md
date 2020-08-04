# DeadLock 이 뭔가요? 해결 방법은?

### Dead Lock?

Dead Lock (이하 데드락)은 프로세스가 자원을 얻지 못해 다음 작업을 진행하지 못하는 상태이다.  
다른 말로 교착상태라고 부른다.

### 언제 발생하나?

한정된 자원을 여러 곳에서 사용 할 때 발생한다.  

한정된 자원을 단순히 여러 곳에서 사용 할 때 발생한다기 보다 몇가지 조건이 충족되면 발생할 수 있다.

#### 데드락의 발생 조건

1. 상호배제 (Mutual Exclusion)
   - 자원은 한 번에 한 프로세스만 사용가능하다.
2. 점유대기 (Hold and Wait)
   - 하나의 자원을 점유한( *할당* ) 상태로 다른 프로세스가 사용하고 있는 자원을 기다리는( *대기* ) 프로세스가 존재해야한다.
     - 순환대기는 이러한 점유대기 상태의 프로세스가 서로의 자원을 대기하는 상태이다.
3. 비선점 (No Preemption)
   - 다른 프로세스가 할당받은 자원을 사용이 끝나기 전에 강제로 빼앗아( *선점* ) 올 수 없다.
4. 순환대기 (Circular Wait)
   - 사용자 1이 A자원을 가지고 B를 요청한다.
   - 사용자 2이 B자원을 가지고 A를 요청한다.
   - 이와 같이 서로가 필요로 하는 자원을 가지고 서로가 점유하고 있는 자원을 요청하는 상태가 되어야한다.

### 해결 방법은?

데드락의 발생조건중 한 가지라도 충족되지 않으면 데드락이 발생하지 않는다.

크게 교착 상태를 **예방, 회피, 탐지, 회복**하는 방법이 있다.

#### 교착 상태 예방

각 데드락(교착상태) 발생조건의 반대라고 생각하면 된다.

#### 교착 상태 회피

데드락이 발생하면 피하는 방식으로 대표적으로 은행원 알고리즘이 있다.

은행원 알고리즘은 프로세스가 자원을 요구할 때 시스템이 자원을 할당한 후에도 안전 상태로 남을 수 있는지 검사하여 자원을 할당할지 하지 않을지 결정하는 방식이다.

안전 상태란, 시스템이 데드락 발생없이 각 프로세스가 요구한 최대 요구량 만큼 필요한 자원을 할당해 줄 수 있는 상태의 프로세스 실행 순서가 존재하는 상태를 의미한다.

#### 교착 상태 탐지

자원 할당 그래프를 통해 교착 상태를 탐지한다.

자원 할당 그래프에서 Cycle ( *순환* )이 발견되면 교착 상태의 가능성이 있다고 본다.

반드시 순환이 발견된다고 교착 상태에 빠지는 것은 아니고 프로세스가 자원을 반납한 상황을 가정해 보았을 때도 순환이 풀리지 않는다면 교착 상태라고 본다. 

교착 상태에 빠진 순환

![image](https://user-images.githubusercontent.com/13347548/88688402-3f604600-d134-11ea-90f7-0f2840eb0bbf.png)

교착 상태에 빠지지 않은 순환

![image](https://user-images.githubusercontent.com/13347548/88688518-63238c00-d134-11ea-88ef-f8fcdf6d784a.png)

#### 교착 상태 회복

교착 상태가 발생하면 프로세스를 종료하거나, 할당된 자원을 해제하여 교착상태를 회복하는 것을 의미한다.

프로세스를 종료하는 방식은 **교착상태의 프로세스를 모두 중지**시키는 방법과, **교착 상태가 제거될 때 까지 프로세스를 하나씩 중지** 시키는 방식이 있다.

회복하는 방식은 교착 상태에 빠진 프로세스가 점유한 자원을 빼앗아( 선점 ) 다른 프로세스에게 할당하고 빼앗긴 프로세스는 일시 정지 시키는 방식이다.

보통 우선 순위가 낮은 프로세스, 수행된 횟수가 적은 프로세스를 위주로 자원을 빼앗아 간다.