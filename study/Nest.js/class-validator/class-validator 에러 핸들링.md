# useGlobalPipes 와 ValidationPipe

```javascript
@Column({ type: 'text', nullable: false })
@ApiProperty({
	nullable: false,
})
@IsNotEmpty()
@IsString()
contents!: string;

  

@IsBoolean()
@Column({ type: 'boolean', nullable: false, default: true })
@ApiProperty({
	nullable: false,
})
isPublic!: boolean;
```

> @IsString과 @IsBoolean은 request body dto에서 string과 boolean값이 아니면 validator에서 에러를 던져준다. 하지만 *useGlobalFilters* 를 이용하여 에러를 핸들링 하고 있다면, 아마도 http 커스텀 에러 결과와 class-validator의 결과값 형식이 다르다는것을 알 수 있다. 해당 부분은 일관성 있는 에러를 던져주기에 적합하지 않은 방법이다. 아래와 같은 코드를 이용하여 일관성 있는 에러를 던져주게 수정.

```typescript
// main.ts

...


// global pipes
app.useGlobalPipes(
new ValidationPipe({
	transform: true,
	stopAtFirstError: true,
	exceptionFactory: (validationErrors: ValidationError[] = []) => {
	
		const { constraints } = validationErrors[0];
		
		if (!constraints) return validationErrors;
		
		const [firstKey] = Object.keys(constraints);
		
		return BadRequestServiceException(constraints[firstKey]);
	},
	}),
);

...


```


- 변경전

```typescript
{  
	"message": "Validation failed (uuid is expected)",  
	"error": "Bad Request",  
	"statusCode": 400  
}

```

- 변경후

```typescript
{
	"success": false,
	"timestamp": "2024-03-08T03:56:43.009Z",
	"status": 400,
	"message": "Validation failed (uuid is expected)",
	"path": "/api/feeds"
}

```

- http error

```typescript
{
	"success": false,
	"timestamp": "2024-03-08T04:27:10.269Z",
	"status": 404,
	"message": "피드를 찾을 수 없습니다",
	"path": "/api/feeds/3d082f5f-5d9e-4a99-9db0-74eb5858aafb"
}
```






