---
layout: post
title: "Parcel로 React, TypeScript, Styled Components 쓰기"
date: "2021-07-31"
categories:
  - React
excerpt: |
  Parcel은 Webpack, Rollup보다 훨씬 간편하게 JS 번들을 관리하게 해준다. 별도의 설정 없이 간편하게 시작하고 싶다면 Parcel을 추천한다. 오늘은 Parcel을 이용해 React 프로젝트를 생성하고, TypeScript, Styled Components를 적용해본다.
feature_text: |
  ## Parcel로 React, TypeScript, Styled Components 쓰기
  Parcel은 Webpack, Rollup보다 훨씬 간편하게 JS 번들을 관리하게 해준다. 별도의 설정 없이 간편하게 시작하고 싶다면 Parcel을 추천한다. 오늘은 Parcel을 이용해 React 프로젝트를 생성하고, TypeScript, Styled Components를 적용해본다.
feature_image: "https://images.unsplash.com/photo-1614018453562-77f6180ce036?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1950&q=80"
image: "https://images.unsplash.com/photo-1614018453562-77f6180ce036?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1950&q=80"
---

가끔, CRA나 Webpack 설정은 너무 작업량이 많다는 생각이 들 때가 있다.

바닐라 JS는 가독성이나 유지보수 측면에서 엄두가 안 나고, SPA는 필요한데, CRA나 Webpack 커스터마이징은 또 너무 과하다고 생각되는 규모가 작은 프로젝트들을 하게될 때가 있다.

물론 template이나 boilerplate를 사용해도 된다. 하지만, boilerplate조차 무겁다고 느껴진다면 어떻게 해야할까?

그럴 때는 주저없이 parcel을 써보자.

사실 프로덕션 레벨에서 사용하는 번들링 도구로는 webpack, rollup 등 다양하지만, 이들 모두 내 마음에 맞게 사용하려면 상당한 시행착오를 거쳐야 한다.

익숙하지 않은 개발자가 프로젝트를 넘겨받게 되면 낮지 않은 러닝커브로 유지보수에 문제가 되기도 한다.

CRA를 쓰면 프로젝트 생성은 간편하지만, eject를 하지 않고, 입맛에 맞게 사용하는 것이 쉽지 않다는 단점이 있다. 내가 원하는 스타일링 방법을 도입하기 위해서는 eject를 하던가 craco를 쓰던가 해야하는데, 이런것 모두가 간단한 프로젝트 앞에서는 다 비용이다.

## Parcel 사용하기

먼저 디렉토리를 생성해주자.

```shell
mkdir react-parcel
npm init
```

다음으로 React와 TypeScript를 설치해준다.

```shell
npm i react react-dom
npm i -D @babel/preset-react
npm i -D @babel/preset-env
npm i -D parcel-bundler
npm i -D typescript
```

디렉토리 구조를 만들어 준다. 간단하게 사용하기 위해 `src` 만 만들어 본다.

```shell
mkdir src
touch index.html
touch index.tsx
```

`index.html` 에는 일반 React를 쓸 때처럼 아래와 같은 코드를 넣어주면 된다.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta http-equiv="X-UA-Compatible" content="IE=edge" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React Parcel</title>
  </head>
  <body>
    <div id="app"></div>
    <script src="index.tsx"></script>
  </body>
</html>
```

`package.json`에 아래 스크립트를 추가해준다.

```json
  "scripts": {
    "start": "parcel ./src/index.html",
    "build": "parcel ./src/index.html --no-sourcemaps"
  }
```

`build` 스크립트가 추가되어 있어야만 TypeScript로 작성한 모듈이나 컴포넌트가 정상적으로 작동한다.

다음으로 `tsconfig.json` 을 추가해 TypeScript 관련 설정을 진행한다.

```json
{
  "compilerOptions": {
    "esModuleInterop": true,
    "jsx": "react",
    "module": "commonjs",
    "strict": true,
    "target": "es5"
  }
}
```

다음으로 App 컴포넌트를 하나 만들어준다.

```tsx
// src/App.tsx
import React from "react";

export default function App() {
  return <div>Hello Jane</div>;
}
```

이제, `index.tsx` 에 아래 코드를 넣고 `npm run start` 로 작동시켜본다.

```tsx
// src/index.tsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

const mountNode = document.getElementById("app");
ReactDOM.render(<App />, mountNode);
```

실행.

```shell
npm run start
```

아마 기본적인 포트번호는 1234로 잡혀있을 것이다.

```shell
Server running at http://localhost:1234
```

라고 콘솔 창에 뜰 것이다. `localhost:1234` 로 이동해 App 컴포넌트가 제대로 마운트 되었는지, React가 정상적으로 동작하는지 확인한다.

## styled-components

styled-components를 이용하면 CSS를 JS처럼 사용할 수 있다. 모듈을 만들고, 재사용하며, 변수를 주고받고 JS로 동적으로 변형할 수도 있다.

less, sass 등 다양한 옵션도 있고, styled-components 특성상 성능 면에서 불리한 면도 있지만, 개발 편의성을 위해 사용하지 않을 이유가 없어 보인다.

### 설치

```shell
npm i styled-components styled-reset
npm i -D @types/styled-components
npm i -D babel-plugin-styled-components
```

마지막으로 설치된 바벨 플러그인은 SSR이나 스타일 Minification 등에 도움을 준다.

예를들어, `MyButton`이라는 styled 컴포넌트를 만들었다고 하자. 이 경우, 위 바벨 플러그인을 이용하면 React DevTools 없이도 `<button class="MyButton-asdf123 asdf123" />` 와 같이 보다 풍부한 정보를 확인할 수 있게 한다.

더불어 React DevTools가 있다면 `styled.button`이 아니라 `MyButton` 으로 보여지므로 디버깅이 훨씬 편리하다.

### GlobalStyles.ts

style을 reset 하고, 전역에서 설정되어야 할 부분들을 다루기 위해 `GlobalStyles.ts`를 두면 편리하다.

```typescript
import { createGlobalStyle } from "styled-components";
import reset from "styled-reset";

export const GlobalStyles = createGlobalStyle`
  ${reset}
  *{
    box-sizing: border-box;
    -ms-overflow-style: none;  /* IE and Edge */
    scrollbar-width: none;  /* Firefox */
  }
  *:focus {
    outline: none;
  }
  *::-webkit-scrollbar {
    display: none;
  }
  html {
    height: -webkit-fill-available;
  }
  a {
    text-decoration: none;
    cursor: pointer;
    color: inherit;
  }
`;
```

사용은 아래와 같이 하면 된다.

```tsx
import React from "react";
import { GlobalStyles } from "./GlobalStyles";

export default function App() {
  return (
    <>
      <GlobalStyles />
      <div className="text">Hello Jane</div>
    </>
  );
}
```

컴포넌트처럼 추가해준다.
