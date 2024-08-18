
# 일급 함수
#Javascript/First-class-function-일급함수

- **함수를 언어에서 값으로 다룰 수 있다.** 
- 런타임에서 언제나 정의할 수 있고 언제나 들고 다닐 수 있고 인자로 넘길 수 있고 언제나 원할때 평가 할 수 있는 함수

```js title="함수를 값으로 다룰 수 있다"
const f1 = function(a) { return a * a }
console.log(f1); // [Function: f1]
```


- 함수를 인자로 받는 경우

```js title="함수가 함수를 인자로 받을 수 있다"
function f3(f){
	return f();
}

console.log (f3(function() { return 10;})); // 10
```

> f3에 익명 함수를 인자로 넘길 수 있다.

## 함수형 프로그래밍이란?
> 언제 평가해도 상관없는 순수 함수들을 많이 만들고 그 함수들을 들고 다니면서 필요한 시점에서 평가를 하면서 다양한 로직을 만드는것

```js title="add_maker 함수"
function add_maker(a){
	return function(b){
		return a + b;
	}
}

const add10 = add_maker(10);

console.log(add10(20)); // 30


const add5 = add_maker(5);
console.log(add5(10));// 15

const add15 = add_maker(15);
console.log(add15(10)); // 25
```

> add_maker에 10을 인자로 넘기게 되면
> 함수를 반환 받고 그 함수를 add10 에 담긴다.
> const add10 = function(b){
> 	return a + b;
> }
> **add10이라는 변수에 함수가 들어갈 수 있는 특징과 add_maker란 함수가 함수를 값으로 다뤄서 함수를 리턴할 수 있는 특징을 이용한 함수다** 
> *add_maker 함수에서 return 하는 익명함수가 클로저이기도 하다* 
> add_maker는 a라는 값을 받아서 그 a라는 값을 알고 있는 컨텍스트에서 function(b){}함수를 정의 하면서 리턴 했는데 해당 함수는 a라는 값을 기억하는 함수가 되면서 클로저가 된다.

- add_maker함수에서 a라는 값을 변경하는곳은 어디에도 없기 때문에 항상 동일한 결과를 리턴하는 순수 함수이다.


```js
function f4(f1, f2, f3){
	return f3(f1() + f2());
}
console.log(
	f4(
		function(){ return 2;},
		function(){ return 1;},
		function(a){ return a * a;}
	)
);

```

> function f4(f1, f2, f3){
> 	return f3( 2 + 1 );
> }
> 위의 코드와 동일 하다.

