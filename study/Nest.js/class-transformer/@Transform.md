
# @Transform
- [[@Expose]] 는 **없는 프로퍼티를 만들어 내는것** 이라고 하면, *@Transform* 는 **있는 프로퍼티를 원하는 형태로 변환**  해주는 역할을 한다.
- 당연히 [[ClassSerializerInterceptor]] 가 있어야 한다.

```typescript
export declare function Transform(transformFn: (params: TransformFnParams) => any, options?: TransformOptions): PropertyDecorator;

export interface TransformFnParams {

	value: any;
	key: string;
	obj: any;
	type: TransformationType;
	options: ClassTransformOptions;

}

export interface TransformOptions {

/**
* First version where this property should be exposed.
*
* Example:
* ```ts
* instanceToPlain(payload, { version: 1.0 });
* ```
*/
since?: number;

/**
* Last version where this property should be exposed.
*
* Example:
* ```ts
* instanceToPlain(payload, { version: 1.0 });
* ```
*/
until?: number;

/**
* List of transformation groups this property belongs to. When set,
* the property will be exposed only when transform is called with
* one of the groups specified.
*
* Example:
* ```ts
* instanceToPlain(payload, { groups: ['user'] });
* ```
*/
groups?: string[];

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

- 이미지 프로퍼티가 호출될때 이미지 url을 자동으로 생성 해주고 싶을 때
```typescript
import { Transform } from 'class-transformer';

export enum ImageModelType {
	POST_IMAGE,
}

@Entity()
export class ImageModel extends BaseModel {

...
	@Column({
		enum: ImageModelType,
	})
	@IsEnum(ImageModelType)
	@IsString()
	type: ImageModelType;
	
	@Column()
	@IsString()
	@Transform(({ value, obj }) => {
		//obj 인스턴스화 되었을때 현재 객체를 가져올 수 있다
	if (obj.type === ImageModelType.POST_IMAGE) {
		return `/${join(POST_PUBLIC_IMAGE_PATH, value)}`;
	} else {
		return value;
	}
	path: string;

...

}
```
- value : 프로퍼티에 입력된 실제 값 value
- obj: obj 인스턴스화 되었을때 현재 객체

> path에는 원래 abc.jpg라는 데이터가 들어가 있는데, *@Transform* 을 이용하여 ImageModelType이 POST_IMAGE일때 join을 이용하여 이미지 경로를 만들어서 리턴 해줄 수 있다.
> @Transform의 첫번째 파라미터가 그렇고 두번쨰 파라미터는 option에 관련한건데 해당 옵션도 @Expose와 @Exclude와 마찬가지로 *toClassOnly* 와 *toPlainOnly* 옵션이 있는걸 알 수 있다. 즉, request일때만 적용 하거나 response일때만 적용 가능하다.



