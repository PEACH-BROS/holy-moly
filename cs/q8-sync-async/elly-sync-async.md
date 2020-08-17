# 동기와 비동기

## 동기 synchronous

이벤트를 자신이 *직접* 처리. (확인의 주체가 유저 프로세스이며, 다 될 때까지 기다리거나 스스로 확인)

<br/>

## 비동기 asynchronous

이벤트 핸들러 (callback)에 의해 처리. (callback 함수가 호출되기까지 다른 작업 가능)

ex. 시험날의 학생과 선생님

![image](https://user-images.githubusercontent.com/19922698/90335287-f0a91c00-e00e-11ea-8116-0b16ec950cb9.png)

```
1. 학생이 시험문제를 푼다.
2. 선생님에게 시험지를 건낸다.
3. 선생님은 채점을 한다.
4. 채점이 끝나면 채점된 시험지를 학생에게 건넨다. (그 동안 학생은 밥도 먹고, 잠도 잔다.)
5. 학생은 시험 결과를 확인한다.
```

학생과 선생님은 "시험지"라는 연결고리가 있지만, 시험지에 행하는 행위(목적)는 서로 다르다. 서로의 행위가 다르기 때문에 둘의 작업 처리 시간을 일치하지 않고, 일치하지 않아도 된다.



<br/>

---

### 🤔 Sync/Async와 Blocking/Non-Blocking 뭐가 다르지?

### : 관심사가 다르다.

Sync와 Blocking이 비슷하고, Async와 Non-Blocking이 비슷하지만, 둘은 관심사가 다르다.

### **Blocking/Non-Blocking**

>  호출된 함수가 바로 return하느냐 마느냐. (제어권을 돌려받느냐 마느냐)

<br/>

호출된 함수가 자신의 작업을 모두 마칠 때까지 호출한 곳에 *제어권을 넘겨주지 않고 대기하게 만들면 Blocking*

❗학생이 시험지를 선생님에게 건넨 후 가만히 앉아 채점이 끝나기만을 기다린다. -> 학생은 **블록 상태**이다.

<br/>

호출된 함수가 바로 리턴해서 호출한 곳에 *제어권을 넘겨주고, 다른 일을 할 기회를 줄 수 있으면 Non-Blocking*

❗️ 학생이 시험지를 선생님께 건넨 후 채점을 기다리지 않고 다른 일도 한다면 학생은 **논블록 상태**이다.

<br/>

### **Sync/Async**

> 호출된 함수의 작업 완료 여부를 누가 신경쓰냐

<br/>

호출된 함수 작업 완료 후 return을 기다리거나, 바로 return받더라도 작업 완료 여부를 *호출한 곳에서 계속 신경쓰는 방식이 Synchronous*

<br/>

호출된 함수에게 callback을 전달해서 그쪽의 작업이 완료된 후 전달받은 callback을 실행하고, *호출한 곳은 신경쓰지 않으면 Asynchronous*

​	- 비동기 작업(호출된 함수의 작업)을 다른 쓰레드에서 진행하고, 이를 전달받은 callback을 Future 객체에 담을 수 있다.





🤔 관심사에 따라 같은 현상에 대해서도 Sync 또는 Block이라고 말 할 수 있는걸까?

<br/>

---

### 카페에서 커피를 주문하는 것을 예로 들어보자.

1. 커피를 타달라는 요청이 들어왔다.

- 이 떄, 커피가 있으면 타준다. (block/non-block 모두)

- 커피가 없는 경우, **block**: "잠깐만요" 하고 사러 간다.

​						**non-block**: "커피가 없어요." 하고 사러 간다.

- **동기**: 커피가 타졌는지 안 타졌는지 내가 확인한다.
- **비동기**: 벨이 울리면 받으러 간다.







> 참고
>
> https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/  
> https://brainbackdoor.tistory.com/26  
> https://parkcheolu.tistory.com/15  
> https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/  

