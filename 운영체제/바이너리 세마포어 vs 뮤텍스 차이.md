
# 바이너리 세마포어vs 뮤텍스 차이점


| 바이너리 세마포어                                      | 뮤텍스                                     |
| ---------------------------------------------- | --------------------------------------- |
| 신호 전달 메커니즘 기반으로 동작                             | 잠금 메커니즘 기반으로 동작                         |
| 현재 스레드보다 우선순위가 높은 스레드가 바이너리 세마포어를 해제하고 잠글 수 있음 | 뮤텍스를 획득한 스레드는 크리티컬 섹션에서 나갈 때만 뮤텍스 해제 가능 |
| 값은 `wait()` , `signal()` 에 따라 변경               | 값이 locked, unlocked 으로 수정               |
| 여러 개의 스레드가 동시에 이진 세마포어를 획득 가능                  | 한 번에 하나의 스레드만 뮤텍스를 획득 가능                |
| *소유권이 없다*                                      | 뮤텍스를 소유한 스레드만 잠금을 해제 가능하므로 *소유권이 있음*    |
| 다른 스레드/프로세스가 잠금 해제가 가능하기 때문에 뮤텍스보다 빠르다.        | 획득한 스레드만 잠금 해제가 가능하므로 바이너리 세마포어보다 느리다.  |
|                                                |                                         |


# Reference
- [https://blog.seongjun.kr/26-difference-between-binary-semaphore-and-mutex/](https://blog.seongjun.kr/26-difference-between-binary-semaphore-and-mutex/) 
- [https://www.geeksforgeeks.org/difference-between-binary-semaphore-and-mutex/](https://www.geeksforgeeks.org/difference-between-binary-semaphore-and-mutex/) 

