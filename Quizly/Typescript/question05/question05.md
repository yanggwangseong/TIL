
# 문제5
> 다음은 함수 선언식과 함수 표현식으로 타입을 적용한 예시이다. 둘중 어떤 방식이 좋을까요?


- 함수 선언식

```ts
async function checkedFetch(input: RequestInfo, init?: RequestInit) {
  const response = await fetch(input, init)
  if (!response.ok) {
    // 비동기 함수 내에서는 거절된 프로미스로 변환합니다.
    throw new Error('Request failed: ' + response.status)
  }
  return response
}

```


- 함수 표현식

```ts
const checkedFetch: typeof fetch = async (input, init) => {
  const response = await fetch(input, init)
  if (!response.ok) {
    throw new Error('Request failed: ' + response.status)
  }
  return response
}
```
