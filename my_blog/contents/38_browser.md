---
date: '2022-06-05'
title: 'Javascript Deep Dive'
categories: ['TIL', 'Deep Dive']
summary: '38. 브라우저의 렌더링 과정'
thumbnail: './deep_dive.png'
---

## 브라우저의 렌더링 과정

- 파싱?
  - 프로그래밍 언어의 문법에 맞게 작성된 텍스트 문서를 읽어 들여 실행하기 위해 텍스트 문서의 문자열을 토큰으로 분해하고, 토큰에 문법적 의미와 구조를 반영하여 트리 구조의 자료구조인 파스 트리를 생성하는 일련의 과정을 말함
- 렌더링?

  - HTMl, CSS, 자바스크립트로 작성된 문서를 파싱하여 브라우저에 시각적으로 출력하는 것을 말함

- 브라우저는 다음과 같은 과정을 거쳐 렌더링을 수행함
  1. 브라우저는 HTML, CSS, 자바스크립트, 이미지, 폰트 파일 등 렌더링에 필요한 리소스를 요청하고 서버로부터 응답을 받음
  2. 브라우저의 렌더링 엔진은 서버러부터 응답된 HTML과 CSS를 파싱하여 DOM과 CSSOM을 생성하고 이들을 결합하여 렌터 트리를 생성함
  3. 브라우저의 자바스크립트 엔진은 서버로부터 응답된 자바스크립트를 파싱하여 AST(Abstract Syntax Tree)를 생성하고 바이트코드로 변환하여 실행함. 이때 자바스크립트는 DOM API 를 통해 DOM이나 CSSOM을 변경할 수 있음. 변경된 DOM과 CSSOM은 다시 렌더 트리로 결함됨
  4. 렌더 트리를 기반으로 HTML 요소의 레이아웃(위치와 크기)을 계산하고 브라우저 화면에 HTML 요소를 페인팅 함
     <br>

### 요청과 응답

- 브라우저의 핵심 기능은 필요한 리소스를 서버에 요청하고, 서버로부터 응답을 받아 브라우저에 시각적으로 렌더링 하는 것
- 즉, 렌더링에 필요한 리소스는 모두 서버에 존재하므로 필요한 리소스를 서버에 요청하고 서버가 응답한 리소스를 파싱하여 렌더링하는 것
- 서버에 요청을 전송하기 위해 브라우저는 주소창을 제공함
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F4jfhy%2Fbtrvbhya9KK%2FMSygerMcKAdZbWsJP33KzK%2Fimg.png"  style="width: 748px" />

- 서버에 요청을 하기 위해서는 브라우저 주소창에 URL을 입력하고 엔터키를 누름
- URL의 호스트 이름이 DNS를 통해 IP 주소로 변환되고 이 IP 주소를 갖는 서버에게 요청을 전송함
- 요청과 응답은 개발자도구 Network 패널에서 확인 가능
- 브라우저의 렌더링 엔진이 HTML을 파싱하는 도중에 외부 리소스를 로드하는 태그, 즉 CSS 파일 로드하는 link 태그, img 태그, script 태그 등 만나면 HTML의 파싱을 일시 중단하고 해당 리소스 파일로 서버를 요청하기 때문

<br>

### HTML 파싱과 DOM 생성

- 브라우저의 요청에 의해 서버가 응답한 HTML 문서는 문자열로 이루어진 순수한 텍스트
- 이러한 HTML 문서를 브라우저에 시각적인 픽셀로 렌더링 하려면 브라우저가 이해할 수 있는 자료구조(객체)로 변환하여 메모리에 저장해야 함
- 브라우저의 렌더링 엔진은 응답받은 HTML 문서를 파싱하여 브라우저가 이해할 수 있는 자료구조인 DOM(Document Object Model)을 생성함
- `DOM` 은 `HTML 문서를 파싱한 결과물`

<br>

### CSS 파싱과 CSSOM 생성

- 렌더링 엔진은 HTML 을 처음부터 한 줄씩 순차적으로 파싱하며 DOM 을 생성해 나감
- 도중에 link 태그나 style 태그를 만나면 DOM 생성을 일시 중단함
- link 태그의 href 어트리뷰트에 지정된 CSS 파일을 서버에 요청하여 로드한 CSS 파일이나,
- style 태그 내의 CSS를 HTML과 동일한 파싱 과정을 거치며 해석하여 CSSOM 을 생성함
- 이후 CSS 파싱을 완료하면 HTML 파싱이 중단된 지점부터 다시 HTML 파싱하기 시작하여 DOM 생성을 재개함
  <br>

### 렌더 트리 생성

- 렌더링 엔진은 서버로부터 응답된 `HTML` 과 `CSS` 를 파싱하여 각각 `DOM` 과 `CSSOM` 를 생성함
- DOM 과 CSSOM 은 렌더링을 위해 `렌더 트리`로 결합됨
- 렌더 트리는 렌더링을 위한 트리 구조의 자료구조
- 따라서 브라우저 화면에 렌더링되지 않는 노드(meta, script 태그 등)와 CSS에 의해 비표시되는 노드들은 포함하지 않음
- 즉, 렌더 트리는 브라우저 화면에 렌더링되는 노드만으로 구성됨
- 이후 완성된 렌더 트리는 각 HTML 요소의 레이아웃(위치와 크기)을 계산하는 데 사용되며 브라우저 화면에 픽셀을 렌더링하는 페인팅 처리에 입력됨
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FcmYdP4%2Fbtru9Po3G9K%2FddHcmCNkMMaT7daDaRKDDK%2Fimg.png" style="width: 748px" />

- 브라우저의 렌더링 과정은 반복해서 실행될 수 있음
- 리렌더링은 성능에 악영향을 주는 작업이므로 가급적 빈번하게 발생하지 않도록 주의해야 함

<br>

### 자바스크립트 파싱과 실행

- HTML 문서를 파싱한 결과물로서 생성된 DOM은 HTML 문서의 구조와 정보뿐만 아니라 HTML 요소와 스타일 등을 변경할 수 있는 프로그래밍 인터페이스로서 DOM API를 제공함
- 즉, 자바스크립트 코드에서 DOM API 를 사용하면 이미 생성된 DOM 을 동적으로 조작할 수 있음
- 렌더링 엔진은 HTML 을 한 줄씩 순차적으로 파싱하다 script 태그를 만나면 DOM 생성을 일시 중지함
- 그리고 script 태그 내의 자바스크립트 코드 등을 파싱하기 위해 자바스크립트 엔진에 제어권을 넘김
- 이후 자바스크립트 파싱과 실행이 종료되면 DOM 생성을 재개함
- 자바스크립트 파싱과 실행은 자바스크립트 엔진이 처리함
- 렌더링 엔진으로부터 제어권을 넘겨받은 자바스크립트 엔진이 자바스크립트 코드를 파싱하는데,
- 자바스크립트 엔진은 자바스크립트를 해석하여 AST(Abstruct Syntax Tree 추상적 구문 트리)를 생성하고, 이를 기반으로 인터프리터가 실행할 수 있는 중간 코드인 바이트코드를 생성하여 실행함

<br>

### 리플로우와 리페인트

- 자바스크립트 코드에서 DOM 이나 CSSOM 을 변경하는 DOM API가 사용된 경우 DOM 이나 CSSOM이 변경됨
- 이때 변경된 DOM과 CSSOM은 다시 렌더 트리로 결합되고 변경된 렌더 트리를 기반으로 레이아웃과 페인트 과정을 거쳐 브라우저의 화면에 다시 렌더링 함
- 이를 `리플로우`, `리페인트` 라고 함
- 리플로우는 레이아웃을 다시 계산하는 것을 말하는 것으로, <br>
  노드 추가/삭제, 요소 크기/위치 변경 등 레이아웃에 영향을 주는 변경이 발생한 경우에 한하여 실행됨
- 리페인트는 재결합된 렌터 트리를 기반으로 다시 페인트를 하는 것
- 기존 요소에 변경 사항이 생겼다고 항상 리플로우-리페인트가 일어나는 것은 아님
- 레이아웃에 영향이 미치지 않는 단순 색상 변경 같은 사항은 리플로우 수행 없이 리페인트만 수행하게 됨
- 하지만 리플로우가 일어나면 반드시 리페인트가 수행됨

- 리플로우(Reflow)가 일어나는 대표적인 속성들

  - position
  - width, height, margin, padding
  - border, border-width
  - font-size, font-weight
  - line-height, text-align, overflow

- 리페인트(Repaint)만 일어나는 대표적인 속성들
  - background
  - color, text-decoration
  - border-style, border-radius

<br>

### 자바스크립트 파싱에 의한 HTML 파싱 중단

- 렌더링 엔진과 자바스크립트 엔진은 직렬적으로 파싱을 수행함
- 즉, HTML 문서에서 위에서 아래 방향으로 순차적으로 HTML, CSS, JS를 파싱하고 실행함
- 이때 script 태그의 위치에 따라 HTML 파싱이 블로킹 되어 DOM 생성이 지연될 수 있으므로, script 태그의 위치는 중요한 의미를 갖음
- 만약 자바스크립트 코드가 DOM을 변경하는 DOM API를 사용할 때 DOM 생성이 완료되지 않은 상태라면 문제가 발생할 수 있음
- 또한 자바스크립트 로딩/파싱/실행으로 인해 HTML 요소들의 렌더링에 지장받아 페이지 로딩 시간이 늘어날 수 있음
- 이를 해결하기 위해서 body 요소의 가장 아래에 자바스크립트를 위치시키거나, async/defer 어트리뷰트를 이용하는 방법이 있음

<br>

### script 태그의 async/defer 어트리뷰트

- 자바스크립트 파싱에 의한 DOM 생성이 중단되는 문제를 근본적으로 해결하기 위해 HTML5부터 script 태그에 async와 defer 어트리뷰트가 추가되었음
- async/defer 어트리뷰트는 src 어트리뷰트를 통해 외부 자바스크립트 파일을 로드하는 경우에만 사용이 가능함

```js
<script async src="extern.js"></script>
<script defer src="extern.js"></script>
```

- async/defer 어트리뷰트를 사용하면 HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행됨
- 하지만 자바스크립트의 실행 시점에 차이가 존재

<br>

### async 어트리뷰트

- HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행됨
- 단, 자바스크립트 파싱과 실행은 자바스크립트 파일의 로드가 완료된 직후 진행되며 이때 HTML 파싱이 중단됨
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FsQd8c%2FbtrvhMKUIDi%2F8z4acSXPTfhDjoAW7QpagK%2Fimg.png" style="width: 748px" />

- 여러개의 script 태그에 async 를 지정하면 script 태그의 순서와는 상관없이 로드가 완료된 자바스크립트 부터 먼저 실행되므로 순서가 보장되지 않음

<br>

### defer 어트리뷰트

- async 와 마찬가지로 HTML 파싱과 외부 자바스크립트 파일의 로드가 비동기적으로 동시에 진행됨
- 단, 자바스크립트의 파싱과 실행은 HTML 파싱이 완료된 직후
- 즉, DOM 생성이 완료된 직후 진행됨
  <img src="https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ft0E6H%2FbtrvaJPEvX7%2FjIW8nHYtZnfo9KndoMKZkK%2Fimg.png" style="width: 748px" />

- 따라서 DOM 생성이 완료된 이후 실행되어야 할 자바스크립트에 유용함
  <br>
