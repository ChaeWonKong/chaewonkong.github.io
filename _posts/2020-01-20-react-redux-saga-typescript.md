---
layout: post
title: "React에서 TypeScript로 Redux-Saga 사용하기(예제)"
date: "2020-01-20"
categories:
  - React
excerpt: |
  튜토리얼(예제)을 직접 따라하며 React에서 TypeScript로 Redux-Saga의 사용법을 배워봅니다.
feature_text: |
  ## React에서 TypeScript로 Redux-Saga 사용하기(예제)
  튜토리얼(예제)을 직접 따라하며 React에서 TypeScript로 Redux-Saga의 사용법을 배워봅니다.
feature_image: "https://images.unsplash.com/photo-1507721999472-8ed4421c4af2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
image: "https://images.unsplash.com/photo-1507721999472-8ed4421c4af2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
---

오늘은 React 프로젝트에서 TypeScript를 이용해 Redux-Saga를 사용해 보겠습니다.

우선, 본 포스트는 Redux를 사용해 봤다는 전제 하에 Redux-Saga를 설명합니다. 또, `generator` 를 들어보고 대충은 알고 있다는 전제 하에 설명을 진행합니다.

간단히 설명하며, 실제로 Redux-Saga의 작동을 몸소 체험할 수 있도록 Redux가 구현된 프로젝트를 공유하니 클론(clone)해서 튜토리얼처럼 따라해 보세요.

## Redux-Saga 개요

Redux-Saga는 Redux에서 비동기 처리를 효과적으로 결합할 수 있는 방법을 제시합니다.

Redux에서 사이드이펙트를 효과적으로 관리할 수 있도록 돕는 미들웨어입니다.

Redux-Thunk의 대안이라고도 하죠.

ES6의 `generator` 를 활용하여 비동기 처리를 효과적으로 진행하면서도 actionCreator 함수를 순수함수로 남겨두어 Redux-Thunk와 같은 사이드이펙트간의 엉킴 등이 없습니다.

## 프로젝트 시작하기

우리는 두개의 버튼으로 스코어를 더하고 빼는 어플리케이션을 가지고 Redux-Saga를 연습할 계획입니다.

Redux-Saga를 적용해 버튼이 클릭된 후 2초 뒤에 스코어(score)가 업데이트 되도록 비동기 처리를 진행해 보겠습니다.

[예제 프로젝트 Github 주소](https://github.com/ChaeWonKong/redux-saga-example)

먼저 Redux가 구현되어 있는 예제 프로젝트를 클론합니다.

```shell
git clone https://github.com/ChaeWonKong/redux-saga-example.git
```

클론한 디렉토리로 이동해 패키지 설치를 진행합니다.

```shell
cd redux-saga-example
yarn
```

다음으로 예제 프로젝트가 정상적으로 작동하는지 확인해 봅시다.

```shell
yarn start
```

`http://localhost:3000` 으로 이동해 보면 다음과 같은 화면을 만나실 겁니다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/score.png?raw=true" width="60%" />

버튼을 눌러 보셨나요?

스코어 숫자가 바로 바로 변화하는 것을 확인하실 수 있습니다.

## 프로젝트 소개

지금 설치한 프로젝트는 TypeScript와 Hook을 이용해 Redux를 사용하고 있습니다.

어떤 식으로 구현되어 있는지 간단히 살펴 보도록 할게요.

먼저 `constants.ts` 파일을 볼게요. 이 파일은 Redux에서 사용할 액션 타입들을 상수로 선언하고 사용하기 위해 생성한 파일입니다.

**src/actions/constants.ts**

```typescript
export const SCORE_UP = "SCORE_UP";
export const SCORE_UP_ASYNC = "SCORE_UP_ASYNC";
export const SCORE_DOWN = "SCORE_DOWN";
export const SCORE_DOWN_ASYNC = "SCORE_DOWN_ASYNC";
```

매번 액션을 타이핑해 사용하다 보면 오탈자로 인해 버그가 발생할 수 있어 이렇게 상수로 선언하고 가져다 쓰는 경우가 많죠.

**src/actions/actions.ts**

```typescript
import { SCORE_DOWN, SCORE_UP } from "./constants";

export const scoreUp = () => {
  return { type: SCORE_UP, score: 1 };
};

export const scoreDown = () => {
  return { type: SCORE_DOWN, score: 1 };
};

export type ScoreAction =
  | ReturnType<typeof scoreUp>
  | ReturnType<typeof scoreDown>;
```

다음으로 액션함수를 살펴보겠습니다.

먼저 `SCORE_UP`, `SCORE_DOWN` 을 가져옵니다.

액션함수는 `scoreUp()` 과 `scoreDown()` 두개를 생성해 사용하겠습니다.

여기서 중요한 것은 각 액션함수의 반환 타입(return type)을 타입으로 export 해주는 것입니다. TypeScript를 사용하기 때문에 reducer에서 필요해 집니다.

**src/stores/reducers.ts**

```typescript
import { SCORE_DOWN, SCORE_UP } from "../actions/constants";
import { ScoreAction } from "../actions/actions";

const initialState = {
  score: 0
};

const reducer = (state = initialState, action: ScoreAction) => {
  const newState = { ...state };

  switch (action.type) {
    case SCORE_UP:
      newState.score += 1;
      return newState;
    case SCORE_DOWN:
      newState.score -= 1;
      return newState;
    default:
      return state;
  }
};

export type RootState = ReturnType<typeof reducer>;
export default reducer;
```

다음으로 reducer를 볼게요.

액션 타입과 액션함수들을 가져오구요.

reducer함수는 액션의 타입에 따라 state의 `score` 를 +1 하거나 -1 하게 됩니다.

앞서 **actions/actions.ts** 에서 export했던 `ScoreAction` 을 action의 type으로 넘겨준 것을 확인해 주세요.

`const reducer = (state = initialState, action: ScoreAction) =>{}`

여기서도 중요한 것이 바로 `export type RootState = ReturnType<typeof reducer>;` 이 부분입니다.

TypeScript이기 때문에 추후 Redux-Saga에서 reducer의 반환 타입(return type)이 필요해 집니다.

**src/index.tsx**

```tsx
const store = createStore(rootReducer);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

스토어를 생성하고 **index.tsx**에서 연결하는 것은 보통의 Redux와 같습니다.

다음으로 커스텀 훅입니다.

**src/hooks/useScore.ts**

```typescript
import { useSelector, useDispatch } from "react-redux";
import { RootState } from "../stores/reducers";
import { scoreDown, scoreUp } from "../actions/actions";
import { useCallback } from "react";

export default function useScore() {
  const score = useSelector((state: RootState) => state.score);
  const dispatch = useDispatch();

  const onScoreUp = useCallback(() => dispatch(scoreUp()), [dispatch]);
  const onScoreDown = useCallback(() => dispatch(scoreDown()), [dispatch]);

  return { score, onScoreUp, onScoreDown };
}
```

`useScore()` 라는 커스텀 훅을 생성하여 상태 관리와 프레젠테이션 컴포넌트를 최대한 분리했습니다.

사용방법은 [Velopert, "리액트 컴포넌트 타입스크립트로 작성하기"](https://velog.io/@velopert/create-typescript-react-component) 을 참고했습니다.

## Redux-Saga 도입하기

이제 Redux-Saga를 도입해 보겠습니다.

먼저 src 디렉토리에 **sagas** 라는 디렉토리를 생성합니다.

그리고 다음 코드를 작성하겠습니다.

**src/sagas/saga.ts**

```typescript
import { takeEvery, put, delay, all } from "redux-saga/effects";
import {
  SCORE_DOWN_ASYNC,
  SCORE_UP_ASYNC,
  SCORE_DOWN,
  SCORE_UP
} from "../actions/constants";

function* scoreUpAsync() {
  yield delay(2000);
  yield put({ type: SCORE_UP_ASYNC, score: 1 });
}

function* scoreDownAsync() {
  yield delay(2000);
  yield put({ type: SCORE_DOWN_ASYNC, score: 1 });
}

function* watchScoreUp() {
  yield takeEvery(SCORE_UP, scoreUpAsync);
}

function* watchScoreDown() {
  yield takeEvery(SCORE_DOWN, scoreDownAsync);
}

export default function* rootSaga() {
  yield all([watchScoreUp(), watchScoreDown()]);
}
```

먼저 `scoreUpAsync()`와 `scoreDownAsync()` 함수는 제너레이터(generator)를 이용해 2초의 딜레이 후 액션 객체를 반환합니다.

다음으로 `watchScoreUp()`과 `watchScoreDown()`은 기존에 우리가 작성한 **src/actions/actions.ts**의 액션함수(action creators)가 실행되어 액션 객체가 리턴되면 reducer로 전달되기 직전에 해당 액션객체를 가로채 `scoreUpAsync()`와 `scoreDownAsync()` 함수를 실행합니다.

`takeEvery()`는 모든 async 요청을 반영하도록 하는 헬퍼 함수입니다. 우리의 경우 버튼을 연속해 클릭하면 클릭한 횟수만큼 반영이 되는 셈입니다.

반면에 `takeLatest()`를 사용하면 버튼을 연속해 클릭해도 가장 마지막 클릭만 반영됩니다.

마지막으로 `rootSaga()`는 `watchScoreUp()`과 `watchScoreDown()` 을 묶어서 실행해 줍니다.

제너레이터 함수를 이용함으로써, 우리는 async 작업이 끝날 때 우리에게 필요한 액션 객체를 reducer로 전달할 수 있게 되었습니다.

만약 이것이 Ajax 요청이었다면 response를 받은 후 데이터가 액션 객체의 payload 형태로 reducer로 전달되어 상태값이 변경되었을 것입니다.

다음으로 reducer를 조금 수정해 보겠습니다.

**src/stores/reducers.ts**

```typescript
import { SCORE_DOWN_ASYNC, SCORE_UP_ASYNC } from "../actions/constants";
import { ScoreAction } from "../actions/actions";

const initialState = {
  score: 0
};

const reducer = (state = initialState, action: ScoreAction) => {
  const newState = { ...state };

  switch (action.type) {
    case SCORE_UP_ASYNC:
      newState.score += 1;
      return newState;
    case SCORE_DOWN_ASYNC:
      newState.score -= 1;
      return newState;
    default:
      return state;
  }
};

export type RootState = ReturnType<typeof reducer>;
export default reducer;
```

여기서 주목할 부분은 액션객체의 type 부분입니다. `SCORE_UP`은 `SCORE_UP_ASYNC`로, `SCORE_DOWN`은`SCORE_DOWN_ASYNC`로 변경해 주었습니다.

앞서 만든 **src/sagas/saga.ts** 파일에서 우리는 async 작업이 완료되면 새 액션객체를 리턴했습니다.

당연히 reducer는 가로채어져서 변경된 새 액션객체를 기준으로 동작해야 하므로 액션의 타입을 위처럼 변경해줬습니다.

마지막으로 우리가 생성한 스토어(store)에 Redux-Saga를 추가해주겠습니다.

**src/index.tsx**

```tsx
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import * as serviceWorker from "./serviceWorker";

import { createStore, applyMiddleware } from "redux";
import { Provider } from "react-redux";
import rootReducer from "./stores/reducers";
import rootSaga from "./sagas/saga";
import createSagaMiddleware from "redux-saga";

const sagaMiddleware = createSagaMiddleware();

const store = createStore(rootReducer, applyMiddleware(sagaMiddleware));

sagaMiddleware.run(rootSaga);

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);

serviceWorker.unregister();
```

우선 **src/sagas/saga**에서 `rootSaga`를, **redux-saga**에서 `createSagaMiddleware`를, **redux**에서 `applyMiddleware` 를 import합니다.

그리고 `sagaMiddleware`를 생성하고 `store`에 추가해 줍니다.

마지막으로 `rootSaga`를 매개변수로 `sagaMiddleware.run(rootSaga);`를 호출해 줍니다.

**드디어, 완성입니다.**

이제 프로젝트를 로컬에서 실행해 보면, 버튼을 눌러도 score가 바로 올라가지 않고 2초간 기다렸다 변화하는 것을 확인할 수 있습니다!

## 마치며

만약 기존의 순수한 Redux 코드와 Redux-Saga를 적용한 코드를 비교해 보고 싶다면 브랜치를 변경해 확인하세요.

- `start` 브랜치는 초기 프로젝트처럼 Redux 만으로 코드가 구성되어 있습니다.
- `complete` 브랜치에서는 Redux-Saga가 적용되어 완성된 코드를 확인할 수 있습니다.

**초기 상태로 돌려서 코드 보기**

```shell
git checkout start
```

**Redux-Saga 적용된 완성본 코드 보기**

```shell
git checkout complete
```

## 참고자료

- [Velopert, "리액트 컴포넌트 타입스크립트로 작성하기"](https://velog.io/@velopert/create-typescript-react-component)
- [Redux-Saga 공식 사이트](https://redux-saga.js.org/)
- [redux-saga로 비동기처리와 분투하다.](https://github.com/reactkr/learn-react-in-korean/blob/master/translated/deal-with-async-process-by-redux-saga.md)
