
# @IsOptional
- 프로퍼티가 만약에 optional일때 있을때만 검증 하게 해주는 데코레이터

```typescript
import { IsOptional } from 'class-validator';

export class CreatePostDto extends PickType(PostsModel, [
	'title',
	'content',
	] as const) {
	@IsOptional()
	@IsString()
	images: string;
}
```

> 예를들어, post를 작성하는 dto를 만든 뒤 images 프로퍼티가 있을 때도 있고 없을 때도 있을때 *@IsOptional()* 데코레이터가 없다면 images 프로퍼티가 없을때마다 에러를 던져준다. 해당 프로퍼티를 필수값이냐 아니냐를 구분 지어준다.



