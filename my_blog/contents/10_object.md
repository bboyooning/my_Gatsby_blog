---
date: '2022-06-01'
title: 'Javascript Deep Dive'
categories: ['TIL', 'Deep Dive']
summary: '10. 객체'
thumbnail: './deep_dive.png'
---

## 객체

- 자바스크립트는 객체 기반의 프로그래밍 언어
- 자바스크립트를 구성하는 거의 ‘모든 것'이 객체
- 원시 값을 제외한 나머지 값(함수, 배열, 정규 표현식 등)은 모두 객체
- 원시 타입은 단 하나의 값만 나타내지만, 객체 타입은 다양한 타입의 값(원시 값 또는 다른 객체)을 하나의 단위로 구성한 복합적인 자료구조
- 원시 값은 변경이 불가능 하지만 객체 타입의 값, 즉 객체는 변경 가능한 값
- 객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키와 값으로 구성됨

```jsx
var person = {
  name: 'Lee',
  age: 20,
}
```

- 자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있음
- 자바스크립트의 함수는 일급 객체 이므로 값으로 취급할 수 있음<br>
  ( \* 일급객체? 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체 )<br>
  따라서 함수도 프로퍼티 값으로 사용할 수 있음
- 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 이를 `메서드` 라고 부름

```jsx
var counter = {
  num: 0, // 프로퍼티
  increase: function () {
    // 메서드
    this.num++
  },
}
```

- 이처럼 객체는 `프로퍼티`와 `메서드`로 구성된 집합체
  - `프로퍼티` : 객체의 상태를 나타내는 값(data)
  - `메서드` : 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작
- 객체는 객체의 상태를 나타내는 값(프로퍼티)과 프로퍼티를 참조하고 조작할 수 있는 동작(메서드)을 모두 포함할 수 있기 때문에 <br> 상태와 동작을 하나의 단위로 구조화할 수 있어 유용
  <br>

### 객체 리터럴에 의한 객체 생성

- 자바스크립트는 프로토타입 기반 객체지향 언어로서 다양한 객체 생성 방법을 지원
  - 객체 리터럴
  - Object 생성자 함수
  - 생성자 함수
  - Object.create 메서드
  - 클래스(ES6)
- 객체 생성 방법 중에서 가장 일반적이고 간단한 방법이 `객체 리터럴` 을 사용하는 방법
- 리터럴? 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용하여 값을 생성하는 표기법
- 객체 리터럴? 객체를 생성하기 위한 표기법
- 객체 리터럴은 중괄호({…}) 내에 0개 이상의 프로퍼티를 정의
  변수에 할당되는 시점에 자바스크립트 엔진은 객체 리터럴을 해석하여 객체를 생성함
- 만약 중괄호 내에 프로퍼티를 정의하지 않으면 빈 객체 생성
- 객체 리터럴의 중괄호는 코드 블록을 의미하지 않음! 따라서 코드 블록을 닫는 중괄호 뒤에는 세미콜론을 붙이지 않음
- 하지만 객체 리터럴은 값으로 평가되는 표현식이므로, 객체 리터럴의 닫는 중괄호 뒤에는 세미콜론을 붙인다.
- 객체 리터럴은 자바스크립트의 유연함과 강력함을 대표하는 객체 생성 방식
- 객체 리터럴에 프로퍼티를 포함시켜 객체를 생성함과 동시에 프로퍼티를 만들 수도 있고, 객체를 생성한 이후 프로퍼티를 동적으로 추가할 수도 있음
- 객체 리터럴 외의 객체 생성 방식은 모두 함수를 사용해 객체를 생성
  <br>

### 프로퍼티

- 객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성
- 프로퍼티를 나열할 때는 쉼표(,)로 구분. 일반적으로 마지막 프로퍼티 뒤에는 쉼표를 사용하지 않으나 사용해도 상관없음

```jsx
var minsu = {
  name: '민수변수', // 프로퍼티 키: name, 프로퍼티 값: '민수변수'
  age: 200, // 프로퍼티 키: age, 프로퍼티 값: 200
}
```

- 프로퍼티 키와 값으로 사용할 수 있는 값은 다음과 같음
  - 프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
  - 프로퍼티 값 : 자바스크립트에서 사용할 수 있는 모든 값
- 프로퍼티 키는 프로퍼티 값에 접근할 수 있는 이름으로서 식별자 역할을 함
- 심벌 값도 프로퍼티 키로 사용할 수 있지만 일반적으로 문자열을 사용. 이때 프로퍼티 키는 문자열이므로 따옴표(’…’ or “…”)로 묶어야 함
- 식별자 네이밍 규칙을 따르면 생략 가능 / 따르지 않으면 반드시 따옴표 사용
  - 식별자 네이밍 규칙
    - 반드시 문자로 시작
    - 1~30까지만 지정 가능
    - A~Z까지의 대소문자, 0~9까지의 숫자, 특수기호는 \_, $, #만 가능
    - 오라클에서 사용되는 예약어 사용 불가
    - 다른 객체명에서 사용중인 것과 중복 불가
    - 공백 사용 불가
- 식별자 네이밍 규칙을 따르지 않는 프로퍼티 키를 사용하면 번거롭기 때문에 가급적 식별자 네이밍 규칙을 준수하는 프로퍼티 키를 사용할 것을 권장
- 문자열 또는 문자열로 평가할 수 있는 표현식 사용해 프로퍼티 키를 동적으로 생성 가능. 이 경우에는 프로퍼티 키로 사용할 표현식을 대괄호([…])로 묶어야 함
- 빈 문자열을 프로퍼티 키로 사용해도 에러나지 않음
- 프로퍼티 키에 문자열이나 심벌 값 외의 값을 사용하면 암묵적 타입변환을 통해 문자열이 됨.<br>예를 들어, 프로퍼티 키로 숫자 리터럴을 사용하면 따옴표 붙지 않지만 내부적으로 문자열로 변환됨
- var, function 과 같은 예약어를 프로퍼티 키로 사용해도 에러 발생하지 않음<br>하지만 예상치 못한 에러가 발생할 여지가 있어 권장하지 않음
- 이미 존재하는 프로퍼티 키를 중복 선언하면, 나중에 선언한 프로퍼티가 먼저 선언한 프로퍼티를 덮어씀. 에러 발생하지 않으니 주의!
  <br>

### 메서드

- 자바스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값으로 사용할 수 있음
- 자바스크립트의 함수는 (일급)객체 이므로 값으로 취급할 수 있어 프로퍼티 값으로 사용가능
- 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위하여 `메서드` 라고 부름
- `메서드` 는 객체에 묶여 있는 함수를 의미함
  ```jsx
  var counter = {
    num: 0, // 프로퍼티
    increase: function () {
      // 메서드
      this.num++ // this 는 counter 객체를 가리킴
    },
  }
  ```
- 메서드 내부에 사용한 `this` 키워드는 객체 자신을 가르키는 참조변수
  <br>

### 프로퍼티의 접근

- 프로퍼티 접근 방법
  - 마침표 프로퍼티 접근 연산자(.)를 사용하는 `마침표 표기법`
  - 대괄호 프로퍼티 접근 연산자([…])를 사용하는 `대괄호 표기법`
- 프로퍼티 키가 식별자 네이밍 규칙을 준수하는, 즉 자바스크립트에서 사용 가능한 유효한 이름이라면 마침표 표기법과 대괄호 표기법 모두 사용할 수 있음
- 이 두 가지 방법의 좌측에는 객체로 평가되는 표현식을 기술, 마침표 프로퍼티 접근 연산자 우측 또는 대괄호 프로퍼티 접근 연산자 내부에는 프로퍼티 키 지정
- 대괄호 표기법 사용하는 경우, 대괄호 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 ‘’ `따옴표` 로 감싼 문자열이어야 함! <br>
  따옴표로 감싸지 않은 이름을 프로퍼티 키로 사용하면 자바스크립트 엔진은 식별자로 해석
- 프로퍼티 키가 식별자 네이밍 규칙을 준수하지 않는 이름
- 즉 자바스크립트에서 사용 가능한 유효한 이름이 아니면 `반드시` `대괄호 표기법`을 사용해야 함
- 단, 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표 생략할 수 있고, 대괄호 내에 들어가는 프로퍼티 키는 반드시 따옴표로 감싼 문자열이어야 함
- ex)

```jsx
var person = {
	'last-name': 'Lee',
	1: 10
};

person.'last-name';  // SyntaxError: Unexpected string
person.last-name;    // 브라우저 환경: NaN
                     // Node.js 환경: ReferenceError: name is not defined
person[last-name];   // ReferenceError: last is not defined
person['last-name']; // Lee

// 프로퍼티 키가 숫자로 이뤄진 문자열인 경우 따옴표 생략 가능
person.1;    // SyntaxError: Unexpected number
person.'1';  // SyntaxError: Unexpected string
person[1];   // 10 : person[1] -> person['1]
person['1']; // 10
```

- person.last-name 실행 결과는 node.js 환경에서 참조오류 이고, 브라우저에선 NaN
- person.last-name 실행할 때, 자바스크립트 엔진은 먼저 person.last 를 평가함.
  person 객체에서는 프로퍼티 키가 last 인 프로퍼티가 없기 때문에 undefined 로 평가됨<br>
  따라서 person.last-name 은 undefined-name 과 같다.<br>
  다음으로 자바스크립트 엔진은 name 이라는 식별자를 찾는다. 이때 name 은 프로퍼티 키가 아닌 식별자로 해석된다.

- Node.js 환경에서는 현재 어디에도 name 이라는 식별자(변수, 함수 등의 이름) 선언이 없으므로 <br> ReferenceError: name is not defined 에러가 발생함

- 그러나 브라우저 환경에서는 name 이라는 전역 변수(전역 객체 window의 프로퍼티)가 암묵적으로 존재함.
- 전역 변수 name 은 window 의 이름을 가르키며, 기본값은 빈 문자열임.따라서 person.last-name 은 undefined-’’와 같으므로 NaN 이 됨
  <br>

### 프로퍼티 값 갱신

- 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신됨<br>
  이미 person 이란 객체에 name 프로퍼티가 존재하므로 name 프로퍼티의 값이 갱신됨

<br>

### 프로퍼티 동적 생성

- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가되고 프로퍼티 값이 할당됨 <br>
  person 객체에 age 프로퍼티 존재하지 않으므로, person 객체에 age 프로퍼티가 동적으로 생성되고 갑이 할당됨

<br>

### 프로퍼티 삭제

- delete 연산자는 객체의 프로퍼티를 삭제함
- 이때 delete 연산자의 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 함<br>
  만약 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시됨
- person 객체에 age 프로퍼티를 동적으로 생성한 후, delete 연산자로 age 프로퍼티 삭제<br>
  address 프로퍼티는 존재하지 않아서 삭제할 수 없다. 하지만 에러는 발생하지 않음!

<br>

### ES6에서 추가된 객체 리터럴의 확장 기능

### 프로퍼티 축약 표현

- 객체 리터럴의 프로퍼티는 프로퍼티 키와 값으로 구성됨
- 프로퍼티 값은 변수에 할당된 값, 즉 식별자 표현식 일수도 있음
- ES6 에서는 프로퍼티 값으로 변수를 사용하는 경우, 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있음<br>
  이때 프로퍼티 키는 변수 이름으로 자동 생성됨
  <br>

### 계산된 프로퍼티 이름

- 문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용하여 프로퍼티 키를 동적으로 생성할 수도 있음 <br>
  단, 프로퍼티 키로 사용할 표현식을 대괄호([…])로 묶어야 함. 이를 `계산된 프로퍼티 이름` 이라고 함
- ES5 에서 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성하려면 객체 리터럴 외부에서 대괄호([…]) 표기법을 사용해야 함
- ES6 에선 객체 리터럴 내부에서도 계산된 프로퍼티 이름으로 프로퍼티 키를 동적 생성 가능
  <br>

### 메서드 축약 표현

- ES5에서 메서드를 정의하려면 프로퍼티 값으로 함수를 할당
- ES6에서 메서드를 정의할 때 function 키워드를 생략한 축약 표현 사용 가능
- ES6의 메서드 축약 표현으로 정의한 메서드는 프로퍼티에 할당한 함수와 다르게 동작함
  → ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다

  ````jsx
  const obj = {
  x: 1,
  foo() { return this.x; } // foo는 메서드다

        //bar에 바인딩 된 함수는 메서드가 아닌 일반 함수다.
        bar: function() { return this.x; }
      };

      obj.foo // 1
      obj.bar // 1
      ```

      - ES6 사양에서 정의한 메서드는 인스턴스를 생성할 수 없는 non-constructor이다. 따라서 생성자 함수로서 호출할 수 없다.

          ```jsx
          new obj.foo(); // is not a function
          ```

      - ES6의 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
  ````
