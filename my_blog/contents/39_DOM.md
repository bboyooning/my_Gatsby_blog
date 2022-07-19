---
date: '2022-06-06'
title: 'Javascript Deep Dive'
categories: ['TIL', 'Deep Dive']
summary: '39. DOM'
thumbnail: './deep_dive.png'
---

## DOM

- DOM(Document Object Model) 은 HTML 문서의 계층적 구조와 정보를 표현하며 이를 제어할 수 있는 API, 즉 프로퍼티와 메서드를 제공하는 트리 자료구조
  <br>

### HTML 요소와 노드 객체

- HTML 요소는 HTML 문서를 구성하는 개별적인 요소를 의미
- HTML 요소는 렌더링 엔진에 의해 파싱되어 DOM 을 구성하는 요소 노드 객체로 변환됨
- 이때 HTML 요소의 어트리뷰트는 어트리뷰트 노드로, HTML 요소의 텍스트 콘텐츠는 텍스트 노드로 변환됨
- HTML 문서는 HTML 요소들의 집합으로 이뤄지며, HTML 요소는 중첩 관계를 갖음
- 즉, HTML 요소의 콘텐츠 영역(시작 태그와 종료 태그 사이)에는 텍스트뿐만 아니라 다른 HTML 요소도 포함할 수 있음
- 이때 HTML 요소 간에는 중첩 관계에 의해 계층적인 부자 관계가 형성됨
- 이러한 HTML 요소 간의 부자 관계를 반영하여 HTML 문서의 구성요소인 HTML 요소를 객체화한 모든 노드 객체들을 트리 자료 구조로 구성함

<br>

### 트리 자료구조

- 노드들의 계층 구조로 이루어짐
- 즉, 트리 자료구조는 부모 노드와 자식 노드로 구성되어 노드 간의 계층적 구조(부자, 형제 관계)를 표현하는 비선형 자료구조 를 말함
- 트리 자료구조는 하나의 최상위 노드에서 시작함
- 최상위 노드는 부모 노드가 없으며, 루트 노드라 함
- 루트 노드는 0개 이상의 자식 노드를 갖음
- 자식 노드가 없는 노드를 리프 노드 라고 함
- 노드 객체들로 구성된 트리 자료구조를 DOM 이라 함
- 노드 객체의 트리로 구조화되어 있기 때문에 DOM 을 DOM 트리라고 부름

### 노드 객체의 타입

- 렌더링 엔진은 HTML 문서를 파싱하여 다음과 같이 DOM을 생성함

  ```html
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="UTF-8" />
      <link rel="stylesheet" href="style.css" />
    </head>
    <body>
      <ul>
        <li id="apple">Apple</li>
        <li id="banana">Banana</li>
        <li id="orange">Orange</li>
      </ul>
      <script src="app.js"></script>
    </body>
  </html>
  ```

  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbPAbSi%2FbtrAVsA1ACk%2F7mEGOEkGVyjXitUwriik11%2Fimg.png" style="width: 748px" />

- 이처럼 DOM 은 노드 객체의 계층적인 구조로 구성됨
- 노드 객체는 총 12개의 종류(노드 타입)가 있음
- 그 중 4개가 중요함!

#### `문서 노드`

- 문서 노드는 DOM 트리의 최상위에 존재하는 루트 노드로서 document 객체를 가리킴
- document 객체는 브라우저가 렌더링한 HTML 문서 전체를 가리키는 객체로서 전역 객체 window의 document 프로퍼티에 바인딩 되어있음
- 따라서 문서 노드는 window.document 또는 document 로 참조할 수 있음
- 브라우저 환경의 모든 자바스크립트 코드는 script 태그에 의해 분리되어 있어도 하나의 전역 객체 window 를 공유함
- 따라서 모든 자바스크립트 코드는 전역 객체 window의 document 프로퍼티에 바인딩 되어 있는 하나의 document 객체를 바라봄
- 즉, HTML 문서당 document 객체는 유일함
- 문서노드, 즉 document 객체는 DOM 트리의 루트 노드이므로 DOM 트리의 노드들에 접근하기 위한 진입점 역할을 담당
- 즉, 요소, 어트리뷰트, 텍스트 노드에 접근하려면 문서 노드를 통해야 함

#### `요소 노드`

- 요소 노드는 HTML 요소를 가리키는 객체
- HTML 요소 간의 중첩에 의해 부자 관계를 가지며, 이를 통해 정보를 구조화 함
- 따라서 요소 노드는 문서의 구조를 표현함

#### `어트리뷰트 노드`

- 어트리뷰트 노드는 HTML 요소의 어트리뷰트를 가리키는 객체
- 어트리뷰트가 지정된 HTML 요소의 요소 노드와 연결되어 있음
- 어트리뷰트 노드는 부모 노드가 없고 요소 노드에만 연결되어 있음
- 어트리뷰트를 참조하거나 변경하려면 먼저 요소 노드에 접근해야 함

#### `텍스트 노드`

- 텍스트 노드는 HTML 요소의 텍스트를 가리키는 객체
- 요소 노드가 문서의 구조를 표현한다면, 텍스트 노드는 문서의 정보를 표현함
- 텍스트 노드는 DOM 트리의 최종단
- 텍스트 노드에 접근하려면 먼저 부모 노드인 요소 노드에 접근해야 함

<br>

### 노드 객체의 상속 구조

- DOM 을 구성하는 노드 객체는 자신의 구조와 정보를 제어할 수 있는 DOM API 를 사용할 수 있음
- DOM 을 구성하는 노드 객체는 ECMAScript 사양에 정의된 표준 빌트인 객체가 아닌, 브라우저 환경에서 추가적으로 제공하는 호스트 객체임
- 하지만 노드 객체도 자바스크립트 객체이므로 프로토타입에 의한 상속 구조를 갖음
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcDX134%2FbtrAYmthuJ7%2FGlcerFsrqKiqY62CvNFbpK%2Fimg.png" style="width: 748px" />
- 모든 노드 객체는 Object, EventTarget, Node 인터페이스를 상속받음
- input 요소 노드 객체의 특성
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcetzMz%2FbtrAWA6BMpc%2FmEfMLikTstxrjQQkLvR2BK%2Fimg.png" style="width: 748px" />

- 노드 객체의 상속 구조는 개발자 도구 Elements 패널 우측의 Properties 패널에서 확인 가능
- DOM 은 HTML 문서의 계층적 구조와 정보를 표현하는 것은 물론 노드 객체의 종류, 즉 노드 타입에 따라 필요한 기능을 프로퍼티와 메서드의 집합인 DOM API 로 제공함
- 이 DOM API 를 통해 HTML 의 구조나 내용 또는 스타일 등을 동적으로 조작할 수 있음
  <br>
