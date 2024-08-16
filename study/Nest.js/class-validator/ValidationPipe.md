
# ValidationPipe
- class-validator로 @IsString() @IsEmail등 validation을 하려면 2가지가 필요한데 그중 하나가
- *ValidationPipe* 를 app.useGlobalPipes(new ValidationPipe()) 해주어야 한다.
- 그리고 꼭 class-transformer도 설치 해주어야한다.

```typescript
// main.ts

import { ValidationPipe } from '@nestjs/common';

async function bootstrap(){
	...

	app.useGlobalPipes(new ValidationPipe())
	
	...
}
```

## 🍘 ValidationPipeOptions

- [0]  1. transform

- 인스턴스를 변환을 해줘도 허용 된다.

> transform 옵션을 true로 주지 않으면 class-validation과 class-transfomer가 기본적으로 동작하는 방식이 값을 넣지 않으면 값을 넣지 않은대로 동작 하는게 기본, 프로퍼티에 넣어준 기본값들이 있는데 전달된 파라미터가 없을경우에 기본값을 넣은채로 인스턴스를 생성해도 괜찮다 라는 허가 해주는 옵션

```typescript
export class PaginatePostDto {
	@IsOptional()
	@IsIn(['ASC' | 'DESC'])
	order__createdAt: 'ASC' | 'DESC' = 'ASC';
}

// controller
getPosts(@Query() query: PaginatePostDto) {
...
}
```

> 페이징을 위해 query 옵션들을 검증하는 PaginatePostDto를 생성해서 validate를 진행 할 때 order__createdAt는 @IsOptional() 데코레이터로 client가 보낼 수 도 있고 안 보낼 수 도 있는데 해당 프로퍼티가 없을 경우에 default값으로 해당 프로퍼티에 'ASC'값이 할당 되게 선언 했지만 실제로 http request 요청을 보내게 되면 해당 값이 undefined로 나오는걸 알 수 있다. 그 이유는 위의 중요 핵심 옵션인
   *transform: true*  옵션을 주어서 프로퍼티에 값이 없을 경우 기본값을 넣은채로 인스턴스를 생성하도록 허가를 해주어야 한다!



- [1]  2. transformOptions:{ enableImplicitConversion : boolean}

```typescript
// main.ts
app.useGlobalPipes(

new ValidationPipe({
	transform: true, 
	transformOptions: {
		enableImplicitConversion: true,
	},
}
```

- enableImplicitConversion : 임의로 변환하는것을 허가한다.

> transform: true 옵션과 함께 사용 되고, transform이 될때 class-validator 기반으로 만약 프로퍼티에 @IsNumber()라고 되어 있으면 숫자로 변환 되는게 정상이구나 라는걸 자동으로 인지를 한 다음에 자동으로 숫자로 변환 시킨 후 @IsNumber 데코레이터를 통과를 시킨다.
> 즉, [[@Type]] 데코레이터를 안쓰고도 데이터 형변환을 자동으로 해준다.
> 문제가 있다. 해당 옵션을 키게 되면 엄격하게 타입 검사를 안한다. 개인적으로는 *해당 옵션은 false로 하는게 맞는것 같다. 왜냐하면 해당 옵션을 true로 했을때 @Type 데코레이터를 쓰지 않아도 되는 장점은 있지만 중첩 객체를 validation 하지 못한다*  

- [2]  3. whitelist

- If set to true validator will strip validated object of any properties that do not have any decorators
- 만약에 이 옵션이 true가 되었다면 validator가 적용되지 않은 모든 프로퍼티들을 삭제 할 것이다

```typescript
// main.ts
app.useGlobalPipes(

new ValidationPipe({
	transform: true, 
	transformOptions: {
		enableImplicitConversion: true,
	},
	whitelist: true,
}
```


```typescript
import { Type } from 'class-transformer';
import { IsIn, IsNumber, IsOptional, IsString } from 'class-validator';
import { BasePaginationDto } from 'src/common/dto/base-pagination.dto';

export class PaginatePostDto extends BasePaginationDto {

	@IsNumber()
	@IsOptional()
	where__likeCount__more_than: number;
	
	  
	
	//@IsString()
	//@IsOptional()
	//where__title__i_like: string;

}
```

> 해당 코드에서 만약 client가 서버 개발자쪽에서 where__title__i_like를 주석 처리 했다고 했는데 까먹고 그대로 보내게 된다면 where__title__i_like가 존재 했을때 돌아가는 로직이 실행 되는 불상사를 겪게 될것이다.
> 그때 사용하는게 바로 *whitelist : true*  설정이다. 즉, validator가 적용되지 않은 프로퍼티면 삭제 한다.


- [3]  4. forbidNonWhitelisted

- *whitelist* 옵션과 함께 사용 된다.
- validator가 되지 않은 프로퍼티를 삭제하는게 whitelist인데 그냥 삭제만 할 뿐이고 에러를 던지지 않는다.
- 즉, validator가 되어야 할 프로퍼티가 없는데 client가 보내면 에러를 던진다.
- true를 하게 된다면 위의 validator가 적용되지 않은 프로퍼티를 삭제 하고 에러를 던진다 - 왜냐하면 프론트엔드 개발자가 요청을 보내는데 실수 했을 수 도 있기 때문에

```typescript
// main.ts
app.useGlobalPipes(

new ValidationPipe({
	transform: true, 
	transformOptions: {
		enableImplicitConversion: true,
	},
	whitelist: true,
	forbidNonWhitelisted: true,
}

/*
* [response] - 400 Bad Request
* {
*   "statusCode" : 400,
*   "message" : [
*     "property where__title__i_like should not exist"
*   ],
*   "error" : "Bad Request"
* }
*
*/
```



- [4]  5. stopAtFirstError

- class-validator에서 에러 메세지가 2개일때 하나만 출력되게 해주는 옵션

```typescript
{
    "statusCode": 400,
    "message": [
        "username must be a string",
        "username should not be empty"
    ],
    "error": "Bad Request"
}
```

```javascript
// main.ts
app.useGlobalPipes(

new ValidationPipe({
	transform: true, 
	transformOptions: {
		enableImplicitConversion: true,
	},
	whitelist: true,
	forbidNonWhitelisted: true,
	stopAtFirstError: true,
}
```

```javascript
{
    "statusCode": 400,
    "message": [
        "username must be a string"
    ],
    "error": "Bad Request"
}
```








