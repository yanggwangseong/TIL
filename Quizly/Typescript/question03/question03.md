# 문제3

```ts
type LineStringNumber= "Name" | 110;
type LineString = "Name";
type LineNumber = 110;

type Line = LineString & LineNumber; // ?
type LineUnion = LineString | LineNumber; // ?
type LineThird = LineNumber & LineStringNumber // ?
  

interface Person {
	name: string;
}

interface Lifespan {
	birth: Date;
	death?: Date;
}

type PersonSpan = Person & Lifespan; // ?

type PersonUnion = keyof (Person | Lifespan); // ?
```

> Line, LineUnion, LineThird, PersonSpan, PersonUnion의 타입이 어떻게 추론 될까요?



