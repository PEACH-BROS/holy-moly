# CPU 스케줄링

#### 스케줄링?

프로세스를 실행하는 순서를 결정한다.

🤔 **어떻게 프로세스들이 CPU를 효율적으로 사용하게 할 것인가?** 에서 출발.



<br/>

### 프로세스의 생명주기

준비 / 대기 / 실행 / 종료

![image](https://user-images.githubusercontent.com/19922698/89120971-beb79600-d4f5-11ea-8466-d08172317577.png)

<br/>

### CPU 스케줄링이 발생하는 시점

- 입출력 요청 : 실행(running) → 대기(waiting) `//이 상황에서 발생하는 스케줄링 → 비선점` 
- 인터럽트 발생 : 실행 → 준비(ready) `//이 상황에서 발생하는 스케줄링 → 선점` 
- 입출력 종료 : 대기 → 준비 `//선점`
- 종료 : 실행 → 종료(terminated) `//비선점`

**❗️ 그냥 상태가 변할 때 발생하나보다.** 



<br/>

크게 2가지로 분류된다.

<br/>

## 선점 스케줄링

프로세스가 CPU를 할당받아 실행 중이더라도, OS가 CPU를 강제로 뺏을 수 있다. (우선순위가 높다면)

:thumbsup: CPU 처리 시간이 매우 긴 프로세스가 CPU 독점하는 것을 막을 수 있다.

:thumbsup: 우선순위가 높은 프로세스를 빠르게 처리할 때 유용하다.

:thumbsdown: context switching이 잦아 오버헤드가 자주 발생한다.



- **SRTF (Shortest Remaining Time First) - SJF의 선점 방식**

  먼저 온 프로세스가 CPU를 할당받고 있어도, `남은 처리시간`이 `뒤에 온 프로세스의 처리시간`보다 크면 뒤에 온 프로세스에게 CPU를 빼앗긴다.

- **RR (Round Robin)**

  프로세스에게 각각 동일한 CPU 할당 시간을 부여해서 이 시간동안만 CPU를 사용하게 한다. 다 처리하지 못하면 뺏기고, 다음 순서가 돌아왔을 때 마저 처리한다. 뺏긴 프로세스는 준비 큐의 맨 뒤(Tail)로 간다.

  :slightly_smiling_face: *장점 : Response time이 빨라진다.*

  - n 개의 프로세스가 준비 큐에 있고, 할당 시간이 q인 경우, 각 프로세스는 q 단위로 CPU 시간의 1/n을 얻는다. 즉, 어떤 프로세스도 (n-1) * q 시간 이상을 기다리지 않는다.
  - 프로세스가 기다리는 시간이 CPU를 사용할 만큼 증가하므로 공정한 스케줄링이라고 할 수 있다.

- **Priority - 선점형** 

  우선순위가 가장 높은 프로세스에게 CPU를 할당한다. 더 높은 우선순위의 프로세스가 도착하면 실행 중인 프로세스를 멈추고 CPU를 선점한다. 

- **다단계 큐**

  준비 큐를 여러 개 사용하는 기법. 각 큐는 자신의 스케줄링 알고리즘을 수행하며, 큐 자체에 우선순위가 부여된다.

- **다단계 피드백 큐**

  특정 그룹의 준비 큐에 들어간 프로세스가 다른 준비 큐로 이동할 수 없는 다단계 큐 기법을 이동 가능하도록 개선한 기법

<br/>

## 비선점 스케줄링

프로세스가 CPU를 점유하고 있다면 이를 뺴앗을 수 없다.

:thumbsup: 오버헤드가 상대적으로 적다.

:thumbsdown: CPU 처리 시간이 매우 긴 프로세스가 CPU 독점하는 것을 막을 수 없다.



- **FCFS (First Come First Served)**

  선입선출. 모든 프로세스의 우선순위 동일. 준비 큐에 도착한 순서대로 CPU를 할당한다.

- **SJF (Shortest Job First)**

  준비 큐에 있는 프로세스 중 실행 시간이 가장 짧은 놈부터 CPU를 할당한다.

- **HRN(Highest Response Ratio Next)**

  수행 시간과 대기 시간을 모두 고려해 우선순위를 정한다.

- **Priority - 비선점형**

  더 높은 우선순위의 프로세스가 도착하면 준비 큐의 Head에 넣는다.




### **🤔 요즘 CPU들은 뭘 쓰지?**

RR





> 참고
>
> https://preamtree.tistory.com/19  
> https://www.studytonight.com/operating-system/cpu-scheduling  
>