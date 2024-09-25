
```typescript
type result = expect<-1_000_000n>;
type result2 = expect<9_999n>;
type expect<T extends number | string | bigint> = `${T}`;

```

> bigint 타입을 백틱으로 감싸주면
>  `{ts icon title:"-1000000"}type result = expect<-1_000_000n>`  "-1000000" 로 추론 됨. 
>  `{ts icon title:"9999"}type result2 = expect<9_999n>`  "9999" 로 추론 됨. 

