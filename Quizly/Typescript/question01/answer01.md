
### 타입스크립트는 런타임에는 타입 체크가 불가능합니다.

####  방법1 height 속성이 존재하는지 체크

```typescript
interface Square {
  width: number
}
interface Rectangle extends Square {
  height: number
}
type Shape = Square | Rectangle // 태그된 유니온(tagged union)
function calculateArea(shape: Shape) {
  if ('height' in shape) {
    shape // Type is Rectangle
    return shape.width * shape.height
  } else {
    shape // Type is Square
    return shape.width * shape.width
  }
}
```

####  방법2 런타임에 접근 가능한 타입 정보를 명시적으로 저장하는 '태그' 기법

```typescript
interface Square {
  kind: 'square'
  width: number
}
interface Rectangle {
  kind: 'rectangle'
  height: number
  width: number
}
type Shape = Square | Rectangle

function calculateArea(shape: Shape) {
  if (shape.kind === 'rectangle') {
    shape // Type is Rectangle
    return shape.width * shape.height
  } else {
    shape // Type is Square
    return shape.width * shape.width
  }
}
```


####  타입(런타임에서 접근 불가)과 값(런타임 접근 가능)을 둘 다 사용하는 기법 클래스

```typescript
class Square {
  constructor(public width: number) {}
}
class Rectangle extends Square {
  constructor(public width: number, public height: number) {
    super(width)
  }
}
type Shape = Square | Rectangle

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    shape // Type is Rectangle
    return shape.width * shape.height
  } else {
    shape // Type is Square
    return shape.width * shape.width // OK
  }
}
```


