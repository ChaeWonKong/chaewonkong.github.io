---
layout: post
title: "React Native - React Native에서 TypeScript 사용하기"
date: "2019-10-28"
categories:
  - React-Native
excerpt: |
  React Native에서 TypeScript를 사용하고 AirBnB 스타일의 tslint를 적용해본다.
feature_text: |
  ## React Native - React Native에서 TypeScript 사용하기
  React Native에서 TypeScript를 사용하고 AirBnB 스타일의 tslint를 적용해본다.
feature_image: "https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/typescript.png"
image: "https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/typescript.png"
---

오늘은 React Native 프로젝트에 TypeScript를 적용하는 방법을 알아본다.

## React Native 프로젝트 생성

터미널에서 아래 명령을 실행해 MyProject를 생성한다

```shell
react-native init MyProject
```

## TypeScript 적용

다음으로 TypeScript를 설치한다.

```shell
yarn add typescript @types/jest @types/react @types/react-native @types/react-test-renderer
```

루트에 `tsconfig.json` 파일을 생성한다.

```js
{
  "compilerOptions": {
    "allowJs": true,
    "allowSyntheticDefaultImports": true,
    "esModuleInterop": true,
    "isolatedModules": true,
    "jsx": "react",
    "lib": ["es6"],
    "moduleResolution": "node",
    "noEmit": true,
    "strict": true,
    "target": "esnext",
    "resolveJsonModule": true // 이 부분 추가
  },
  "exclude": [
    "node_modules",
    "babel.config.js",
    "metro.config.js",
    "jest.config.js"
  ]
}
```

여기서 특히 확인해야 할 것은 `resolveJsonModule: true` 부분이다. 이 부분이 빠지면 \*.json 형태의 파일 임포트(import)에서 에러가 발생한다.

다음으로 `index.js`와 `App.js`의 확장자를 `*.tsx`로 바꿔준다.

즉, `index.tsx`와 `App.tsx`로 바꿔준다.

만약 jest를 사용한다면 `jest.config.js`파일을 생성하고 아래 내용을 입력해 준다.

```js
module.exports = {
  preset: "react-native",
  moduleFileExtensions: ["ts", "tsx", "js", "jsx", "json", "node"]
};
```

## Lint 적용

다음으로 AirBnB 스타일의 tslint를 적용해 보자.

우리는 `tslint-config-airbnb`라는 모듈을 설치해 사용할 예정이다.

먼저 `tslint.json` 파일을 루트에 생성하고, 아래 내용을 입력해 준다.

```js
{
  "defaultSeverity": "error",
  "extends": ["tslint-config-airbnb", "tslint-react"],
  "jsRules": {},
  "rules": {
    "import-name": [false],
    "variable-name": [
      true,
      "ban-keywords",
      "check-format",
      "allow-pascal-case" // 파스칼 케이스를 허용해야 import React from 'react'에서 React가 소문자 react로 린팅되지 않음
    ],
    "quotemark": [true, "single", "avoid-escape", "avoid-template"]
  },
  "rulesDirectory": []
}
```

다음으로 루트의 `package.json`을 열고 `scripts`에 다음과 같이 lint 옵션을 추가한다.

```js
"scripts": {
		// 기존 scripts 내용
    "lint": "tslint -p tsconfig.json src/**/*.{ts,tsx}" // 린트 옵션 추가(이미 존재한다면 수정하기)
  },
```

이제 `tslint-config-airbnb`와 `tslint`, `tslint-react`를 설치하자.

```shell
yarn add -D tslint tslint-config-airbnb tslint-react
```

`-D` 옵션을 사용해 "devDependency"에 위의 라이브러리들이 추가되도록 한다.

## 확인

이제 초기 환경 세팅은 완료되었다.

`yarn tsc` 명령을 터미널에서 실행하여 정상적으로 프로젝트가 컴파일 되는지 확인한다.

## 참고자료

- [kern yoo, "react-native + typescript — 1. 기본 설정"](https://medium.com/@trustyoo86/react-native-typescript-1-%EA%B8%B0%EB%B3%B8-%EC%84%A4%EC%A0%95-cb875761da7c)
- [Jan Hesters, "Using TypeScript With React Native"](https://medium.com/@jan.hesters/using-typescript-with-react-native-946aa4b4ae6f)
- [Facebook React Native Docs, "Using TypeScript with React Native"](https://facebook.github.io/react-native/docs/typescript)
