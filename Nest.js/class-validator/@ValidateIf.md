
# @ValidateIf

```ts
export declare function ValidateIf(condition: (object: any, value: any) => boolean, validationOptions?: ValidationOptions): PropertyDecorator;
```

> 첫번째 인자값 
> object
> value
> 리턴값은 반드시 boolean
> 두번째 인자값
> `ValidationOptions` 

- `object` : Validation 대상 객체 request dto 객체에 접근 가능
- `vlaue` : 해당 데코레이터 대상 프로퍼티의 기본값 value를 가져온다.

```ts
export class ChatCreateReqDto {
	@ApiProperty({
		description: '채팅 참여자 ID 목록',
		example: ['123e4567-e89b-12d3-a456-426614174000'],
	})
	@IsUUID(4, { each: true, message: uuidValidationMessage })
	memberIds!: string[];

	@ApiProperty({
		description: '채팅 타입',
		example: 'GROUP',
	})
	@IsIn(ChatType, { message: isInValidationMessage })
	chatType!: Union<typeof ChatType>;

	@ApiProperty({
		description: '그룹 아이디',
		example: '123e4567-e89b-12d3-a456-426614174000',
	})
	@IsUUID(4, { message: uuidValidationMessage })
	@ValidateIf((o, value) => {
		console.log(o);
		console.log(value);
		return o.chatType === 'GROUP';
	})
	groupId?: string = '1';
}
```

> `o`  결과값
> `value` 결과값  '1'

```
ChatCreateReqDto {
	groupId: '1',
	memberIds: [
	'7dd8f87b-fe25-4a12-956a-9511efdfc3fb',
	'83491506-9047-4cfc-9dec-9f1e2016ae13',
	'31a3ff5a-3715-43ca-a419-2b949a0dee53',
	'410b7202-660a-4423-a6c3-6377857241cc'
	],
	chatType: 'DIRECT'
}
'1'
```

```ts title="사용 예제 chatType이 'GROUP' 일때 "
@ValidateIf((o) => o.chatType === 'DIRECT')
groupId?: string;
```

> 사용 예제 chatType이 'GROUP' 일때 해당 validation 데코레이터들을 실행 시킨다!
