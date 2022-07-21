# my_Gatsby_blog

### 블로그 주소 : https://bboyooning.github.io/

<br>

### 리액트 정적 프레임워크인 Gatsby 를 사용하여 자기소개 블로그 만들기

- Technologies

  - Git, github
  - TypeScript , React , Gatsby , GraphQL, @emotion

<br>

- 구현 기능

  1. 반응형 디자인, 인피니티 스크롤 구현, 404 페이지 커스텀

  <br>

  - **반응형 디자인**: 각각 profile Image, Introduction 컴포넌트, CategoryList 컴포넌트, PostList 컴포넌트, Footer 컴포넌트에 일반적으로 사용되는 태블릿 너비인 768px에 맞춰서
    미디어 쿼리를 통해 반응형 디자인을 구현.

  - **인피니티 스크롤 구현**: 카테고리 별로 아이템을 필터링 해줌과 동시에 인피니티 스크롤 기능까지 제공하는 useInfiniteScroll 커스텀 훅을 통해 구현. (특정 요소가 화면에 보일 경우, 다음 데이터를 로드하는 방식)

  - **404 페이지 커스텀**

  <br>

  2. 메인 페이지

   <br>

  - 간단한 소개, 카테고리, 프로필 이미지로 구성
  - 필터 기능을 통해서 원하는 카테고리의 포스트에 들어 갈 수 있음
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/2de7d064-6b41-4d7a-8f51-8d125d21e377/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220721%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220721T113457Z&X-Amz-Expires=86400&X-Amz-Signature=a7ed1ce83eea9e1a0e6f323aeef4516998e1ac5b6c4ab1fc0a72c80d2979838c&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject">

  <br>

  3. 포스트

  - 상단 배경 안에 계속해서 카테고리가 보이도록 구현
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/8bed3a1b-c0e4-43de-ba5b-b3712d58fb49/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220721%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220721T113546Z&X-Amz-Expires=86400&X-Amz-Signature=6fa2335eb030ef87212d917312da8b484b0b892a31d2a9c34935246a71665af9&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject">
  - github Utterances 위젯을 통해 댓글을 달면 해당 repo issue에 댓글이 달릴 수 있게 함
    <img src="https://s3.us-west-2.amazonaws.com/secure.notion-static.com/07f7afac-07ce-4d71-b49e-a1d4b8eb5c2b/Untitled.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=AKIAT73L2G45EIPT3X45%2F20220721%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20220721T113607Z&X-Amz-Expires=86400&X-Amz-Signature=8a399f65ef0e977974b1d82e05a51d250c4ed14f854284cd5e4312088b6a29d0&X-Amz-SignedHeaders=host&response-content-disposition=filename%20%3D%22Untitled.png%22&x-id=GetObject">
