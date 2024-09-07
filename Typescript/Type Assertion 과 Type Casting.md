
# Type Assertion (타입 표명)과 Type Casting(타입캐스팅)

## Type Assertion (타입 표명)

- 타입 표명은 **순수하게 컴파일 시간**에만 존재하는 기능으로, TypeScript 컴파일러에게 개발자가 해당 변수의 타입을 정확히 알고 있음을 알리는 **힌트**를 제공하는 수단
- 실제로는 타입 변경이 일어나지 않으며, 타입 검사와 관련된 내용이 컴파일러 수준에서 처리된다. 즉, **런타임에서 어떤 동작도 발생하지 않는다.** 
- 주로 컴파일러가 충분히 타입을 추론하지 못하거나, 개발자가 변수를 보다 명확하게 지정하고 싶을 때 사용한다. (ex : `as` )

```typescript title="Type Assertion"
let value: any = "Hello, World!";
let strLength: number = (value as string).length;  // 컴파일러에게 value가 string이라고 알림
```

> `value`는 `any` 타입으로 선언되었지만, `as string`으로 타입을 표명(Type Assertion)하여 컴파일러가 이 변수가 `string` 타입임을 알 수 있게 한다.

*Type Casting* : 실행시간에 어떤 동작이 일어날 것임을 내포한다.

## Type Casting (타입 캐스팅)
- 타입 캐스팅은 **런타임 실행 시간**에 실제로 타입을 변환하는 과정
- TypeScript 자체에서 제공되지는 않지만, 자바스크립트의 타입 변환 기능에 의존하는 부분이 있다.
- 타입 캐스팅은 주로 JavaScript에서의 동적 타입 변환과 관련이 있다.
- 타입 캐스팅은 **실제 런타임 동작**을 기반으로 값을 변환할 때 사용. (ex: `number`를 `string`으로 변환)

```typescript
let value: any = "123";
let numValue: number = Number(value);  // 실행 시간에 "123"을 숫자 123으로 변환
```

> `Number(value)`는 자바스크립트에서 문자열을 숫자로 변환하는 실제로 *런타임에서 수행* 하는 변환이다.
## 주요 차이점

| 항목          | Type Assertion (타입 표명) | Type Casting (타입 캐스팅) |
| ----------- | ---------------------- | --------------------- |
| 적용 시점       | 컴파일 시점                 | 실행 시점                 |
| 주 목적        | 컴파일러에게 타입 힌트를 제공       | 실제로 값을 다른 타입으로 변환     |
| 실제 변환 발생 여부 | 변환은 없고 타입 검사만 발생       | 런타임에 타입 변환 발생         |
| 예시          | `(value as string)`    | `Number(value)`       |
