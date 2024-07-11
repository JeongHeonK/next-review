## Routeing & Rendering

### 병렬 라우트 설정 및 사용

- 별도의 경로를 가지는 2개의 페이지를 한 페이지에서 동시에 랜더링 하는 것.
- 폴더 경로는 `@` 로 시작 한다.
- 한 페이지 내에서 각각의 페이지가 다른 기능을 할 때 사용한다. (아니라면, 그냥 컴포넌트를 import해서 불러오는 것과 차이가 없다.)

```
- archive - layout.js
 ⎣@archive - page.js
 ⎣@latest - page.js
```

- 이렇게 작성할 경우 layout.js의 props에서 `@` 뒤에 적힌 경로를 키로 가진 프로퍼티를 사용할 수 있다.

- 병렬 라우터는 폴더를 무시한다. <br/>
  (@modal -> (.)image) => O <br/>
  (@modal -> (..)image) => X

```jsx
// layout.js
export default function archiveLayout({ archive, latest }) {
  // ... 생략
  return (
    <div>
      <section>{archive}</section>
      <section>{latest}</section>
    </div>
  );
}
```

---

### 병렬 라우트에서의 중첩 라우트

- default.js를 사용해야 한다.
- 만약 archive페이지에서 자신의 중첩 라우트인 `/archive/someurl`로 이동한다면, latest에서 보여줄게 없어진다.
- 이때 `default.js`를 통해 병렬 처리된 라우트가 이동할 때 보여줄 기본값을 설정한다. 병렬 라우팅 폴더 안에 만들어야 함 -> `@latest - default.js`

---

### Catch-All Route

- dynamic route 문제점.
  <br/>
  ex) post/[id] -> post/1, post/2 가능, post/1/123 불가능

- 중첩된 모든 요소의 route를 잡을 수 있다.`[id]` -> `[[...id]]`
  <br/>
  ex) post/[[...id]] -> post/1/123, post/2/22/44 가능

---

### Intercepting

- `(.)intercept할 URL` : 같은 디랙토리
- `(..)intercept할 URL` : 상위 디랙토리
- 새로고침이나 경로 직접 입력시 해당 URL로 이동
- Link 태그등을 이요해서 접속시 intercepting page 실행

---

### Intercept Routing과 병렬 라우팅 결합

- 모달창 구현시 사용
- server component에서 구현 가능.
- modal로 변경할 path를 병렬 라우팅 내에서 intercept 한다.
- 병렬 라우터는 폴더를 무시하기에 경로는 `(.)`로 설정한다. <br/>
  (@modal -> (.)image) => O <br/>
  (@modal -> (..)image) => X
- 병렬 라우터 내부이면서, intercepting하는 경로 상위에 `default.js`를 추가한다.
- 'default.js`는 null을 return 한다.

```jsx
export default parallelPageDefault() {
  return null;
}
```

---

### useRoute

- `"use client"`에서 사용 가능
- `const router = useRouter();`
- `router.back()`을 실행하면 뒤로가기.

---

### Route group

- `(폴더명)` 작성
- path에 안 뜸.
- 이를 통해 각각의 layout.ts페이지 생성가능
- 외에도 목적에 따라 파일들 분류 가능

---

### Route handler

- `api`폴더 생성.
- 함수 이름은 `GET, POST, PATCH, DELETE`로 만듬.
- 보통 JSON 데이터를 반환하거나 수신되는 json데이터를 수집하고 반환함.
- 기본 구조

```jsx
export function GET(request) {
  console.log(request);

  return new Response("Hello!");
}
```

---

### middleware

- 루트 경로에 `middleware.js` 생성

```jsx
import { NextResponse } from "next/server";

export function middleware(request) {
  console.log(request);
  return NextResponse.next();
}
```

- 모든 요청에 대한 경과를 처리함.

```jsx
import { NextResponse } from "next/server";

export function middleware(request) {
  console.log(request);
  return NextResponse.next();
}

export const config = {
  matcher: "/news",
};
```

- `config`: 미들웨어 설정 가능.

---
