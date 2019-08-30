---
layout: post
title: "React & Redux 1"
date: "2018-10-30"
categories:
- React
excerpt: |
  Redux는 어떻게 작동하는지 알아보자
feature_text: |
  ## React에서 Redux 사용하기1
  Redux는 어떻게 작동하는가
feature_image: "https://cdn-images-1.medium.com/max/1200/1*VeM-5lsAtrrJ4jXH96h5kg.png"
image: "https://cdn-images-1.medium.com/max/1200/1*VeM-5lsAtrrJ4jXH96h5kg.png"
---


## Redux란 무엇이고 왜 사용해야 하는걸까?
React에서 state를 관리하기 위해 흔히 Redux를 사용한다. Redux는 React에서만 사용되는 상태관리 도구이다. React에서는 state와 props를 활용해 컴포넌트들에게 data를 공급한다. 하지만 컴포넌트의 개수가 많아지면 컴포넌트별로 각각의 state를 두는 React의 방식은 매우 불편하고 복잡해진다. 

props의 경우도 마찬가지이다. A 컴포넌트 안에 B 컴포넌트, B 컴포넌트 안에 C 컴포넌트가 child 컴포넌트로 존재한다고 가정하자. A 컴포넌트에서 data를 C 컴포넌트로 보내기 위해서는 반드시 B 컴포넌트를 거처야만 한다. A에서부터 prop으로 B를 통해 C로 건네야만 하는데, 만약 B 컴포넌트는 해당 data가 필요 없다면 이는 큰 비효율이다.

Redux를 이용하면 data를 한 곳에 저장하고, state가 변경될 때마다 컴포넌트를 rerender할 수 있다. 또 props의 형태로 각 컴포넌트가 data를 이어받을 필요 없이, 해당 data가 필요한 컴포넌트에 그 데이터를 바로 꽂아줄 수 있다.

Redux는 하나의 중앙 집중형 data 저장소이다. data를 모아 저장하고, 변경사항이 있을 시 컴포넌트들에게 이를 알린다. Redux의 컨셉은 새로 배우는 사람이 이해하기엔 조금 어렵다. Reducer, action, actionCreator, store 등등 구조를 만들면서도 왜 이런 구조를 만들어야하는지 모르기 때문이다. 하지만, 한번 Redux를 사용하기 시작하면, Redux 없는 React는 상상하기 힘들다. 그리고 앱이 복잡해질 수록, Redux는 더욱 더 빛을 발할 것이다.

## 기본적인 Redux 로직
1. 사용자가 운동량을 타이핑함
2. 새 text와 함께 Action Creator를 호출함
3. Action Creator가 Action을 반환함
4. Action이 모든 Reducer로 전송됨
5. Reducer가 새로운 앱 상태(state)를 업데이트함
6. 모든 컴포넌트로 상태(state)가 전송됨
7. 컴포넌트들이 새 상태를 바탕으로 rerender

## 실습할 앱 소개
이번에 실습할 앱은 헬스장에서 본인의 운동량을 기록할 수 있는 웹앱이다. 한번쯤은 헬스를 하면서 저번 운동 때 바벨을 몇 킬로그람까지 들었는지, 몇 세트를 했는지 헷갈렸던 기억이 있을 것이다. 이 앱은 운동의 종류, 무게, 횟수, 날짜 등을 기록할 수 있게 해 좀 더 체계적으로 운동을 할 수 있도록 도와준다.

웹페이지 형태로 된 앱을 제작할 것이지만, 철저하게 모바일 사용을 전제로 하여 UI와 UX를 설계했다. 운동에 관한 정보는 개인 정보다. 따라서 로그인을 통해 개인별로 자신의 운동량을 기록하도록 한다. Firebase를 이용하여 로그인 및 여타 기능을 제공할 것이다.

## React에서 Redux 설치하기
앞서 말했듯, React와 Redux는 찰떡궁합이지만, Redux는 React만을 위한 도구는 아니다. 따라서 React에서 Redux를 사용하기 위해서는 그 둘을 연결해주는 헬퍼 등을 사용해야 한다.

먼저 Redux를 설치해보자.

```npm install --save react-redux redux```

혹은 yarn을 사용한다면,

```yarn add react-redux redux```

react-redux와 redux를 모두 설치해야 한다. react-redux는 React와 Redux를 바인딩해주는 라이브러리이다.


## 마치며
Redux를 사용하는 방법은 단순하지 않다. 이해하기도 까다롭고 숙달되기 위해서는 많은 반복을 필요로 한다. 그러나 그럴만한 가치가 있다. 다음 편에서는 React에서 Redux를 사용하는 법을 본격적으로 알아본다.


