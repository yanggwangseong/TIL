- @nestjs/common 모듈에 있다.

> serialization -> 직렬화 -> 현재 시스템에서 사용되는 (NestJS) 데이터의 구조를 다른 시스템에서도 쉽게 사                                            용 할 수 있는 포맷으로 변환 
> 					-> class의 object에서 JSON 포맷으로 변환
> deserialization -> 역직렬화

- 즉, 보통 NestJs와 TypeOrm을 사용 할때 보통 entity의 인스턴스 즉 UserModel: { } 이런식의 object객체를 반환 해주는데 그걸 JSON 포맷으로 변환 해주는 역할을 한다. 보통 @Exclude와 @Expose와 함께 사용 된다.


```typescript
// app.module.ts
@Module({


	providers:[
		{
			provider: APP_INTERCEPTOR,
			useClass: ClassSerializerInterceptor
		}
	]
})
```
- 이런식으로 app.module.ts에 APP 전역 인터셉터로 커스텀 프로바이더로 등록하여 모든 요청과 응답에 다 적용 되게 해준다.

> provider에 글로벌로 등록 할 수 있는 커스텀 프로바이더는 INTERCEPTOR 이외에도
> *APP_FILTER* , *APP_GUARD* , *APP_PIPE* 가 있다.

