> `@IsString` `@IsNotEmpty` `@IsNumber` 등 validation decorator에서 메세지가 기본 default가 있지만 해당 메세지는 영어로 되어 있기 때문에 서비스에 맞게 error mesage를 주는게 바람직하다.


# ValidationOptions의 message 프로퍼티 커스텀
```javascript
// IsString 데코레이터 스펙
export declare function IsString(validationOptions?: ValidationOptions): PropertyDecorator;

export interface ValidationOptions {
	...
	message?: string | ((validationArguments: ValidationArguments) => string);
	...
}

export interface ValidationArguments {

	value: any;
	constraints: any[];
	targetName: string;
	object: object;
	property: string;

}
```

> 다음과 같은 스팩을 가지고 있기 떄문에

```typescript
export const COMMENT_IS_STRING_MESSAGE = 'comment는 string만 올 수 있습니다!' as const;


...

@IsString({
	message: COMMENT_IS_STRING_MESSAGE,
})
commentContents!: string;
```

> 이런식으로 작성 할 수 있는데 이렇게 되었을때 const 폴더 같은걸 만들어서 모든 validation 에러메세지를 다 기입 해주어야 하는 번거로움이 생긴다. 그래서 validation opions type 스팩을 보면 콜백 함수도 받는다!.
> `((validationArguments: ValidationArguments) => string)` 


# ValidationOptions의 message 일반화
```typescript
// string-validation-message.ts
import { ValidationArguments } from 'class-validator';

export const stringValidationMessage = (args: ValidationArguments) => {
	return `${args.property}는 string만 올 수 있습니다!`;
};
```

> property 라는 프로퍼티는 commentContents의 이름을 가져올 수 있다. 그래서 콜백 함수 하나를 만들어서 범용성있게 사용 할 수 있음!



