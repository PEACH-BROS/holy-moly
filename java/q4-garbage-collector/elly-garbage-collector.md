# Garbage Collector

## GC의 작동 과정

1. **Mark**: GC는 Stack의 모든 변수를 스캔하면서 각각 어떤 객체를 참조하고 있는지 찾아서 마킹한다. Heap의 Reachable Object가 참조하고 있는 객체도 찾아서 마킹한다.
2. **Sweep**: 마킹되지 않은 객체를 Heap에서 제거한다.



## GC는 언제 일어날까?

* Minor GC

  

* Major GC

Eden Survival 0 Survival 1 Old Generation



## GC의 종류



### Stop the World

