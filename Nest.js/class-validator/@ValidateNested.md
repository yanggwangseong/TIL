
# @ValidateNested
- 프로퍼티가 중첩 객체를 Validate 해주는 데코레이터

```typescript
/**

* Objects / object arrays marked with this decorator will also be validated.

*/

export declare function ValidateNested(validationOptions?: ValidationOptions): PropertyDecorator;
```

> ValidationOptions 타입의 validationOptions가 올 수 있고, each 옵션을 주로 사용한다.

```typescript
import {
	IsEmail,
	IsNotEmpty,
	IsNotEmptyObject,
	IsNumberString,
	ValidateNested,
} from 'class-validator';

import { CreateAddressDto } from './create-address.dto';
import { Type } from 'class-transformer';

export class CreateCustomerDto {

	@IsEmail()
	email: string;
	
	@IsNumberString()
	@IsNotEmpty()
	id: number;
	
	@IsNotEmpty()
	name: string;
	
	@IsNotEmptyObject() // -> 빈객체 인지 validate
	@ValidateNested()
	@Type(() => CreateAddressDto)
	address: CreateAddressDto;

	//@ValidateNested({ each: true }) // -> 배열일때 each : true 옵션
	//@Type(() => CreateAddressDto)
	//address: CreateAddressDto[];
	
}
```

## @Type
 - @ValidateNested 사용할때 class-transformer의 @Type과  함께 써야 한다.

```typescript
export declare function Type(typeFunction?: (type?: TypeHelpOptions) => Function, options?: TypeOptions): PropertyDecorator;
```

> 꼭 기억하고 넘어가야 할 사실은 중첩객체를 validation 하기 위해서 @ValidateNested와 @Type 데코레이터를 사용 해야 하지만 (Request 요청시)
> 만약 중첩 객체의 @Transfomer 같은 작업 시에는 @Type 데코레이터만 사용 해도 된다 (예를 들어 Response시)

[공식 문서 class-transformer 사용시 중첩 객체](https://github.com/typestack/class-transformer?tab=readme-ov-file#working-with-nested-objects) 

