---
layout: post
title: "React Native - React Native에 TypeScript로 Redux 적용하기"
date: "2019-11-11"
categories:
  - React-Native
excerpt: |
  TypeScript로 작업 중인 React Native 프로젝트에 TypeScript 기반의 Redux를 도입하는 방법을 알아본다.
feature_text: |
  ## React Native - React Native에 TypeScript로 Redux 적용하기
  TypeScript로 작업 중인 React Native 프로젝트에 TypeScript 기반의 Redux를 도입하는 방법을 알아본다.
feature_image: "https://miro.medium.com/max/2048/1*zsgKJMIjtEEky4q58jUCfg.png"
image: "https://miro.medium.com/max/2048/1*zsgKJMIjtEEky4q58jUCfg.png"
---

React Native 프로젝트에 TypeScript로 Redux를 적용해보자.

## 목차

1. [목표](#0)
2. [준비 사항 및 참고 사항](#1)
3. [Redux 설치하기](#2)
4. [관련 디렉토리 생성하기](#3)
5. [Actions](#4)
6. [Reducers](#5)
7. [Redux를 활용해 API로 가져온 posts 저장하기](#6)
8. [참고자료](#7)

<p id="0" />
## 1. 목표

이번 포스트의 목표는 TypeScript를 이용해 React Native 프로젝트에 Redux를 구현하는 것이다.

**데이터(유튜브 영상의 목록)를 API로 가져와서 Redux 스토어(store)에 저장하는 기능을 구현**해보기로 한다.

<p id="1" />
## 2. 준비 사항 및 참고 사항

- Lodash 설치(deep clone을 쉽게 사용하기 위해 필요)
- TypeScript가 적용된 React Native 프로젝트(["React Native에서 TypeScript 사용하기" 포스트 보기](https://chaewonkong.github.io/posts/typescript-with-react-native.html))
- 프로젝트 디렉토리 구성: 개략적인 프로젝트 구성은 아래와 같다. actions, reducers 디렉토리는 본 포스트를 따라가며 생성해 나갈 것이다.

```
android
ios
index.tsx
node_modules
. . .
src
├── actions
│   ├── index.ts
│   └── types.ts
├── api
│   └── index.ts
├── components
│   ├── App.tsx
│   ├── Header.tsx
│   └── List.tsx
├── reducers
│   ├── index.ts
│   └── postReducer.ts
└── shared-interfaces.ts
```

- 예시 API Response: posts는 아래 오브젝트와 같은 구조의 오브젝트들이 리스트 형태로 들어있다.

```json
{
  "posts": [
    {
      "id": 1,
      "title": "말좀 예쁘게하세요",
      "content": "예쁘게예쁘게 ^^",
      "video_id": "SJUsz8HV5mQ",
      "author": "Leon"
    },
  ...
  ]
}
```

- shared-interfaces.ts

```ts
export interface ISimplePost {
  id: string | number;
  title: string;
  video_id: string;
  author: string;
}

export interface IPosts {
  posts: ISimplePost[];
}
```

<p id="2" />
## 3. Redux 설치하기

```shell
yarn add redux react-redux
yarn add --dev @types/redux @types/react-redux
```

@types로 설치하는 라이브러리는 type definition을 추가해 주기 위해 설치하는 것이므로 dev-dependency로 설치한다.
**react-redux는 React를 위한 공식 redux 바인딩이다.**

<p id="3" />
## 4. 관련 디렉토리 생성하기

다음으로 필요한 디렉토리를 생성하자. src 디렉토리에 하위 디렉토리로 actions와 reducers 디렉토리를 각각 생성한다.
actions에는 Action Creator가, reducers에는 Reducer가 들어갈 것이다.

<p id="4" />
## 5. Actions

### actions/types.ts

먼저 type을 지정하는 파일을 생성하자.
actions/types.ts 파일을 생성한다.
여기서 **type은 reducer에서 action을 처리할 때, 그 action의 종류를 한 번에 식별할 수 있는 String 혹은 Symbol**을 의미한다.

```ts
import { IPosts, ISimplePost } from "../shared-interfaces";

export enum POSTS_ACTION_TYPES {
  UPDATE_POSTS = "POSTS/UPDATE_POSTS",
  GET_POSTS = "POSTS/GET_POSTS"
}

export type GetPostAction = {
  type: string;
  payload: IPosts;
};

export type PostListState = {
  posts: ISimplePost[];
};

export type AppState = {
  postList: PostListState;
};
```

POSTS_ACTIONS_TYPES는 enum으로 선언하되 String을 넣어주었다. Redux의 type은 반드시 String 타입이어야 한다(Number 타입이면 안됨).

GetPostAction은 action의 type을 지정해주기 위한 타입이다.

PostListState는 추후 작성할 Reducer에서 사용할 initialState의 타입이다.

AppState는 List.tsx 컴포넌트에서 사용될 타입이다. 추후 등장하면 다시 설명한다.

### actions/index.ts

actions 폴더 안에 index.ts 파일을 생성한다.

```ts
import { GetPostAction, POSTS_ACTION_TYPES } from "./types";
import { IPosts } from "../shared-interfaces";

export const getPosts = (posts: IPosts): GetPostAction => ({
  type: POSTS_ACTION_TYPES.GET_POSTS,
  payload: posts
});
```

우리는 getPosts라는 Action Creator를 정의할 것이다. 이 action은 posts를 Payload로 Reducer에 넘긴다.

여기서 다시 한번 type과 payload에 대해 짚고 넘어가자.
**type은 reducer에서 action을 처리할 때, 그 action의 종류를 한 번에 식별할 수 있는 String 혹은 Symbol이다.**
**payload는 action의 실행을 위해 제공되는 데이터를 담는 Object이다.**

우리가 만든 getPosts Action Creator는 POSTS_ACTION_TYPES.GET_POSTS을 type으로, posts를 payload로 리턴한다.

<p id="5" />
## 6. Reducers

다음으로 Reducer를 작성할 차례이다. Reducer 함수는 순수 함수여야 한다. 이것만 기억하고 진행하자.

### reducers/index.ts

index.ts에서는 추후 Reducer의 개수가 늘어날 경우를 대비해 Reducer들을 묶어서 내보낼 수 있도록 설정해두자.
(물론 지금은 1개의 Reducer만 존재하므로 아직은 불필요한 기능이다)

```ts
import { combineReducers } from "redux";
import { postList } from "./postReducer";

export default combineReducers({
  postList
});
```

### postReducer.ts

다음으로 우리의 첫 Reducer인 postReducer를 작성해보자.

```ts
import {
  PostListState,
  GetPostAction,
  POSTS_ACTION_TYPES
} from "../actions/types";
import _ from "lodash";

export const initalState: PostListState = { posts: [] };

export const postList = (
  state: PostListState = initalState,
  action: GetPostAction
) => {
  const newState: PostListState = _.cloneDeep(state);
  switch (action.type) {
    case POSTS_ACTION_TYPES.GET_POSTS:
      return { ...initalState, ...action.payload };
    default:
      return newState;
  }
};
```

먼저 initialState는 상태의 초깃값을 선언하기 위한 변수이다.

우리가 작성할 Reducer 함수의 이름은 postList이다. postList는 state와 action을 매개변수로 받는데 state에는 디폴트로 앞서 선언한 initialState가 들어있다.

action에는 앞서 설명했듯 type과 payload가 들어있다.

**Reducer에서는 보통 switch문을 사용해서 action의 type에 따라 전역 스토어의 상태를 업데이트 한다.** 즉, **각 action.type의 케이스 별로 다른 return**을 하게 되는데, 여기서 return 하는 Object가 Redux의 store에 저장되는 것이다.

### store 추가하기

자, 이제 store를 생성하고 우리 프로젝트에 추가할 단계다.
src/components/App.tsx를 열자.
여기서 App.tsx는 루트의 index.tsx에서 임포트하는 가장 상위 컴포넌트다.

#### Before

```tsx
import React from "react";
import Navigator from "../Navigator";

const store = createStore(reducer);

const App = () => {
  return <Navigator />;
};
```

#### After

```tsx
import React from "react";
import { createStore } from "redux";
import { Provider } from "react-redux";
import reducer from "../reducers";
import Navigator from "../Navigator";

const store = createStore(reducer);

const App = () => {
  return (
    <Provider store={store}>
      <Navigator />
    </Provider>
  );
};
```

Navigator는 react-navigation과 관련된 것이므로 일단 신경쓰지 말자.

createStore는 우리가 이전에 작성한 Reducer로 store를 만들어주는 함수다.

핵심은 이것이다. **store를 선언하고 Provider에 store를 prop으로 넘긴 후 그 Provider로 기존의 App 컴포넌트를 감싼다.**
과정이 헷갈릴 수 있을까봐 Before와 After를 구분했다. 우리의 코드는 After와 같은 형태가 되어야 한다.

<p id="6" />
## 7. Redux를 활용해 API로 가져온 posts 저장하기

이제 우리가 구현한 Redux store를 사용해볼 차례다.

### src/components/List.tsx

먼저 간단히 설명하자면, Hooks를 사용한 Functional Component 형태로 List.tsx를 구성할 것이다.

List.tsx는 유튜브 영상 목록을 가져와 리스트 뷰로 뿌려준다. 원래는 스타일 등 더 복잡한 코드이지만,
Redux의 작동을 살피는 것이 목적이므로 코드는 간단하게 첨부했다.

우리가 작성하고자 하는 최종 코드는 아래와 같다.

```tsx
import React from "react";
import { View, Text, ScrollView } from "react-native";
import { ISimplePost } from "../shared-interfaces";
import { getPosts } from "../actions";
import { connect } from "react-redux";
import { bindActionCreators } from "redux";
import { AppState } from "../actions/types";
import { tubesdayApi } from "../api";

interface IProps {
  onGetPosts: typeof getPosts;
  children: React.ReactNode;
}

const List: React.FC<IProps> = props => {
  const [posts, setPosts] = React.useState();

  React.useEffect(() => {
    getPosts();
  }, []);

  const getPosts = async (): Promise<void> => {
    const res = await tubesdayApi.getAllPosts();
    const posts = res.data;
    props.onGetPosts(posts);
    setPosts(posts);
  };

  function renderVideoList() {
    return posts.map((item: ISimplePost, idx: number) => {
      return (
        <View>
          <Text>{item.title}</Text>
          <Text>{item.author}</Text>
        </View>
      );
    });
  }

  return (
    <View>
      <ScrollView>{posts ? renderVideoList() : null}</ScrollView>
    </View>
  );
};

const mapStateToProps = (state: AppState) => ({
  posts: state.postList
});

const mapDispatchToProps = (dispatch: any) => ({
  onGetPosts: bindActionCreators(getPosts, dispatch)
});

export default connect(
  mapStateToProps,
  mapDispatchToProps
)(List);
```

이제 위 코드를 조금 더 자세히 살펴보자.

```tsx
import { ISimplePost } from "../shared-interfaces";
import { getPosts } from "../actions";
import { connect } from "react-redux";
import { bindActionCreators } from "redux";
import { AppState } from "../actions/types";
```

먼저 각 포스트의 인터페이스인 ISimplePost를 가져온다. posts를 map으로 순회하며 처리할 때 각각의 item을 지칭할 타입이다.

다음으로 앞서 만든 **getPosts** Action Creator를 가져온다. mapDispatchToProps 함수에서 dispatch와 getPosts를 바인드 해줄 것인데,
이로써 List 컴포넌트에서 해당 Action Creator를 이용해 스토어에 정보를 넣거나 가져올 수 있게 된다.

다음으로 connect를 가져온다. **React 컴포넌트가 Redux의 흐름과 연결되기 위해서는 React Redux의 connect라는 함수를 사용해 연결해 줘야만 한다.**

```tsx
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(List);
```

추후 이런식으로 List 컴포넌트를 Redux의 흐름과 연결할 것이다.

**bindActionCreators**는 Action의 생성부터 Dispatch의 실행까지 한번에 이뤄질 수 있게 돕는 함수다.

마지막으로 mapStateToProps에서 state의 타입을 지정하기 위해 actions/type.ts에서 AppState라는 타입을 가져왔다.

```tsx
const mapStateToProps = (state: AppState) => ({
  posts: state.postList
});

const mapDispatchToProps = (dispatch: any) => ({
  onGetPosts: bindActionCreators(getPosts, dispatch)
});
```

다음으로 볼 부분은 이 두 함수이다. **mapStateToProps는 Redux 스토어에 저장된 상태(state)를 props 형태로 React 컴포넌트에 전달한다.** 이를 통해 **컴포넌트 안에서 Redux 스토어의 데이터에 접근할 수 있게 된다.**

**mapDispatchToProps는 Action Creator를 props로 React 컴포넌트에 전달한다.** 이를 통해서 **Action을 생성하고 Redux 스토어에 저장된 데이터를 변경할 수 있게 된다.**

```tsx
export default connect(
  mapStateToProps,
  mapDispatchToProps
)(List);
```

이제 connect를 이용해서 컴포넌트와 Redux를 연결해 준다.

```tsx
interface IProps {
  onGetPosts: typeof getPosts;
  children: React.ReactNode;
}

const List: React.FC<IProps> = props => {
  const [posts, setPosts] = React.useState();

  React.useEffect(() => {
    getPosts();
  }, []);

  const getPosts = async (): Promise<void> => {
    const res = await tubesdayApi.getAllPosts();
    const posts = res.data;
    props.onGetPosts(posts);
    setPosts(posts);
  };
};
```

다음으로 IProps라는 인터페이스를 선언한다. React.FC에 props의 형태에 대한 타입을 전달하기 위해서 사용한다.

`const List: React.FC<IProps> = props => {`
이런 형태로 IProps를 제공해주면 된다.

React.useEffect나 React.useState에 관한 부분은 Hooks 관련된 자료를 참고하길 바란다.

```tsx
const getPosts = async (): Promise<void> => {
  const res = await tubesdayApi.getAllPosts();
  const posts = res.data;
  props.onGetPosts(posts);
  setPosts(posts);
};
```

마지막으로 getPosts라는 함수를 만들었는데, API를 호출해 posts를 가져온다.
이후 props에 제공된 Action Creator 함수인 onGetPosts를 호출하여 Redux 스토어에 posts를 저장한다.

컴포넌트 안에서 `console.log(props)`를 해보면 posts가 추가된 것을 확인할 수 있을 것이다.

<p id="7" />
## 8. 참고자료

- [yesdoing, "TypeScript with React + Redux 사용하기"](https://velog.io/@yesdoing/TypeScript-with-React-Redux-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0-k5jsis62ah)
- [Costas Ioannou, "React-Native + Redux + Typescript Guide"](https://medium.com/@killerchip0/react-native-redux-typescript-guide-f251db03428f)
- [오지고지리고알파고포켓몬고, "React Native + Redux + Jest + Typescript + React Navigation 맛보기"](https://yuddomack.tistory.com/entry/React-Native-Redux-Jest-Typescript-React-Navigation-%EB%A7%9B%EB%B3%B4%EA%B8%B0)
- [carrot useless, "아마 이게 제일 이해하기 쉬울걸요? React + Redux 플로우의 이해"](https://medium.com/@ca3rot/%EC%95%84%EB%A7%88-%EC%9D%B4%EA%B2%8C-%EC%A0%9C%EC%9D%BC-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-%EC%89%AC%EC%9A%B8%EA%B1%B8%EC%9A%94-react-redux-%ED%94%8C%EB%A1%9C%EC%9A%B0%EC%9D%98-%EC%9D%B4%ED%95%B4-1585e911a0a6)
