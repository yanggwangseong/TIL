
## Promise.all
### 특징
- Promise.all 메서드가 종료하는 데 걸리는 시간은 가장 늦게 fulfilled 상태가 되는 프로미스의 처리 시간보다 조금 더 길다.
- 처리 순서가 보장된다.
- Promise.all 메서드는 인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스 가 fullfilled 상태가 되는 것을 기다리지 않고 즉시 종료한다.
