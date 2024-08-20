```js
const users = [
	{ id: 1, name: 'ID', age: 36},
	{ id: 2, name: 'BJ', age: 32},
	{ id: 3, name: 'JM', age: 32},
	{ id: 4, name: 'PJ', age: 27},
	{ id: 5, name: 'HA', age: 25},
	{ id: 6, name: 'JE', age: 26},
	{ id: 7, name: 'JI', age: 31},
	{ id: 8, name: 'MP', age: 23}
];

```

# 명령어 코드

1. 30세 이상인 users를 거른다.

```js title="30세 이상인 users"
const users = [
	{ id: 1, name: 'ID', age: 36},
	{ id: 2, name: 'BJ', age: 32},
	{ id: 3, name: 'JM', age: 32},
	{ id: 4, name: 'PJ', age: 27},
	{ id: 5, name: 'HA', age: 25},
	{ id: 6, name: 'JE', age: 26},
	{ id: 7, name: 'JI', age: 31},
	{ id: 8, name: 'MP', age: 23}
];

const temp_users = [];
for (let i =0; i < users.length; i++){
	if(users[i].age >= 30){
		temp_users.push(users[i]);
	}
}
console.log(temp_users);
```

2. 30세 이상인 users의 names를 수집한다.

```js title="30세 이상인 users의 names를 수집"
const users = [
	{ id: 1, name: 'ID', age: 36},
	{ id: 2, name: 'BJ', age: 32},
	{ id: 3, name: 'JM', age: 32},
	{ id: 4, name: 'PJ', age: 27},
	{ id: 5, name: 'HA', age: 25},
	{ id: 6, name: 'JE', age: 26},
	{ id: 7, name: 'JI', age: 31},
	{ id: 8, name: 'MP', age: 23}
];

const names = [];
for (let i=0; i< users.length; i++){
	if(users[i].age >= 30){
		names.push(users[i].name);
	}
}
console.log(names);
```

3. 30세 미만인 users를 거른다.

```js title="30세 미만인 users"
const users = [
	{ id: 1, name: 'ID', age: 36},
	{ id: 2, name: 'BJ', age: 32},
	{ id: 3, name: 'JM', age: 32},
	{ id: 4, name: 'PJ', age: 27},
	{ id: 5, name: 'HA', age: 25},
	{ id: 6, name: 'JE', age: 26},
	{ id: 7, name: 'JI', age: 31},
	{ id: 8, name: 'MP', age: 23}
];

const temp_users = [];
for (let i =0; i < users.length; i++){
	if(users[i].age < 30){
		temp_users.push(users[i]);
	}
}
console.log(temp_users);
```

4. 30세 미만인 users의 ages를 수집한다.

```js title="30세 미만인 users의 ages를 수집"
const users = [
	{ id: 1, name: 'ID', age: 36},
	{ id: 2, name: 'BJ', age: 32},
	{ id: 3, name: 'JM', age: 32},
	{ id: 4, name: 'PJ', age: 27},
	{ id: 5, name: 'HA', age: 25},
	{ id: 6, name: 'JE', age: 26},
	{ id: 7, name: 'JI', age: 31},
	{ id: 8, name: 'MP', age: 23}
];

const ages = [];
for (let i =0; i < users.length; i++){
	if(users[i].age < 30){
		ages.push(users[i].age);
	}
}
console.log(ages);
```

# 2. filter, map으로 리팩토링
> 함수형 프로그래밍에서는 코드의 중복을 줄이는 아이디어를 다양하게 떠올릴 수 있다.

```js title="_filter 함수"
const users = [
	{ id: 1, name: 'ID', age: 36},
	{ id: 2, name: 'BJ', age: 32},
	{ id: 3, name: 'JM', age: 32},
	{ id: 4, name: 'PJ', age: 27},
	{ id: 5, name: 'HA', age: 25},
	{ id: 6, name: 'JE', age: 26},
	{ id: 7, name: 'JI', age: 31},
	{ id: 8, name: 'MP', age: 23}
];

function _filter(uesrs, predi){
	const new_list = [];
	for (let i =0; i < users.length; i++){
		if(predi(users[i])){
			new_list.push(users[i]);
		}
	}
	return new_list;
}
```

> 만약 filter 함수 내부에서 console.log를 사용하면  `{js icon title:"console.log는 부수효과를 일으킨다"}console.log("hello")` 그렇기 때문에 return으로 바꾼다.

- 15번째줄 어떤 조건일때 if문 안에 들어올것인가를 predi 함수에게 완전히 위임하게 된다.

- [0] 위에서 만든 `_filter` 함수

- filter 함수는 *응용형 함수* 이다.
	- 함수를 인자로 받아 원하는 시점에 평가를 하면서 `_filter` 함수 내에서 알고 있는 특정한 인자를 적용 해나가면서 로직을 완성해 나가는 프로그래밍을 응용형 프로그래밍 (적용형 프로그래밍) 그리고 `_filter` 같은 함수를 응용형 함수라고 한다.
- filter 함수는 *고차함수* 다. #Javascript/Higher-Order-Function_고차함수
	- *고차함수(Higher-Order-Function)*  란 : 함수를 리턴하거나 함수안에서 함수를 인자로 받아 함수를 실행하는 함수.

```js title="_filter 함수 적용"
const users = [
	{ id: 1, name: 'ID', age: 36},
	{ id: 2, name: 'BJ', age: 32},
	{ id: 3, name: 'JM', age: 32},
	{ id: 4, name: 'PJ', age: 27},
	{ id: 5, name: 'HA', age: 25},
	{ id: 6, name: 'JE', age: 26},
	{ id: 7, name: 'JI', age: 31},
	{ id: 8, name: 'MP', age: 23}
];

function _filter(uesrs, predi){
	const new_list = [];
	for (let i =0; i < users.length; i++){
		if(predi(users[i])){
			new_list.push(users[i]);
		}
	}
	return new_list;
}

console.log(
	_filter(users, function(users) { return users.age >= 30;})
);

console.log(
	_filter(users, function(users) { return users.age < 30;})
);
```

