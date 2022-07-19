---
date: '2022-06-07'
title: 'Javascript Deep Dive'
categories: ['TIL', 'Deep Dive']
summary: '45. Promise'
thumbnail: './deep_dive.png'
---

## 프로미스

- 자바스크립트는 비동기 처리를 위한 하나의 패턴으로 콜백 함수를 사용
- 하지만 전통적인 콜백 패턴은 콜백 헬로 인해 가독성이 나쁘고 비동기 처리 중 발생한 에러 처리가 곤란
- 또한 여러개의 비동기 처리를 한번에 처리하는 데에도 한계가 있음
- ES6에서는 비동기 처리를 위한 또 다른 패턴으로 Promise 를 도입
- Promise 는 전통적인 콜백 패턴이 가진 단점을 보완하며 비동기 처리 시점을 명확하게 표현할 수 있다는 장점

<br>

### 비동기 처리를 위한 콜백 패턴의 단점

#### `콜백 헬`

- 예제) GET 요청을 위한 함수

  ```js
  const get = url => {
    const xhr = new XMLHttpRequest()
    xhr.open('GET', url)
    xhr.send()

    xhr.onload = () => {
      if (xhr.status === 200) {
        // 서버의 응답을 콘솔에 출력함
        console.log(JSON.parse(xhr.response))
      } else {
        console.error(`${xhr.status} ${xhr.statusText}`)
      }
    }
  }

  // id가 1인 post를 취득
  get('https://jsonplaceholder.typicode.com/posts/1')
  ```

- 위 예제의 get 함수는 서버의 응답 결과를 콘솔에 출력함
- get 함수가 서버의 응답 결과를 반환하게 하려면 어떻게 해야 할까?
- get 함수는 비동기 함수임
- `비동기 함수` 란 함수 내부에 비동기로 동작하는 코드를 포함한 함수임
- 비동기 함수를 호출하면 함수 내부의 비동기로 동작하는 코드가 완료되지 않았다 해도 기다리지 않고 즉시 종료됨
- 즉, 비동기 함수 내부의 비동기로 동작하는 코드는 비동기 함수가 종료된 이후에 완료됨
- 따라서 비동기 함수 내부의 비동기로 동작하는 코드에서 처리 결과를 외부로 반환하거나 상위 스코프의 변수에 할당하면 기대한 대로 동작하지 않음

- 예제) 비동기 함수인 setTimeout 함수

  ```js
  let g = 0

  // 비동기 함수인 setTimeout 함수는 콜백 함수의 처리 결과를
  // 외부로 반환하거나 상위 스코프의 변수에 할당하지 못함

  setTimeout(() => {
    g = 100
  }, 0)
  console.log(g) // 0
  ```

  - setTimeout 함수가 비동기 함수인 이유는 콜백 함수의 호출이 비동기로 작동하기 때문
  - setTimeout 함수의 콜백 함수는 setTimeout 함수가 종료된 이후에 호출됨
  - setTimeout 함수의 콜백 함수에 상위 스코프의 변수에 값을 할당하면, <br>
    setTimeout 함수는 생성된 타이머를 식별할 수 있는 고유한 타이머 id를 반환하므로 콜백 함수에서 값을 반환하는 것은 무의미함

<br>

- 이처럼 비동기 함수는 비동기 처리 결과를 외부에 반환할 수 없고, 상위 스코프의 변수에 할당할 수도 없음
- 따라서 비동기 함수의 처리 결과(서버의 응답 등)에 대한 후속 처리는 비동기 함수 내부에서 수행해야 함
- 이때 비동기 함수를 범용적으로 사용하기 위해 비동기 함수에 비동기 처리 결과에 대한 후속 처리를 수행하는 콜백 함수를 전달하는 것이 일반적
- 필요에 따라 비동기 처리가 성공하면 호출될 콜백 함수와 실패하면 호출될 콜백 함수를 전달할 수 있음

<br>

```js
// GET 요청을 위한 비동기 함수
const get = (url, callback) => {
  const xhr = new XMLHttpRequest()
  xhr.open('GET', url)
  xhr.send()

  xhr.onload = () => {
    if (xhr.status === 200) {
      // 서버의 응답을 콜백 함수에 인수로 전달하면서 호출하여 응답에 대한 후속 처리를 함
      callback(JSON.parse(xhr.response))
    } else {
      console.error(`${xhr.status} ${xhr.statusText}`)
    }
  }
}

const url = 'https://jsonplaceholder.typicode.com'

// id가 1인 post의 userId를 취득
get(`${url}/posts/1`, ({ userId }) => {
  console.log(userId) // 1
  // post의 userId를 사용하여 user 정보 취득
  get(`${url}/users/${userId}`, userInfo => {
    console.log(userInfo)
  })
})
```

- 위 예제를 보면 GET 요청을 통해 서버로부터 응답을 취득하고, 이 데이터를 사용하여 또다시 GET 요청을 함
  ```js
  get('/step1', a => {
    get(`/step2/${a}`, b => {
      get(`/step3/${b}`, c => {
        get(`/step4/${c}`, d => {
          console.log(d)
        })
      })
    })
  })
  ```
- 이처럼 콜백 함수를 통해 비동기 처리 결과에 대한 후속 처리를 수행하는 비동기 함수가, 비동기 처리 결과를 가지고 또다시 비동기 함수를 호출해야 한다면 콜백 함수 호출이 중첩되어 복잡도가 높아지는 현상이 발생하는데
- 이를 `콜백 헬` 이라고 함
- 콜백 헬은 가독성을 나쁘게 하며 실수를 유발하는 원인이 됨

<br>

### 에러 처리의 한계

- 비동기 처리를 위한 콜백 패턴의 문제점 중 가장 심각한 것은 에러 처리가 곤란하다는 것

  ```js
  try {
    setTimeout(() => {
      throw new Error('Error!')
    }, 1000)
  } catch (e) {
    // 에러를 캐치하지 못함
    console.error('캐치한 에러', e)
  }
  ```

  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0vpcX%2Fbtrdqga8Eeb%2FOHVaVt3lye5RFNZdflpKxk%2Fimg.png" style="width: 748px" />

- 비동기 함수인 setTimeout 이 호출되면 setTimeout 함수의 실행 컨텍스트가 생성되어 콜 스택에 푸쉬되어 실행됨
- setTimeout 은 비동기 함수이므로 콜백 함수가 호출되는 것을 기다리지 않고 즉시 종료되어 콜 스택에서 제거됨
- 이후 타이머가 만료되면 setTimeout 함수의 콜백 함수는 태스크 큐로 푸시되고 콜 스택이 비어졌을 때 이벤트 루프에 의해 콜 스택으로 푸시되어 실행됨
- setTimeout 함수의 콜백 함수가 실행될 때는 이미 setTimeout 함수는 콜 스택에서 제거가 된 상태임
- 이것은 setTimeout 함수의 콜백 함수를 호출한 것이 setTimeout 함수가 아니라는 것을 의미함
- 에러는 호출자(caller) 방향으로 전파됨
- 즉, 콜 스택의 아래 방향으로 전파됨
- 위의 예제는 setTimeout 함수의 콜백 함수를 호출한 것이 setTimeout 이 아니기 때문에 setTimeout 함수의 콜백 함수가 발생시키는 에러는 catch 블록에서 캐치되지 않음

<br>

- 즉, 비동기 처리를 위한 콜백 패턴은 `콜백 헬` 이나 `에러 처리` 가 곤란하다는 문제점을 가짐
- 이를 극복하기 위해 `ES6` 에서 `프로미스` 가 도입됨

<br>

### 프로미스의 생성

- Promise 생성자 함수를 `new` 연산자와 함께 호출하면 프로미스(Promise 객체)를 생성함
- ES6 에서 도입된 Promise 는 ECMAScript 사양에 정의된 표준 빌트인 객체
- Promise 생성자 함수는 비동기 처리를 수행할 콜백 함수를 인수로 전달받는데 이 콜백 함수는 resolve 와 reject 함수를 인수로 전달 받음

  ```js
  // 프로미스 생성
  const promise = new Promise((resolve, reject) => {
    // Promise 함수의 콜백 함수 내부에서 비동기 처리를 수행함
    if(/* 비동기 처리 성공*/){
      resolve('result');
    } else { /* 실패 */
      reject('failure reason');
    }
  })
  ```

- Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 비동기 처리를 수행함
- 이때 비동기 처리가 성공하면 콜백 함수의 인수로 전달받은 resolve 함수, 실패하면 reject 함수를 호출

  ```js
  // GET 요청을 위한 비동기 함수
  const promiseGet = url => {
    return new Promise((resolve, reject) => {
      const xhr = new XMLHttpRequest()
      xhr.open('GET', url)
      xhr.send()

      xhr.onload = () => {
        if (xhr.status === 200) {
          // 성공하면 resolve 함수 호출
          resolve(JSON.parse(xhr.response))
        } else {
          // 에러 처리를 위해 reject 함수 호출
          reject(new Error(xhr.status))
        }
      }
    })
  }

  // promiseGet 함수는 프로미스를 반환
  promiseGet('https://jsonplaceholder.typicode.com/posts/1')
  ```

- 비동기 함수인 promiseGet 은 함수 내부에서 프로미스를 생성하고 반환
- 비동기 처리는 Promise 생성자 함수가 인수로 전달받은 콜백 함수 내부에서 수행
- 성공하면 비동기 처리 결과를 resolve 함수에 인수로 전달하며 호출, 실패하면 에러를 reject 함수에 인수로 전달

- 프로미스는 다음과 같이 현재 비동기 처리가 어떻게 진행되고 있는지 나타내는 상태 정보를 갖음
  <img src="https://velog.velcdn.com/images%2Fniyu%2Fpost%2F94082c0d-b5f2-41de-b1a8-dde17e47e0b4%2Fimage.png" style="width: 748px" />

- 생성된 직후의 프로미스는 기본적으로 pending 상태
- 비동기 처리 성공: resolve 함수 호출해 프로미스를 fullfilled 상태로 변경
- 비동기 처리 실패: reject 함수 호출해 프로미스를 rejected 상태로 변경
- 이처럼 프로미스의 상태는 resolve 또는 reject 함수 호출하는 것으로 결정됨
- 비동기 처리 성공하면 pending -> fulfilled 로 변화하고, 비동기 처리 결과인 1을 값으로 가짐

  <img src="https://velog.velcdn.com/images%2Fniyu%2Fpost%2F58a9919b-b31f-4e69-aecd-ac48c5abb92e%2Fimage.png" style="width: 748px" />

- 비동기 처리 실패하면 pending -> rejected 로 변화하고, 비동기 처리 결과인 Error 객체를 값으로 가짐
  <img src="https://velog.velcdn.com/images%2Fniyu%2Fpost%2Fb2758d9b-2068-4db3-ac60-fb394c60fcc4%2Fimage.png" style="width: 748px" />

- 즉, 프로미스는 비동기 처리 상태와 처리 결과를 관리하는 객체!

<br>

### 프로미스의 후속 처리 메서드

- 프로미스의 비동기 처리 상태가 변화하면 이에 따른 후속 처리를 해야 함
- 이를 위해 프로미스는 후속 메서드 `then`, `catch`, `finally` 를 제공함
- 프로미스의 비동기 처리 상태가 변화하면 후속 처리 메서드에 인수로 전달한 콜백 함수가 선택적으로 호출됨
- 이때 후속 처리 메서드의 콜백 함수에 프로미스 처리 결과가 인수로 전달됨
- 모든 후속 처리 메서드는 프로미스를 반환하며, 비동기로 동작됨

<br>

#### `Promise.prototype.then`

- then 메서드는 두 개의 콜백 함수를 인수로 전달 받음

  ```js
  // fulfilled
  new Promise(resolve => resolve('fulfilled')).then(
    v => console.log(v),
    e => console.log(e),
  ) // fulfilled

  // rejected
  new Promise((_, reject) => reject(new Error('rejected'))).then(
    v => console.log(v),
    e => console.log(e),
  ) // Error: rejected
  ```

- 첫 번째 콜백 함수는 `비동기 성공 처리 콜백 함수`<br>
  프로미스가 fulfilled 상태(resolve 함수가 호출된 상태)가 되면 호출됨. 이때 콜백 함수는 프로미스의 비동기 처리 결과를 인수로 전달 받음
- 두 번째 콜백 함수는 `비동기 실패 처리 콜백 함수`<br> 프로미스가 rejected 상태가 되면 호출됨. 이때 콜백 함수는 프로미스의 에러를 인수로 전달 받음

- then 메서드는 언제나 프로미스를 반환함
- 만약 then 메서드의 콜백 함수가 프로미스를 반환하면 그 프로미스 그대로 반환하고,
- 콜백 함수가 프로미스가 아닌 값을 반환하면 그 값을 암묵적으로 resolve 또는 reject 하여 프로미스를 생성해 반환함

<br>

#### `Promise.prototype.catch`

- catch 메서드는 한 개의 콜백 함수를 인수로 전달 받음
- catch 메서드의 콜백 함수는 프로미스가 `rejected` 상태인 경우만 호출됨
  ```js
  // rejected
  new Promise((_, reject) => reject(new Error('rejected'))).catch(e =>
    console.log(e),
  ) // Error: 'rejected'
  ```
- catch 메서드는 then 메서드와 마찬가지로 언제나 프로미스를 반환함

<br>

#### `Promise.prototype.finally`

- finally 메서드는 한 개의 콜백 함수를 인수로 전달 받음
- finally 메서드의 콜백 함수는 프로미스 성공 또는 실패와 상관없이 무조건 한 번 호출됨
- 프로미스의 상태와 상관없이 공통적으로 수행해야 할 처리 내용이 있을 때 유용
- finally 메서드도 then/catch 메서드와 마찬가지로 언제나 프로미스를 반환함

  ```js
  new Promise(() => {}).finally(() => console.log('finally')) // finally
  ```

  #### (참고) `try...catch...finally` 문

  - try...catch...finally 문은 에러 처리를 구현하는 방법
  - 먼저 try 코드 블록이 실행됨
  - 이때 try 코드 블록에 포함된 문 중 에러가 발생하면 해당 에러는 catch 문의 err 변수에 전달되고 catch 코드 블록이 실행됨
  - finally 코드 블록은 에러 발생과 관계없이 반드시 한 번 실행됨
  - try...catch...finally 문으로 에러를 처리하면 프로그램이 강제 종료되지 않음

<br>

- 프로미스로 구현한 비동기 함수 get 을 사용해 후속처리를 해보면 아래와 같음

  ```js
  // GET 요청을 위한 비동기 함수
  const promiseGet = url => {
    return new Promise((resolve, reject) => {
      const xhr = new XMLHttpRequest()
      xhr.open('GET', url)
      xhr.send()

      xhr.onload = () => {
        if (xhr.status === 200) {
          // 성공하면 resolve 함수 호출
          resolve(JSON.parse(xhr.response))
        } else {
          // 에러 처리를 위해 reject 함수 호출
          reject(new Error(xhr.status))
        }
      }
    })
  }

  // promiseGet 함수는 프로미스를 반환
  promiseGet('https://jsonplaceholder.typicode.com/posts/1')
    .then(res => console.log(res))
    .catch(err => console.log(err))
    .finally(() => console.log('Bye'))
  ```

<br>

### 프로미스의 에러 처리

- 프로미스는 에러를 문제없이 처리할 수 있음
- 비동기 처리에서 발생한 에러는 then 메서드의 두 번째 콜백 함수로 처리할 수 있음

  ```js
  // 위 예제와 이어짐
  const wrongUrl = 'https://xxx.com/1';

  // 잘못된 URL이 지정되었기 때문에 에러 발생
  promiseGet(wrongUrl).then(
    res => console.log(res)
    err => console.error(err)
  ); // Error: 404
  ```

- 단, then 메서드의 두 번째 콜백 함수는 첫 번째 콜백 함수에서 발생한 에러를 캐치하지 못하고 코드가 복잡해져 가독성이 떨어짐

<br>

- 또한 프로미스 후속 처리 메서드 catch 로도 처리 가능

  ```js
  const wrongUrl = 'https://xxx.com/1'

  // 잘못된 URL이 지정되었기 때문에 에러 발생
  promiseGet(wrongURl)
    .then(res => console.log(res))
    .catch(undefined, err => console.log(err)) // Error: 404
  ```

- catch 메서드를 호출하면 내부적으로 then(undefined, onRejected)을 호출하므로 위 예제처럼 처리됨(굳이 적지 않아도 됨)
- catch 메서드를 모든 then 메서드를 호출한 이후에 호출하면 비동기 처리에서 발생한 에러 뿐만 아니라 then 메서드 내부에서 발생한 에러까지 모두 캐치가 가능

  ```js
  promiseGet('https://github.com/bboyooning');
    .then(res => console.xxx(res))
    .catch(err => console.log(err)); // TypeError: console.xxx is not a function
  ```

- 또한 가독성이 좋고 명확하기 때문에 catch 메서드에서 에러 처리를 하는 것이 좋음

<br>

### 프로미스 체이닝

- 프로미스는 then, catch, finally 후속 처리 메서드를 통해 콜백 헬을 해결함

  ```js
  const url = 'https://github.com/bboyooning'

  // id가 1인 post의 userId를 취득
  promiseGet(`${url}/posts/1`)
    // 취득한 post의 userId로 user 정보 취득
    .then(({ userId }) => promiseGet(`${url}/users/${userId}`))
    .then(userInfo => console.log(userInfo))
    .catch(err => console.err(err))
  ```

- then -> then -> catch 순으로 후속 처리 메서드를 호출
- then, catch, finally 후속 처리 메서드는 언제나 프로미스를 반환하므로 연속적으로 호출이 가능
- 이를 `프로미스 체이닝` 이라고 함
- 후속 처리 메서드의 콜백 함수는 프로미스의 비동기 처리 상태가 변경되면 선택적으로 호출됨
- 후속 처리 메서드의 콜백 함수는 다음과 같이 인수를 전달받으며 호출됨
- 프로미스는 프로미스 체이닝을 통해 비동기 처리 결과를 전달받아 후속 처리를 하므로 콜백 헬이 발생하지 않음
- 다만 프로미스도 콜백 패턴을 사용하므로 콜백 함수를 사용하지 않는 것은 아님

<br>

- 콜백 패턴은 가독성이 좋지 않음
- 이는 `ES8` 에서 도입된 `async/await` 을 통해 해결이 가능
- async/await 를 사용하면 프로미스의 후속 처리 메서드 없이 마치 동기 처리처럼 프로미스가 처리 결과를 반환하도록 구현 가능

  ```js
  const url = 'https://github.com/bboyooning'

  ;(async () => {
    // id가 1인 post의 userId를 취득
    const { userId } = await promiseGet(`${url}/posts/1`)
    // 취득한 post의 userId로 user 정보 취득
    const userInfo = await promiseGet(`${url}/users/${userId}`)

    console.log(userInfo)
  })()
  ```

  <br>

### 프로미스의 정적 메서드

- 프로미스는 주로 생성자 함수로 사용되지만 함수도 객체이므로 메서드를 가질 수 있음
- 프로미스는 5가지 정적 메서드를 제공함

#### `Promise.resolve / Promise.reject`

- Promise.resolve 와 Promise.reject 메서드는 이미 존재하는 값을 래핑하여 프로미스를 생성하기 위해 사용됨

  ```js
  const resolvedPromise = Promise.resolve([1, 2, 3])
  // const resolvedPromise = new Promise(resolve => resolve([1,2,3]);
  resolvedPromise.then(console.log) // 1,2,3;

  const rejectedPromise = Promise.rejected(new Error('Error!'))
  // const rejectedPromise = new Promise((_, reject) => reject(new Error('Error!')));
  rejectedPromise.catch(console.log) // Error: Error!
  ```

#### `Promise.all`

- Promise.all 메서드는 여러 개의 비동기 처리를 모두 병렬 처리할 때 사용됨

  ```js
  const requestData1 = () =>
    new Promise(resolve => setTimeout(() => resolve(1), 3000))
  const requestData2 = () =>
    new Promise(resolve => setTimeout(() => resolve(2), 2000))
  const requestData3 = () =>
    new Promise(resolve => setTimeout(() => resolve(3), 1000))

  // 세 개의 비동기 처리를 병렬로 처리
  Promise.all([requestData1(), requestData2(), requestData3()])
    .then(console.log) // 약 3초 소요 -> [ 1, 2, 3 ]
    .catch(console.error)
  ```

- Promise.all 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달 받음
- 그리고 전달받은 모든 프로미스가 모두 fulfilled 상태가 되면 모든 처리 결과를 배열에 저장해 새로운 프로미스를 반환함
- 따라서 해당 메서드가 종료하는 데 걸리는 시간은 가장 늦게 fulfilled 상태가 되는 프로미스의 처리 시간보다 조금 더 김
- 이때 첫 번째 프로미스가 가장 나중에 fulfilled 상태가 되어도 Promise.all 메서드는 첫 번째 프로미스가 resolve 한 처리 결과부터 차례대로 배열에 저장해 그 배열을 resolve 하는 새로운 프로미스를 반환함
- 즉, 처리 순서가 보장됨
- 또한 Promise.all 메서드는 인수로 전달받은 배열의 프로미스가 하나라도 rejected 상태가 되면 나머지 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고 즉시 종료함
  ```js
  Promise.all([
    new Promise(resolve => setTimeout(() => resolve(1), 3000));
    new Promise(resolve => setTimeout(() => resolve(2), 2000));
    new Promise(resolve => setTimeout(() => resolve(3), 1000));
  ])
  .then(console.log) // 3
  .catch(console.error)
  ```

#### `Promise.race`

- Promise.all 메서드와 동일하게 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달 받음
- 하지만 모든 프로미스가 fulfilled 상태가 되는 것을 기다리지 않고, 가장 먼저 fulfilled 상태가 된 프로미스 처리 결과를 resolve 하는 새로운 프로미스를 반환
  ```js
  Promise.race([
    new Promise(resolve =>
      setTimeout(() => reject(new Error('Error 1')), 3000),
    ),
    new Promise(resolve =>
      setTimeout(() => reject(new Error('Error 2')), 2000),
    ),
    new Promise(resolve =>
      setTimeout(() => reject(new Error('Error 3')), 1000),
    ),
  ])
    .then(console.log) // 3
    .catch(console.error)
  ```
- 또한 전달된 프로미스가 하나라도 rejected 상태가 되면 에러를 reject하는 새로운 프로미스를 즉시 반환
  ```js
  Promise.race([
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Error 1')), 3000),
    ),
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Error 2')), 2000),
    ),
    new Promise((_, reject) =>
      setTimeout(() => reject(new Error('Error 3')), 1000),
    ),
  ])
    .then(console.log)
    .catch(console.error) // Error: Error 3
  ```

#### `Promise.allSettled`

- Promise.allSettled 메서드는 프로미스를 요소로 갖는 배열 등의 이터러블을 인수로 전달 받음
- 전달받은 프로미스가 모두 settled 상태(비동기 처리가 수행된 상태, fulfilled 또는 rejected)가 되면 처리 결과를 배열로 반환함
- 이때 반환한 배열에는 fulfilled 또는 rejected 상태와는 상관없이 메서드가 인수로 전달받은 모든 프로미스들의 처리 결과가 담겨 있음

<br>

### 마이크로태스크 큐

- 어떤 순서로 로그가 출력될까?

  ```js
  setTimeout(() => console.log(1), 0)

  Promise.resolve()
    .then(() => console.log(2))
    .then(() => console.log(3))
  ```

- 프로미스의 후속 처리 메서드도 비동기로 동작하므로 1 -> 2 -> 3 순으로 출력할 것 같지만,
- `2 -> 3 -> 1` 로 출력됨
- 그 이유는 프로미스의 후속 처리 메서드의 콜백 함수는 태스크 큐가 아니라 `마이크로태스크 큐` 에 저장되기 때문
- 마이크로태스크 큐는 태스크 큐와는 별도의 큐 임
- 마이크로태스크 큐에는 프로미스의 후속 처리 메서드의 콜백 함수가 일시 저장됨
- 그 외의 비동기 함수의 콜백 함수나 이벤트 핸들러는 태스크 큐에 일시 저장됨
- 콜백 함수나 이벤트 핸들러를 일시 저장한다는 점에서 태스크 큐와 동일하지만, 마이크로태스크 큐는 태스크 큐보다 우선순위가 높음
- 즉, 이벤트 루프는 콜 스택이 비면 먼저 마이크로태스크 큐에서 대기하고 있는 함수를 가져와 실행함
  <img src="https://uploads.disquscdn.com/images/9466d8aa53fc5b3e63a92858a94bb429df02bbd20012b738f0461343beaa6f90.gif?w=600&h=272" style="width: 748px" />

- 이후 마이크로태스크 큐가 비면 태스크 큐에서 대기하고 있는 함수를 가져와 실행함

<br>

### fetch

- fetch 함수는 XMLHttpRequest 객체와 마찬가지로 HTTP 요청 전송 기능을 제공하는 클라이언트 사이드 Web API
- 사용법이 간단하고 프로미스를 지원하기 때문에 비동기 처리를 위한 콜백 패턴의 단점에서 자유로움
- fetch 함수에는 HTTP 요청을 전송할 URL과 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달함
  ```js
  const promise = fetch(url, [, options])
  ```
- fetch 함수는 HTTP 응답을 나타내는 Response 객체를 래핑한 Promise 객체를 반환하므로 then 을 통해 프로미스가 resolve 한 Response 객체를 전달받을 수 있음
- Response 객체는 HTTP 응답을 나타내는 다양한 프로퍼티를 제공함

  ```js
  fetch('https://jsonplaceholder.typicode.com/todos/1')
    // response 는 HTTP 응답을 나타내는 Response 객체
    // json 메서드를 사용하여 Response 객체에서 HTTP 응답 몸체를 취득하여 역직렬화 함
    .then(response => response.json())
    // json은 역직렬화된 HTTP 응답 몸체
    .then(json => console.log(json))
  ```

- fetch 함수를 사용할 때는 에러 처리에 주의해야 함

  ```js
  const wrongUrl = 'https://jsonplaceholder.typicode.com/xxx/1'

  fetch(wrongUrl)
    .then(() => console.log('ok'))
    .catch(() => console.log('error'))
  ```

- 부적절한 URL 지정으로 에러가 발생하고, catch 메서드에 의해 'error' 가 출력될 것 같지만 'ok' 가 출력됨
- fetch 함수가 반환하는 프로미스는 기본적으로 404 나 500 과 같은 HTTP 에러가 발생해도 에러를 reject 하지 않고,
- 불리언 타입의 ok 상태를 false로 설정한 Response 객체를 resolve 함
- 오프라인 등 네트워크 장애나 CORS 에러에 의해 요청이 완료되지 못한 경우에만 프로미스를 reject 함
- 따라서 fetch 함수를 사용할 때는 아래와 같이 fetxh 함수가 반환한 프로미스가 resolve한 불리언 타입의 ok 상태를 확인해 명시적으로 에러 처리를 해야 함

  ```js
  const wrongUrl = 'https://jsonplaceholder.typicode.com/xxx/1'

  fetch(wrongUrl)
    // response 는 HTTP 응답을 나타내는 Response 객체
    .then(response => {
      if (!response.ok) throw new Error(response.statusText)
      return response.json()
    })
    .then(todo => console.log(todo))
    .catch(err => console.error(err))
  ```

- 참고로 `axios` 는 모든 HTTP 에러를 reject 하는 프로미스를 반환함
- 따라서 모든 에러를 catch 에서 처리할 수 있어 편리함
- 또한 axios는 인터셉터, 요청 설정 등 fetch 보다 다양한 기능을 지원함

- fetch 함수를 통해 HTTP 요청을 전송할 때, 함수에 첫 번째 인수로 HTTP 요청을 전송할 URL 과 두번째 인수로 HTTP 요청 메서드, HTTP 요청 헤더, 페이로드 등을 설정한 객체를 전달함

  ```js
  const request = {
    get(url) {
      return fetch(url)
    },
    post(url, payload) {
      return fetch(url, {
        method: 'POST',
        headers: { 'content-Type': 'application/json' },
        body: JSON.stringify(payload),
      })
    },
    patch(url, payload) {
      return fetch(url, {
        method: 'PATCH',
        headers: { 'content-Type': 'application/json' },
        body: JSON.stringify(payload),
      })
    },
    delete(url) {
      return fetch(url, { method: 'DELETE' })
    },
  }
  ```
