
# ValidationOptions

```typescript
export interface ValidationOptions {

/**
* Specifies if validated value is an array and each of its items must be validated.
*/
each?: boolean;

/**
* Error message to be used on validation fail.
* Message can be either string or a function that returns a string.
*/
message?: string | ((validationArguments: ValidationArguments) => string);

/**
* Validation groups used for this validation.
*/
groups?: string[];

/**
* Indicates if validation must be performed always, no matter of validation groups used.
*/
always?: boolean;

context?: any;

}
```

> class-validator의 거의 모든 내장 decorator에 있는 ValdationOptions은 위와 같다.

## 🦐 each
```typescript
each?: boolean;
```

- 배열을 검증한다.

```typescript

@IsOptional()
@IsString({
	each: true,
})
images?: string[] = [];

```

- images가 배열로 ['abc.jpg', 'def.png'] 이런식으로 들어올때 validator를 하려면 each 옵션을 true로 준다.
## 🦀 message
```typescript
message?: string | ((validationArguments: ValidationArguments) => string);
```
- 메세지 타입은 이러한 형식이고 여기엔 string 형식과 콜백 함수가 올 수 있다.


1. 해당 옵션을 사용하면 에러 메세지를 커스텀 할 수 있다.
```typescript
@IsString({
	message: 'content는 string 타입을 입력 해줘야합니다'
})
```

2. 또는 ((validationArguments: ValidationArguments) => string)
```typescript
//validation-message/length-validation-message.ts
import { ValidationArguments } from 'class-validator';
export const lengthValidationMessage = (args: ValidationArguments) => {}
```
위와 같이 일반화 하며 생성하여 해당 message 프로퍼티에 넣을 수 있다.

### ValidationArguments의 프로퍼티들
```typescript
export interface ValidationArguments {
/**
* Validating value.
*/
value: any;

/**
* Constraints set by this validation type.
*/
constraints: any[];

/**
* Name of the target that is being validated.
*/
targetName: string;

/**
* Object that is being validated.
*/
object: object;

/**
* Name of the object's property being validated.
*/
property: string;

}
```

> 1) value - 검증 되고 있는 값 (입력된 값)
> 2) constraints -> 파라미터에 입력된 제한 사항들 (제약 사항을 넣을 수 있는것만)
> > args.constraints[0] -> 1
> > args.constraints[1] -> 20
> > Ex) @Length(1, 20,{ message: lengthValidationMessage }) 이런식으로 일반화하여 사용 했다고 
> > 가정하면 Length 데코레이터 같은 경우의 앞의 인자값 args 조건 1이 args.constraints[0]이 되고 뒤의 인자값이 args.constraints[1]가 되는것이다.
> 
> 3) targetName -> 검증하고 있는 클래스의 이름
> 4) object -> 검증하고 있는 객체 (거의 쓸일이 없음)
> 5) property -> 검증 되고 있는 객체의 프로퍼티 이름
> > Ex) @Length(1, 20, {
> > 	message: lengthValidationMessage
> > })
> > nickname: string;
> > 이렇게 사용 하고 있다면 검증 되고 있는 객체의 프로퍼티 이름은 *nickname* 일것이다.

#### @Length데코레이터의 message 일반화하여 사용하기
```typescript
export const lengthValidationMessage = (args: ValidationArguments) => {
	if (args.constraints.length === 2) {
		return `${args.property}은 최대${args.constraints[0]}~${args.constraints[1]}글자를 입력 해주세요!`;
	} else {
	return `${args.property}는 최소 ${args.constraints[0]} 글자를 입력 해주세요!`;
	}
}
```
> args.constraints.length가 2라는 뜻은 min length와 max length 인자값 2개를 다 받았다는뜻
> 그게 아니라는뜻은 min length만 받았다는 뜻

- 적용하기
```typescript
@Length(1, 20, {
message: lengthValidationMessage
 })
 nickname: string;
```

#### @IsString데코레이터 message 일반화하여 사용하기
```typescript
import { ValidationArguments } from 'class-validator';

export const stringValidationMessage = (args:ValidationArguments) => }{
	return `${args.property}에 String을 입력 해주세요!`;
}
```

- 활용
```typescript
@IsString({
	message: stringValidationMessage
})
title: string;

/**
결과)
title에 String을 입력 해주세요!
*/
```

