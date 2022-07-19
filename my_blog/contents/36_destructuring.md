---
date: '2022-06-04'
title: 'Javascript Deep Dive'
categories: ['TIL', 'Deep Dive']
summary: '36. 디스트럭처링 할당'
thumbnail: './deep_dive.png'
---

## 디스트럭처링 할당

- 구조 분해 할당은 구조화된 배열과 같은 이터러블 또는 객체를 비구조화 하여 1개 이상의 변수에 개별적으로 할당하는 것
- 배열과 같은 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용함

### 배열 디스트럭처링 할당

- ES6의 배열 구조 분해 할당은 배열의 각 요소를 배열로부터 추출하여 1개 이상의 변수에 할당
- 이때 배열 구조 분해 할당의 대상(할당문의 우변)은 이터러블이어야 하며, 배열의 순서대로 할당됨

```js
const arr = [1, 2, 3]
const [one, two, three] = arr

console.log(one, two, three) // 1 2 3
```

- 배열 구조분해 할당을 위해서는 할당 연산자 왼쪽에 값을 할당받을 변수를 선언해야 함
- 이때 변수를 배열 리터럴 형태로 선언함
- 우변에 이터러블을 할당하지 않으면 에러가 발생함

```js
const [x, y];
// SyntaxError: Missing initializer in destructuring declaration

const [a, b] = {};
// TypeError: {} is not iterable
```

- 배열 구조분해 할당의 기준은 배열의 인덱스. 즉, 순서대로 할당됨
- 이때 변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요는 없음

```js
const [a, b] = [1]
console.log(a, b) // 1 undefined

const [c, , e] = [1, 2, 3]
console.log(c, e) // 1 3
```

- 배열 구조분해 할당을 위한 변수에 기본값을 설정할 수 있음

```js
const [a, b, c = 3] = [1, 2]
console.log(a, b, c) // 1 2 3

// 기본값보다 할당된 값이 우선한다.
const [e, f = 10, g = 3] = [1, 2]
console.log(e, f, g) // 1 2 3
```

- 배열 구조분해 할당을 위한 변수에 Rest 파라미터와 유사하게 Rest 요소 `...` 를 사용할 수 있음
- Rest 요소는 Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 함

```js
//Rest 요소
const [x, ...y] = [1, 2, 3]
console.log(x, y) // 1 [ 2, 3 ]
```

<br>

### 객체 디스트럭처링 할당

- ES6의 객체 구조분해 할당은 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당함
- 이때 객체 구조분해 할당의 대상(우변)은 객체이어야 하며, 할당 기준은 `프로퍼티 키`
- 즉, 순서는 의미가 없고 선언된 변수 이름과 프로퍼티 키가 일치하면 할당됨

```js
const minsu = { firstName: '야끼니꾸', lastName: '민수' }
const { lastName, firstName } = minsu

console.log(firstName, lastName) // '야끼니꾸' '민수'
```

- 배열 구조분해 할당과 마찬가지로 객체 구조분해 할당을 위해서는 할당 연산자 왼쪽에 프로퍼티 값을 할당받을 변수를 선언해야 함
- 이때 변수를 객체 리터럴 형태로 선언함
- 우변에 객체 또는 객체로 평가될 수 있는 표현식을 할당하지 않으면 에러 발생

```js
const { lastName, firstName } = user
// 위와 아래는 동치
// 프로퍼티 축약 표현을 통해 선언한 것
const { lastName: lastName, firstName: firstName } = user
```

- 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음과 같이 변수를 선언함

```js
const user = { firstName: '재혁', lastName: '임' }
const { lastName: ln, firstName: fn } = user

console.log(ln, fn) // '임' '재혁'
```

- 객체 구조분해 할당을 위한 변수에 기본값을 설정할 수 있음

```js
const { firstName = 'boyoon', lastName } = { lastName: 'kim' }
console.log(firstName, lastName) // 'boyoon' 'kim'

const { firstName: fn = 'boyoon', lastName: ln } = { lastName: 'kim' }
console.log(fn, ln) // 'boyoon' 'kim'
```

- 객체 구조분해 할당은 객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용함

```js
const str = 'MinsuMansu'
const { length } = str
console.log(length) // 10

const todo = { id: 1, content: '딥다이브 개념정리', completed: 'true' }
// todo 객체로부터 id 프로퍼티만 추출함
const { id } = todo
console.log(id) // 1
```

- 객체 구조분해 할당은 객체를 인수로 전달받는 함수의 매개변수에도 사용할 수 있음

```js
function printTodo(todo) {
  console.log(
    `할일 ${todo.content}는 ${todo.completed ? '완료' : '비완료'} 상태 입니다.`,
  )
}

printTodo({ id: 1, content: '딥다이브 개념정리', completed: 'true' })
// '할일 딥다이브 개념정리는 완료 상태 입니다.'
```

- 위 예제에서 객체를 인수로 전달받는 매개변수 todo에 객체 구조분해 할당을 사용하면 좀 더 간단하고 가독성이 좋아짐

```js
function printTodo({ content, completed }) {
  console.log(`할일 ${content}는 ${completed ? '완료' : '비완료'} 상태 입니다.`)
}

printTodo({ id: 1, content: '딥다이브 개념정리', completed: 'true' })
// '할일 딥다이브 개념정리는 완료 상태 입니다.'
```

- 배열의 요소가 객체인 경우 배열 구조분해 할당과 객체 구조분해 할당을 혼용할 수 있음

```js
const todos = [
  { id: 1, content: '딥다이브', completed: true },
  { id: 2, content: '면접준비', completed: false },
  { id: 3, content: '프로젝트', completed: false },
]

const [, { id, content }] = todos
console.log(id) // 2
console.log(content) // '면접준비'
console.log(id, content) // 2 '면접준비'
```

- 중첩 객체의 경우는 다음과 같이 사용함

```js
const user = {
  name: '민수',
  address: {
    zipCode: '33333',
    city: 'Tokyo',
  },
}

// address 프로퍼티 키로 객체를 추출하고
// 이 객체의 city 프로퍼티 키로 값을 추출함
const {
  address: { city },
} = user
console.log(city) // 'Tokyo'
```

- 객체 구조분해 할당을 위한 변수에 Rest 파라미터나 Rest 요소와 유사하게 `Rest 프로퍼티 ...` 을 사용할 수 있음
- Rest 프로퍼티는 Rest 파라미터나 Rest 요소와 마찬가지로 반드시 마지막에 위치 <br>

```js
const { x, ...rest } = { x: 1, y: 2, z: 3 }
console.log(x, rest) // 1 { y: 2, z: 3 }
```
