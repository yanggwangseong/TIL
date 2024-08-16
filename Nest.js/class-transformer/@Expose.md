
# @Expose
- @Exclude와 반대로 없는 프로퍼티를 생성 하여 사용 할 수 있게 해준다.
- getter 와 함께 사용 된다.

```typescript
import { Expose } from 'class-transformer';

...

nickname: string;

email: string;

@Expose()
get nicknameAndEmail(){
	return this.name + '/' + this.email;
}

...


/*
* (json)
* data: {
*   nickname: 'apple',
*   email: 'test@test.com',
*   nicknameAndEmail: 'apple/test@test.com'
* }
*/

```

- @Expose 옵션은 @Exclude와 같습니다.
- 제외 시킬지 말지 용도에 따라 사용 되고 다른방법은 @Exclude와 같다



