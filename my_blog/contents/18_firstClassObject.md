---
date: '2022-06-03'
title: 'Javascript Deep Dive'
categories: ['TIL', 'Deep Dive']
summary: '18. 함수와 일급객체'
thumbnail: './deep_dive.png'
---

## 함수와 일급 객체

### 일급 객체

- 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체
- 보통 함수에 인자로 넘기기, 수정하기, 변수에 대입하기와 같은 연산을 지원할 때 `일급 객체` 라고 함
- 다음과 같은 조건을 만족하는 객체를 `일급 객체` 라 함
  - 무명의 리터럴로 생성할 수 있음. 즉, 런타임에 생성이 가능함
  - 변수나 자료구조(객체, 배열 등)에 저장할 수 있음
  - 함수의 매개변수에 전달할 수 있음
  - 함수의 반환값으로 사용할 수 있음
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8aa5ad8b-066e-4001-b4a3-692ff78a63b0/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220719%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220719T114350Z&X-Amz-Expires=86400&X-Amz-Signature=d0e206b8dcdbc48e48f095cc20d217d3bea7ae00c29a4c65f8f7f19babec5ac5&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" style="width: 748px">
- 자바스크립트의 함수는 위의 예제와 같이 조건을 모두 만족하므로 `일급 객체`
- 함수가 일급 객체라는 것은 함수를 객체와 동일하게 사용할 수 있다는 의미
  객체는 값이므로 함수는 값과 동일하게 취급할 수 있음
- 따라서 함수는 값을 사용할 수 있는 곳(변수 할당문, 객체의 프로퍼티 값, 배열의 요소, 함수 호출의 인수, 함수 반환문)이라면 어디서든지 리터럴로 정의할 수 있으며 런타임에 함수 객체로 평가됨
- 일급 객체로서 함수가 가지는 가장 큰 특징은 일반 객체와 같이 함수의 매개변수에 전달할 수 있으며,
  함수의 반환값으로 사용할 수도 있다는 것임
- 함수는 객체이지만 일반 객체와는 차이가 있음
  - `일반 객체` 호출 X ↔ `함수 객체` 호출 O
  - `함수 객체` 는 일반 객체에는 없는 `함수 고유의 프로퍼티` 를 소유함

### 함수 객체의 프로퍼티

- 함수는 객체임. 따라서 함수도 프로퍼티를 가질 수 있음
- 브라우저 콘솔에서 square 함수의 모든 프로퍼티의 프로퍼티 어트리뷰트를
  Object.getOwnPropertyDescriptors 메서드로 확인해보면 다음과 같음
  <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/7c31692c-9bc9-4ab9-bb52-64d68e5458f2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220719%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220719T114551Z&X-Amz-Expires=86400&X-Amz-Signature=82a44383df741619f6938a321ed0027009a94a2df4fbd4934291fe350663648a&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" style="width: 748px">

  - arguments, caller, length, name, prototype 프로퍼티는 모든 함수 객체의 데이터 프로퍼티
  - 이들 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 프로퍼티

- **proto** 는 square 함수의 프로퍼티가 아님. Object.prototype 객체의 접근자 프로퍼티임.
  - **proto** 는 접근자 프로퍼티로 함수 객체 고유의 프로퍼티가 아니라,
    Object.prototype 객체의 프로퍼티를 상속받음
  - square 함수는 Object.prototype 객체로부터 **proto** 접근자 프로퍼티 상속 받음.
  - Object.prototype 객체의 프로퍼티는 모든 객체가 상속받아 사용할 수 있음
    즉, Object.prototype 객체의 **proto** 접근자 프로퍼티는 모든 객체가 사용할 수 있음
    → 19장 “프로토타입" 에서 자세히 살펴볼 예정

### arguments 프로퍼티

- 함수 객체의 arguments 프로퍼티 값은 arguments 객체
- arguments 객체는 함수 호출 시 전달된 인수(argument)들의 정보를 담고 있는 순회 가능한
  `유사 배열 객체`
- 함수 내부에서 지역 변수처럼 사용할 수 있는 arguments 객체를 참조
  즉, 함수 외부에서는 참조할 수 없음
- 함수 객체의 arguments 프로퍼티는 현재 일부 브라우저에서 지원하고 있지만,
  ES3부터 표준에서 폐지됨. 따라서 Function.arguments 와 같은 사용법은 권장되지 않음.
- 자바스크립트는 `함수의 매개변수` 와 `인수(arguments)` 의 `개수` 가 일치하는지 확인하지 않음
- 따라서 함수 호출 시 매개변수 개수만큼 인수를 전달하지 않아도 에러가 발생하지 않음
- arguments 객체의 프로퍼티 예제
  <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8f4772f5-e378-447a-8d93-eabdf0014369/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220719%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220719T114644Z&X-Amz-Expires=86400&X-Amz-Signature=6cbdc9a70d84d851d3080ba6d9bf56360e22318615781ae7ec436e168593d53e&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" style="width: 748px">
  - 함수를 정의할 때 선언한 매개변수는 함수 몸체 내부에서 변수와 동일하게 취급됨
    즉, 함수가 호출되면 함수 몸체 내에서 암묵적으로 매개변수가 선언되고,
    undefined로 초기화 된 이후 인수가 할당됨
  - 선언된 매개변수의 개수보다 인수를 적게 전달했을 경우(multiply(), multiply(1))
    인수가 전달되지 않은 매개변수는 undefined 로 초기화 된 상태를 유지함
  - 매개변수의 개수보다 인수를 더 많이 전달한 경우(multiply(1,2,3)) 초과된 인수는 무시됨.
    하지만 그냥 버려지는 것은 아님.
  - 모든 인수는 암묵적으로 arguments 객체의 프로퍼티로 보관됨
  - arguments 객체는 인수를 프로퍼티의 값으로 소유하며 프로퍼티의 키는 인수의 순서를 나타냄
  - arguments 객체의 callee 프로퍼티는 호출되어 arguments 객체를 생성한 함수,
    즉 함수 자신을 가리킴
  - arguments 객체의 length 프로퍼티는 인수의 개수를 가리킴
- 선언된 매개변수의 개수와 함수를 호출할 때 전달하는 인수의 개수를 확인하지 않는 자바스크립트의 특성 때문에 함수가 호출되면 인수 개수를 확인하고 이에 따라 함수 동작을 달리 정의할 필요가 있을 수 있음 → 이때 유용하게 사용되는 것이 arguments 객체
- arguments 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용함
- arguments 객체는 배열 행태로 인자 정보를 담고 있지만 실제 배열이 아닌 유사 배열 객체임
  ( \* 유사 배열 객체? length 프로퍼티를 가진 객체로 for 문으로 순회할 수 있는 객체 )
- 유사 배열 객체는 배열이 아니므로 배열 메서드를 사용할 경우 에러가 발생
- 따라서 배열 메서드를 사용하려면 Function.prototype.call, Function.prototype.apply 사용해 간접 호출해야 하는 번거로움이 있음
  - 이러한 번거로움을 해결하기 위해 ES6 에서는 Rest 파라미터를 도입

### caller 프로퍼티

- caller 프로퍼티는 ECMAScript 사양에 포함되지 않은 비표준 프로퍼티
- 참고로만 알아두셈!
  - 함수 호출 foo(bar)의 경우 bar 함수를 foo 함수 내에서 호출함
  - 이때 bar 함수의 caller 프로퍼티는 bar 함수를 호출한 foo 함수를 가르킴
  - 함수 호출 bar()의 경우 bar 함수를 호출한 함수는 없음
    따라서 caller 의 프로퍼티는 null

### length 프로퍼티

- 함수 객체의 length 프로퍼티는 함수를 정의할 때 선언한 매개변수의 개수를 가리킴

  ```jsx
  function foo() {
    console.log(foo.length) // ?
  }

  function bar(x) {
    return x
  }
  console.log(bar.length) // ?

  function baz(x, y) {
    return x * y
  }
  console.log(baz.length) // ?
  ```

  <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2452e402-ec4b-46a7-bfd5-fe58402a00c2/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220719%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220719T114754Z&X-Amz-Expires=86400&X-Amz-Signature=19b49ee4dd6f4437ac55f7f5aeeda8959c9355332fb212a9cb255864afe476b6&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" style="width: 748px">

- arguments 객체의 length 프로퍼티는 인자의 개수를 가르키고,
  함수 객체의 length 프로퍼티는 매개변수의 개수를 가르킴.
- arguments 객체의 length 프로퍼티와 함수 객체의 length 프로퍼티 값은 다를 수 있으므로 주의!

### name 프로퍼티

- 함수 객체의 name 프로퍼티는 함수 이름을 나타냄
- name 프로퍼티는 ES6 에서 정식 표준이 되었음
- 익명 함수 표현식의 경우 ES5 에서 name 프로퍼티는 빈 문자열을 값으로 가짐
- 하지만 ES6 에서는 함수 객체를 가리키는 식별자를 값으로 가짐
- 예제를 통해 알아보아요

  ```jsx
  // 기명 함수식 표현
  var namedFunc = function foo() {}
  console.log(namedFunc.name) // ?

  // 익명 함수식 표현
  var anonymousFunc = function () {}
  console.log(anonymousFunc.name) // ES5: ? , ES6: ?

  // 함수 선언문
  function bar() {}
  console.log(bar.name) // ?
  ```

  <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/a2233706-9366-42fa-955c-a656825c58bf/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220719%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220719T114833Z&X-Amz-Expires=86400&X-Amz-Signature=9e935f905e7944b611e86ccff14f35876d0e64f72df41916a42090aebb75633b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" style="width: 748px">

  - 함수 이름과 함수 객체를 가르키는 식별자는 의미가 다름!
  - 함수를 호출할 때는 함수 이름이 아닌 함수 객체를 가리키는 식별자로 호출함

### **proto** 접근자 프로퍼티

- 모든 객체는 [[Prototype]] 이라는 내부 슬롯을 가짐
- [[Prototype]] 내부 슬롯은 객체지향 프로그래밍의 상속을 구현하는 프로토타입 객체를 가르킴
- **proto** 프로퍼티는 [[Prototype]] 내부 슬롯이 가르키는 프로토타입 객체에 접근하기 위해
  사용하는 접근자 프로퍼티
- [[Prototype]] 내부 슬롯에 직접 접근할 수 없고, **proto** 접근자 프로퍼티를 통해 간접적으로 프로토타입 객체에 접근할 수 있음
  <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/4c89f23a-9469-4b74-97a3-7c60033e983f/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220719%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220719T114905Z&X-Amz-Expires=86400&X-Amz-Signature=e18da08cc73470c94b3e21c2bb330d611d62f6cb5eed05704732d3048d81489b&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" style="width: 748px">

### prototype 프로퍼티

- prototype 프로퍼티는 생성자 함수
- 호출할 수 있는 함수 객체, 즉 constructor 만이 소유하는 프로퍼티
- 일반 객체와 생성자 함수로 호출할 수 없는 non-constructor 에는 prototype 프로퍼티가 없음
  <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/9face1cd-2f80-48d9-ad07-4da79a06420d/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220719%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220719T114921Z&X-Amz-Expires=86400&X-Amz-Signature=4151fd8e55e704ef9a1d9795981ef4b7e8c85d4e9072daa2d5c7dece405496d2&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject" style="width: 748px">
- prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때,
  생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가리킴
