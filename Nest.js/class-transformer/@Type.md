
# @Type
- 해당 데코레이터는 일반적으로 쿼리스트링은 전부 string이다. 근데 만약에 dto에 validation 해야될 프로퍼티의 값이 number여야 한다면? 어떻게 될까 무조건 validation 에러가 난다.
- 해당 프로퍼티의 validation value값의 타입을 변환 해주는 데코레이터라고 생각하면 된다.

```typescript
import { Type } from 'class-transformer';

@Type(() => Number) //where__id_more_than의 쿼리 스트링으로 받는것들은 전부 string이라 무언가 변환 해주는건 class-transformer
@IsOptional()
@IsNumber()
where__id_more_than?: number;
```

> 해당 where__id_more_than값이 '28' 이렇게 string으로 들어오는 쿼리 스트링인데 그 값을 *@Type(() => Number) number*  타입으로 변환 해준다.

