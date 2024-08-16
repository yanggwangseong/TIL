
# @Exclude
- 보통적으로 어떤경우에 사용하냐면, 해당 프로퍼티를 제외 시키고 싶을때 사용한다
- class-transformer에서 제공하고, 사용 하려면 *@UseInterceptors(ClassSerializerInterceptor)* ClassSerializerInterceptor(@nestjs/common에서 제공)를 이용해줘야 한다.

```typescript
import { Expose } from 'class-transformer';

@Entity()
export class UserModel extends BaseModel{

	...

	@Exclude()
	password: string
	
	...
}
```
- 해당 class-transformer에서 제공해주는 *@Exclude* 데코레이터를 이용하여 password프로퍼티를 제외 할 수 있다.


## 🍙 @Exclude 옵션
```typescript
export declare function Exclude(options?: ExcludeOptions): PropertyDecorator & ClassDecorator;
```

```typescript
export interface ExcludeOptions {

/**
* Expose this property only when transforming from plain to class instance.
*/
toClassOnly?: boolean;

/**
* Expose this property only when transforming from class instance to plain object.
*/
toPlainOnly?: boolean;

}
```

> Request
> frontend -> backend
> plain object (JSON) -> class instance (dto)
> 
> Response
> backend -> frontend
> class instance (dto) -> plain object(JSON)
> 
> toClassOnly -> class instance 변환될때만 (요청 Request)
> toPlainOnly -> plain object로 변환될때만 (응답 Response)

- 즉 해당 옵션으로 어떻게 줄 수 있냐면 *toClassOnly* 값을 true로 주면 요청이 들어올때만 @Exclude 제외 시킨다는뜻
- *toPlainOnly* 값을 true로 주면 응답을 해줄때만 @Exclude로 제외 시킨다는 뜻


> 활용

```typescript
...

@Exclude({
	toPlainOnly: true
})
password: string;
...

/*
* (응답 Reponse일때만 password 프로퍼티를 제외한다 )
*/
```


> 만약 아주 중요한 entity라 response시에 절대 노출이 되면 안된다고 가정 했을때 class에 줄 수 도 있다.

```typescritp
import { Exclude } from 'class-transformer';

@Exclude()
export class UsersModel extends BaseModel{
	...
}


/*
* (json)
* data : {
*   post:{
*      id: 17;
*      title: '포스트글1',
*      user:{} <-- class에 @Exclude를 주어서 빈객체로 반환된다.
*   }
* }
*/
```





