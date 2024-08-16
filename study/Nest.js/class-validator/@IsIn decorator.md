
# @IsIn
- 프로퍼티가 만약 리터럴 타입과 유니온타입으로 선언 되어 있다면 올 수 있는 값들을 정해준다.

```typescript
import { IsIn } from 'class-validator';

@IsIn(['ASC', 'DESC'])
order__createdAt: 'ASC' | 'DESC' = 'ASC';
```

> order__createdAt 프로퍼티는 'ASC' 와 'DESC' 리터럴 타입이 올 수 있으므로, 해당 리터럴 타입만 올 수 있게 *@IsIn* 데코레이터로 검증 해준다.


