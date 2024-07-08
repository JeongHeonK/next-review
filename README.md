### next.js 복습입니다.

- app router 기반

---

### layout.js

- 최소 1개 이상의 layout.js가 필요하다.
- children props 사용
- `<head></head>` 컴포넌트 대신 `metadata` 사용

---

### icon

- `icon` 이름을 가진 이미지는 next.js에서 favicon으로 인식한다.

---

### 보호되는 파일명

`app/` 폴더 내부에서만 적용

- page.js
- layout.js
- loading.js
- not-found.js
- error.js
- route.js

---

### Dynamic route

`[]` 를 이용해서 만듬. (폴더 명 혹은 파일명)

- 에시 `@/app/blog/[slug]/page.js`
- page.js에서 작성한 컴포넌트에서 'params' 사용가능
- blog/`입력한 값`이 params에 저장된다.
- params내에 { slug: 'post-1' }이란 값이 저장됨.

---

### img

- 이미지 파일을 `import logoImg from "@/assets/logo.png";` 이런 형식으로 불러올 경우 src 에서는 `logoImg.src`라고 적어줘야 load된다.
- `Image` 사용이 권장됨.
  - 이미지 최적화
  - lazy loading
  - `priority` attr 사용 시, lazy loading 사용 안함

---

### 서버 컴포넌트와 클라이언트 컴포넌트

- next.js에서는 서버 컴포넌트와 클라이언트 컴포넌트를 구분한다.
- onClick, useHook등을 사용하려면 'use client'를 최상단에 적어 클라이언트 컴포넌트라는 걸 명시해야 한다.
- 서버 컴포넌트의 이점을 유지하려면, 컴포넌트를 분리하여 최대한 하위 컴포넌트가 클라이언트 컴포넌트가 되도록 한다.
