
# 문제1
```typescript
interface Square {
  width: number
}
interface Rectangle extends Square {
  height: number
}
type Shape = Square | Rectangle

function calculateArea(shape: Shape) {
  if (shape instanceof Rectangle) {
    // ~~~~~~~~~ 'Rectangle' only refers to a type,
    //           but is being used as a value here
    return shape.width * shape.height
    //         ~~~~~~ Property 'height' does not exist
    //                on type 'Shape'
  } else {
    return shape.width * shape.width
  }
}
```

> 다음 코드에서 'Rectangle' only refers to a type 에러가 발생합니다. 해결 방법은 무엇입니까?


