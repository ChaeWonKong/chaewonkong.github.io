---
layout: post
title: "Lerna, React, TypeScript를 활용한 모노리포 세팅(번역)"
date: "2020-02-11"
categories:
  - JavaScript
excerpt: |
  Lerna, React, TypeScript를 활용한 모노리포 세팅(번역)
feature_text: |
  ## Lerna, React, TypeScript를 활용한 모노리포 세팅(번역)
  Lerna를 활용해 React, TypeScript를 사용하는 모노리포를 세팅하는 방법을 알아봅니다.
feature_image: "https://miro.medium.com/max/3188/1*LpiRLeQ4szeN4kvsv8G50A.png"
image: "https://miro.medium.com/max/3188/1*LpiRLeQ4szeN4kvsv8G50A.png"
---

> 이 글은 [Jannik Buschke](https://medium.com/@sisosys7)의 [**"A monorepo setup with lerna, react and typescript"**](https://medium.com/@sisosys7/a-monorepo-setup-with-lerna-react-and-typescript-7b912fb48622)을 번역한 글입니다.

**tl;dr:** 이 글은 단계별로 React와 TypeScript 모듈을 사용하는 Lerna 기반의 모노리포 셋팅 방법을 살펴봅니다.
만약 단계별로 따라가며 사용법을 익히고 싶지 않다면 아래 레포지토리를 클론해서 스스로 살펴봐도 됩니다.

```shell
git clone https://github.com/jannikbuschke/lerna-react-typescript-sample
cd lerna-react-typescript-sample
npm install
npm run bootstrap
npm run watch
npm run start (두번째 터미널에서 실행)
```

이제 `/packages/my-module/src/HelloWorld.tsx`을 수정하고 http://localhost:3000 에서 라이브로 변경사항을 확인하면 됩니다.

그럼 이제 단계별로 살펴보겠습니다.
<br /><br />
![main](https://miro.medium.com/max/4608/1*xf2rdYux0WcMu6--nbiTiw.jpeg)

## Lerna 설치 및 생성

```shell
mkdir lerna-test
cd lerna-test
npm install lerna -g
lerna init
```

위 명령어를 실행하면 깃 리포지토리와 우리의 앱과 모듈을 위한 빈 packages 폴더(Lerna 컨벤션이지만 필수는 아님), 앱과 모듈을 트래킹하는 lerna.json, 그리고 root의 dependency를 기록하는 package.json이 생성됩니다.

```
.git/
packages/
lerna.json
package.json
```

## 앱 생성하기

이제 그 유명한 create-react-app을 사용해 React 앱을 만들어 봅시다.

`npx create-react-app packages/my-app`

## 타입스크립트 패키지 생성

폴더를 생성합니다.

`mkdir packages/my-module`

다음으로 `package.json`을 `/packages/my-module/` 디렉토리 안에 다음과 같은 내용으로 생성합니다.

```json
{
  "name": "my-module",
  "version": "0.1.0",
  "private": false,
  "files": ["lib"],
  "main": "./lib/index.js",
  "types": "./lib/types.d.ts",
  "scripts": {
    "tsc": "tsc"
  },
  "dependencies": {},
  "peerDependencies": {
    "react": ">= 16.8.0 < 17",
    "react-dom": ">= 16.8.0 < 17"
  },
  "devDependencies": {
    "@types/jest": "24.0.11",
    "@types/node": "11.13.0",
    "@types/react-dom": "16.8.3",
    "tslib": "1.9.3",
    "@types/react": "16.8.10",
    "typescript": "3.4.1",
    "react": "16.8.6",
    "react-dom": "16.8.6"
  }
}
```

`/packages/my-module/tsconfig.json` 으로 TypeScript 설정 파일을 생성하고 아래 내용을 추가합니다.

```json
{
  "compilerOptions": {
    "baseUrl": ".",
    "outDir": "./lib",
    "module": "esnext",
    "target": "es5",
    "lib": ["es6", "dom"],
    "sourceMap": true,
    "jsx": "react",
    "skipLibCheck": true,
    "moduleResolution": "node",
    "rootDir": "src",
    "forceConsistentCasingInFileNames": true,
    "noImplicitReturns": true,
    "noImplicitThis": true,
    "noImplicitAny": true,
    "importHelpers": true,
    "strictNullChecks": true,
    "suppressImplicitAnyIndexErrors": true,
    "noUnusedLocals": false,
    "declaration": true,
    "declarationMap": true,
    "allowSyntheticDefaultImports": true
  },
  "exclude": [
    "lib",
    "node_modules",
    "build",
    "scripts",
    "acceptance-tests",
    "webpack",
    "jest",
    "src/setupTests.ts"
  ]
}
```

앱 안에서 사용할 `/packages/my-module/src/HelloWorld.tsx` 라는 예제 컴포넌트를 생성합니다.

```tsx
import * as React from "react";
export const HelloWorld = () => <div>HelloWorld</div>;
```

앱 안에서 모듈 컴포넌트들을 사용하기 위해 export 해줍니다.
아래의 코드로 `/packages/my-module/src/index.ts` 파일을 생성합니다.

```tsx
export * from "./HelloWorld";
```

타입을 만들어 쓰기 위해 `/packages/my-module/src/types.ts` 파일을 생성합니다.

```typescript
export * from "./index";
```

## 앱에서 로컬 모듈 참조

`/packages/my-app/package.json` 의 "depenencies" 부분에 우리가 생성한 모듈에 대한 참조(reference)를 추가합니다. 마치 이미 배포한 것처럼요.

```json
"dependencies": {
    "my-module": "0.1.0",
    "react": "^16.8.6",
    "react-dom": "^16.8.6",
    "react-scripts": "2.1.8"
  },
```

루트 디렉토리로 돌아가 아래 명령을 실행해 줍니다.

`lerna bootstrap --hoist`

실행하면 Lerna는 dependency를 설치하고 모듈 간 symlink를 생성합니다. <u>--hoist</u> 플래그는 공통된 dependency들(예: 여러 패키지가 공유하는 공통 라이브러리)을 (dependency들에 symlink를 추가하며) 루트의 node_modules 폴더로 옮겨 중복을 제거합니다.

다음으로 루트 디렉토리에서 아래 명령을 실행하세요.
<br />
`lerna run tsc`

TypeScript 컴파일을 진행하기 위해서요.
TypeScript 컴파일이 동작하기 위해서는 `npm i typescript -g` 명령을 사용해 전역에서 TypeScript를 설치해야 할 수도 있습니다.

`lerna run <스크립트>` 를 실행하면 Lerna는 어떤 패키지에서든 해당 `<스크립트>`가 (package.jsondp npm 스크립트 형태로) 정의된 것을 찾아 실행해 줍니다.

## my-module 컴포넌트를 앱에서 사용하기

`/packages/my-app/src/App.js` 파일의 확장자를 `.tsx`로 바꾸고 내용을 아래 코드로 변경해 줍니다.

```tsx
import * as React from "react";
import { HelloWorld } from "my-module";
function App() {
  return (
    <div>
      <HelloWorld />
    </div>
  );
}
export default App;
```

이제 실행해볼 시간입니다.
<br />
`lerna run start`

이 스크립튼는 모든 패키지들의 `start`라고 이름붙여진 모든 스크립트를 실행할 겁니다. 우리 예제의 경우 `my-app` 패키지가 해당하는 명령어를 가지고 있죠. http://localhost:3000 을 열고 HelloWorld 컴포넌트가 잘 보이는지 확인해 보세요.

## 모듈 업데이트 하기

우리는 모듈을 수정하면 앱이 그것을 바로 반영하길 원하죠. npm 스크립트를 추가해서 파일이 변경되면 컴파일이 자동으로 진행되게 해봅시다. `/packages/my-module/package.json` 파일의 스크립트 섹션에 아래와 같은 스크립트를 추가해 줍니다.

`"watch": "tsc --watch"`

두번째 터미널의 루트 디렉토리에서 `lerna run watch`를 실행해 보세요.
이제 `HelloWorld` 컴포넌트가 변경되면 우리 앱은 자동으로 이를 반영합니다.

## 헬퍼 스크립트 추가

좀 더 편리하게 쓰기 위해 root의 package.json에 몇가지 스크립트를 추가해 줍시다.

```json
{
  "name": "root",
  "private": true,
  "devDependencies": {
    "lerna": "3.13.1"
  },
  "scripts": {
    "bootstrap": "lerna bootstrap --hoist",
    "start": "lerna run start --stream",
    "tsc": "lerna run tsc --stream",
    "watch": "lerna run watch --stream"
  }
}
```

`--stream` 플래그는 명령의 결과를 지금 쓰는 터미널에서 확인할 수 있게 해줍니다.

이제 우리는 다음과 같이 root 디렉토리의 npm을 이용해서 명령들을 실행할 수 있습니다.

```shell
npm run bootstrap
npm run tsc
npm run watch
npm run start
```

## Github 예제

설명된 예제는 Github에서 확인할 수 있습니다.
<br />
[https://github.com/jannikbuschke/lerna-react-typescript-sample](https://github.com/jannikbuschke/lerna-react-typescript-sample)

## 한계

종종 우리가 한 세팅은 말썽을 일으킬 수 있습니다(새 파일을 생성하거나 삭제하는 경우, 혹은 모듈로부터 새 타입을 export하는 경우). 그 때마다 우리는 watch 명령을 재실행 해줘야 합니다. 에디터가 가끔 새로 export된 타입을 발견하지 못하는 경우도 발생하죠.

이런 경우에 다시 컴파일하도록 재시작 하거나 IDE를 껐다 켜는 것이 도움이 될 수 있습니다. 종종 node_modules 폴더를 정리해 주는 것이 도움이 될 때도 있죠(특히 서드파티 dependency를 업데이트 하는 경우).

## 원문

- [https://medium.com/@sisosys7/a-monorepo-setup-with-lerna-react-and-typescript-7b912fb48622](https://medium.com/@sisosys7/a-monorepo-setup-with-lerna-react-and-typescript-7b912fb48622)
