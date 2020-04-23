---
layout: post
title: "Jest와 Enzyme을 이용한 React 커스텀 훅(custom hook) 테스팅(test)"
date: "2020-04-19"
categories:
  - React
excerpt: |
  Jest와 Enzyme을 이용해 TypeScript 기반의 React에서 API 요청을 담당하는 Custom Hook을 테스트하는 방법들을 알아본다.
feature_text: |
  ## Jest와 Enzyme을 이용한 React 커스텀 훅(custom hook) 테스팅(test)
  Jest와 Enzyme을 이용해 TypeScript 기반의 React에서 API 요청을 담당하는 Custom Hook을 테스트하는 방법들을 알아본다.
feature_image: "https://images.unsplash.com/photo-1547110543-a2af5c37f597?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
image: "https://images.unsplash.com/photo-1547110543-a2af5c37f597?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
---

React에서 Hook을 사용하면 많은 것들이 편리해 진다.
하지만, **Custom Hook을 사용해 API 요청을 하는 경우, 테스트 코드를 작성하는 것이 쉽지많은 않다.**

API 요청은 mocking하지 않으면 테스트를 하는 것이 곤란하다.
**테스트코드는 네트워크의 상황이나 서버의 상태와 독립적으로 작동해야 한다. 우리가 테스트하고 싶은 것은 우리가 작성한 코드의 품질이기 때문이다.**

오늘 살펴볼 주제는 바로 **TypeScript로 작성한 React 프로젝트에서 API 요청을 담당하는 Custom Hook을 테스트하는 방법이다.**
React의 테스트를 위해 흔히 사용되는 **Jest와 Enzyme**을 사용해 그 방법을 알아볼 예정이다.

우리가 테스트할 Custom Hook에서 사용하는 API는 다음과 같다고 하자.

## 목표

### API 명세

우리가 테스트하려는 Custom Hook에서는 아래 API를 호출한다.

```tsx
interface Article {
  id: number;
  title: string;
  body: string;
  created_at: Date;
  updated_at: Date;
  background_image: string;
}

export const getArticle = (id: number): Article => {
  const res = await axios.get(`/article/${id}`);
  return res.data;
};
```

### 테스트하려는 Custom Hook

우리가 Test할 React Custom Hook은 아래와 같다.

```tsx
import { useState, useEffect } from "react";
import { getArticle } from "api";

export const useFetchArticle = (id: number) => {
  const [article, setArticle] = useState();
  const fetchArticle = async () => {
    const data = await getArticle(id);
    setArticle(data);
  };

  useEffect(() => {
    fetchArticle();
  }, []);

  return { article };
};
```

우리의 Custom Hook은 실행될 때 `fetchArticle`이라는 함수를 호출한다.
`fetchArticle`은 비동기적으로 `getArticle`을 호출하여 API 요청을 진행하고 response를 `useState`를 이용해 `article`에 저장한다.
우리의 커스텀 훅은 `{ article }`을 리턴한다.

## 의존 라이브러리

우리는 Jest와 Enzyme, testing-library/react-hook 이 세 가지를 이용할 것이다.
우선 Jest와 Enzyme을 간단하게 사용할 줄 안다는 가정하에 진행한다.

[testing-library/react-hook](https://react-hooks-testing-library.com/)에서 자세한 문서는 확인하자.

설치는 아래와 같다.

```
npm install --save-dev @testing-library/react-hooks
```

## 테스트 코드

### Mock Data 생성

**useFetchArticle.test.ts**

```tsx
import { renderHook } from "@testing-library/react-hooks";
import * as api from 'api';
import { useFetchArticle } from 'useFetchArticle';

const mockData = {
  id: 1;
  title: 'title';
  body: 'body';
  created_at: '2020-04-21T11:31:55.020Z';
  updated_at: '2020-04-21T11:31:55.020Z';
  background_image: 'image';
}

const mockID = 1234;
```

먼저 필요한 파일들을 import 하고, api 요청을 mocking하기 위한 더미데이터인 `mockData`를 만든다.

### API 호출을 Mocking하는 함수 작성

다음으로 테스트를 위한 블록을 생성하는데, 각 테스트 실행 전 호출될 api를 mocking하여 mockData를 반환하도록 해준다.

`mockID`는 `useFetchArticle`에 매개변수로 넘겨주려는 것으로, 사실 어떤 것으로 설정하든 상관은 없다.

```typescript
describe("Test for `useFetchArticle`", () => {
  beforeEach(() => {
    jest.spyOn(api, 'getArticle').mockImplementation(() => mockData)
  })
```

`jest.spyOn()`은 첫번째 매개변수로 object를 받고 두번째 매개변수로 받은 object의 메서드명을 string으로 받아 mock function을 만든다.
(jest mock function을 리턴한다)

이것을 `mockImplementation()` 이라는 메서드로 받아 mockData를 반환하게 하는 것이다.

이로써 `api.getArticle()`을 mocking하여 mockData를 리턴하도록 수정하게 되었다.

자세한 내용은 [jest 문서](https://jestjs.io/docs/en/jest-object)를 확인하자.

### 테스트 케이스 작성

다음으로 테스트 케이스를 작성한다.

```tsx
it("returns article correctly", async () => {
  const { result, waitForNextUpdate } = renderHook(useFetchArticle);
  await waitForNextUpdate();

  expect(result.current.article).toStrictEqual(mockData);
});
```

우리의 테스트 케이스는 3가지 로직으로 작성된다.

1. 첫번째 줄에서 실제처럼 `useFetchArticle`이 작동하여 결과값을 반환하게 한다.
2. 두번째 줄에서 `await waitForNextUpdate();`를 통해 비동기적으로 실행될 `useFetchArticle`내의 `useEffect`가 실행완료되길 기다린다.
3. 세번째 줄에서 결과값이 우리가 예상한 값과 같은지 비교한다.

참고로 `renderHook`은 `result.current` 객체는 렌더링된 커스텀 훅의 return 값을 포함한다.
우리는 `useFetchArticle`에서 `{ article }`을 리턴했기 때문에 `result.current.article`로 접근하여 가져올 수 있는 것이다.

### 전체 코드

우리가 작성한 전체 테스트 코드는 아래와 같다.

```tsx
import { renderHook } from "@testing-library/react-hooks";
import * as api from 'api';
import { useFetchArticle } from 'useFetchArticle';

const mockData = {
  id: 1;
  title: 'title';
  body: 'body';
  created_at: '2020-04-21T11:31:55.020Z';
  updated_at: '2020-04-21T11:31:55.020Z';
  background_image: 'image';
}

const mockID = 1234;

describe("Test for `useFetchArticle`", () => {
  beforeEach(() => {
    jest.spyOn(api, 'getArticle').mockImplementation(() => mockData)
  })

  it('returns article correctly', async () => {
    const { result, waitForNextUpdate } = renderHook(useFetchArticle);
    await waitForNextUpdate()

    expect(result.current.article).toStrictEqual(mockData)
  })
});
```

테스트를 실행해 정상적으로 동작하는지 확인한다.

## 결론

Jest와 Enzyme은 훌륭한 테스트 도구지만, React의 함수형 컴포넌트를 제한적으로 지원한다는 것이 안타깝다.
특히 Hook, Custom Hook의 테스트가 쉽지 않다.

위 방법을 이용하면 Custom Hook에서 API 요청을 할 경우, 그 함수를 mocking함으로써 Custom Hook을 테스트할 수 있다.
