---
layout: post
title: "Jest와 Enzyme을 이용한 React 커스텀 훅(custom hook) 테스팅(test)"
date: "2020-04-19"
categories:
  - React
excerpt: |
  Jest와 Enzyme을 이용해 TypeScript 기반의 React에서 Custom Hook을 테스트하는 방법들을 알아본다.
feature_text: |
  ## Jest와 Enzyme을 이용한 React 커스텀 훅(custom hook) 테스팅(test)
  Jest와 Enzyme을 이용해 TypeScript 기반의 React에서 Custom Hook을 테스트하는 방법들을 알아본다.
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

```tsx
interface Article {
  id: number;
  title: string;
  body: string;
  created_at: Date;
  updated_at: Date;
  background_image: string;
}

const getArticle = (id: number): Article => {
  const res = await axios.get(`/article/${id}`);
  return res.data;
};
```

우리가 Test할 React Custom Hook은 아래와 같다.

```react
import {useState, useEffect} from 'react'

const getArticle = () => {

}
```
