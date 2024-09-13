
# Type Assertion (타입 표명)
```typescript title="타입 표명에서 as를 사용"
interface Member {
  name: string;
  age: number;
}

interface Admin {
  name: string;
  role: string;
}

type MemberOrAdmin = Member | Admin;

const isAdmin = (person: MemberOrAdmin): person is Admin =>
  (person as Admin).role !== undefined;
```

> 해당 코드는 `Type Predicate` 를 사용하여 Admin 타입인지 체크하는 커스텀 가드 함수인데 항상 타입을 표명 할 때 as가 아니라 `as unknown as 타입` 이렇게 많이들 사용한다.
## `as 타입` 사용과 `as unknown as 타입` 사용의 차이
- *아래 예제를 실행해보기 전에 tsconfig 설정에서 strict 관련 옵션을 끄고 실행* 
- 공부를 위해 tsconfig 설정을 러프하게 바꿔줘야 한다. 다 끄고 공부용으로 해봐도 좋다.

```typescript
type BarType = {
    bar: string;
};

type FooType = {
    foo: string;
};

let bar: BarType;

const foo = bar as FooType; //❌ Conversion of type 'BarType' to type 'FooType' may be a mistake because neither type sufficiently overlaps with the other.
const foo2 = bar as unknown as FooType; //✅
```

- `BarType` 과 `FooType` 은 공통적인 속성이 없다. 그렇기 때문에 `as FooType` 을 사용해서 `Type Assertion` 할려고 하면 에러가 발생 한다.
- TypeScript에서 변환할 수 없다고 생각하는 유형 간에 변환하기 위해 `as unknown as` 를 사용한다.

```typescript
type User = {
    name: string;
    friends: User[];
    login: () => void;
}


type Admin = {
    name: string;
    login: () => void;
    updateUser: () => void;
}

let myAdmin: Admin;

function greet(user: User) {
    console.log(`Hi ${user.name}`)
}

// greet(myAdmin as User) // ❌ Conversion of type

greet(myAdmin as unknown as User) //✅
```

- 두 타입간의 공통속성 `name` 이 있을때도 `as unknown as ` 를 사용하여 오류를 해결 할 수 있다.


## 결론
- 즉, `unknown` 을 사용하여 타입을 초기화 한후 다시 as를 통해서 타입을 assertion 한다고 생각하면 되겠다.
- 용도로는 위의 예제처럼 as를 통해서 `assertion` 할 수 없는 경우에 사용 된다.
- 공유 프로퍼티가 없는 유형으로 변수를 타입 팅하면 TypeScript는 새 프로퍼티를 생성하지 않으며, 런타임 전에 액세스하려고 할 때 오류가 표시되지 않을 뿐이다.
- 변수를 공통 속성이 있는 유형으로 타입 캐스팅하면 적어도 해당 속성이 존재한다는 것을 알 수 있다.
