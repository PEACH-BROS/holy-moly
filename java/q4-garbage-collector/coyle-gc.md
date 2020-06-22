JVM

- OS의 메모리 영역에 접근하여 메모리를 관리하는 프로그램
  - GC 수행



GC

- 동적으로 할당된 메모리 영역 중 사용하지 않는 영역을 탐지하여 해제



사용하지 않는 영역의 기준

- reachable
  - root set으로부터 객체 그래프를 탐색 후 마킹
  - 마킹되지 않은 놈들만 제거



Gc 과정

1. Stack의 모든 변수를 스캔하면서 각각 어떤 객체를 참조하고 있는지 찾아서 마킹
2. Reachable Object가 참조하고 있는 객체도 찾아 마킹
3. 마킹되지 않은 객체를 Heap에서 제거



Heap 영역

- New Generation
  - Eden
  - Survival 0
  - Survival 1
- OldGeneration



Minor GC

- 새로운 객체들이 이 영역에 할당된다. Eden과 2개의 Survival 영역으로 나누어 지며 Young 영역에서 일어나는 GC 를 'Minor GC'라 한다.

Promotion

- Age 값이 특정 값 이상이 되면 Old Generation 영역으로 옮겨지는 것 

Major GC (Full GC)

- Old Generation 영역에서 발생하는 GC

MetaSpace 

- 자바8부터 등장했으며 자바7까지는 Permanent generation영역 이었으나 제거 되었다. 클래스 메타데이터들이 이 영역에 할당된다.
- 메타 데이터라고 하면 클래스의 정보 ? => Method Area에 있어야하는 거 아녀  ?



Stop the world

- GC 실행 시 JVM이 애플리케이션 실행을 멈추는 것
- GC를 실행하는 스레드를 제외한 나머지 스레드는 모두 작업을 멈춘다.



왜 Stop the world가 발생해야하지 ?



Weak Generational Hypothesis

- 대부분의 객체는 금방 접근할 수 없는 상태가 된다
- 오래된 객체가 그렇지 않은 객체를 참조하는 일은 거의 없다
  - 만약 오래된 객체가 그렇지 않은 객체를 참조한다면 'card table' 이라는 JVM이 관리하는 바이트 배열을 둔다.
  - 오래된 객체가 젊은 객체를 참조할 때는 이 카드 테이블에 정보를 기록하고 Minor GC 시 Old 영역을 스캔하지 않고 카드 테이블을 통해 GC 대상인지 식별

GC의 종류

1. Serial GC
   - GC를 처리하는 스레드가 1개
   - CPU코어가 1개만 있을 때 사용
   - Mark-Compact Collection 알고리즘
     - 데이터 해제 후 한곳으로 몰아둬 메모리 파편화를 방지
2. Parallel GC
   - GC 처리 스레드 여러 개
   - Serial GC보다 빠르게 객체 처리
   - Parallel GC는 메모리가 충분하고 코어의 개수가 많을 때 사용하면 좋다.
3. Concurrent Mark Sweep GC (CMS GC)
   - Stop the world를 줄인다.
   - 빠른 응답이 중요한 어플리케이션에 사용된다.
   - 다른 GC보다 메모리와 CPU를 더 많이 사용한다.
     - GC 사이클이 조금 더 길기 때문에
   - Compaction 존재 X
   - Initial Mark
     - Stack의 모든 변수를 스캔하면서 각각 어떤 객체를 참조하고 있는지 찾아서 마킹
   - Concurrent Mark
     - 이후의 객체 그래프 참조 마킹
   - Remark
     - Initial Mark, Concureent Mark 과정 중 다른 스레드가 새로 생성한 객체들을 다시 한번 체크 후 마크
   - Concurrent Sweep
     - 마킹안된 데이터를 해제
     - 다른 스레드 같이 실행된다

4. G1 GC
   - 각 영역을 Region 영역으로 나눈다.
   - GC가 일어날 때 전체 영역을 탐색하지 않는다.
   - 바둑판의 각 영역에 객체를 할당하고 GC를 실행
     - 영역이 꽉차면 빈 영역에 객체를 할당하고 GC를 실행
   - STW 시간이 짧다.
   - Compaction을 사용
