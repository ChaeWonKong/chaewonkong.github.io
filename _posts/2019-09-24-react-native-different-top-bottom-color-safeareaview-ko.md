---
layout: post
title: "React Native - iOS SafeAreaView의 top과 bottom 컬러 다르게 설정하기"
date: "2019-09-24"
categories:
  - React-Native
excerpt: |
  React Native - iOS SafeAreaView의 top과 bottom 컬러 다르게 설정하기
  iOS에서 SafeAreaView를 사용할 때 잘려나가는 top/bottom padding의 색을 각각 다른 색으로 설정하는 방법을 알아본다.
feature_text: |
  ## React Native - iOS SafeAreaView의 top과 bottom 컬러 다르게 설정하기
  iOS에서 SafeAreaView를 사용할 때 잘려나가는 top/bottom padding의 색을 각각 다른 색으로 설정하는 방법을 알아본다.
feature_image: "https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
image: "https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
---

{% raw %}
SafeAreaView는 iOS 11버전 이후부터 사용이 가능하며 아이폰 Xs 처럼 노치 등이 있는 경우 자식 컴포넌트에 padding을 적용하여 컨텐츠가 잘려나가지 않게 해준다. 디바이스별 물리적인 화면의 제한을 고려하여 어떠한 경우에도 컨텐츠가 잘려나가지 않고 안전하게 보여주도록 도와준다.

## React Native - SafeAreaView의 top과 bottom 컬러 다르게 설정하기

앱을 개발하다 보면 SafeAreaView를 이용함으로써 잘려나가는 top 부분의 padding과 bottom 부분의 padding에 서로 다른 색깔(컬러)을 적용하고 싶을 때가 있다. 이런 경우 어떻게 대응하는지 알아보자.

### 요구사항

```
react >= 16.2.0
```

우리는 Fragment를 React에서 import해 이 이슈를 해결할 것이다. Fragment를 사용하기 위해서는 16.2.0 이상의 React 버전을 사용해야 한다.

기본적으로 SafeAreaView를 이용한다면 다음과 같을 것이다:

```react
import React, { Component } from "react";
import { SafeAreaView, View, Text } from "react-native";

export default class App extends Component {
  render() {
    return (
      <SafeAreaView style={{ flex: 1 }}>
        <View style={{ flex: 1, backgroundColor: "#25365d" }}>
          <Text style={{ fontSize: 30, color: "#fff" }}>Text</Text>
        </View>
      </SafeAreaView>
    );
  }
}
```

이미지로 결과물을 보면 아래와 같다:

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/safearea1.png?raw=true" height="450px"/>

우리는 Fragment를 React에서 가져와 루트 컴포넌트의 SafeAreaView 컴포넌트를 감쌀 것이다. 이를 통해서 상단과 하단의 패딩(padding)에 서로 다른 색을 적용할 것이다.

코드는 다음과 같다:

```react
import React, { Component, Fragment } from "react";
import { SafeAreaView, Platform, View, StatusBar, Text } from "react-native";

export default class App extends Component {
  render() {
    if (Platform.OS === "ios")
      return (
        <Fragment>
          <SafeAreaView style={{ flex: 0, backgroundColor: "red" }} />
          <StatusBar barStyle="light-content" />
          <SafeAreaView style={{ flex: 1, backgroundColor: "blue" }}>
            <View style={{ flex: 1, backgroundColor: "#25365d" }}>
              <Text style={{ fontSize: 30, color: "#fff" }}>Text</Text>
            </View>
          </SafeAreaView>
        </Fragment>
      );
    else
      return (
        <View style={{ flex: 1, backgroundColor: "#25365d" }}>
          <Text style={{ fontSize: 30, color: "#fff" }}>Text</Text>
        </View>
      );
  }
}
```

StatusBar 컴포넌트는 아이폰 상단의 현재시각, 배터리, 네트워크 연결 상태 등을 보여주는 얇은 띄 형태의 바를 의미한다.

```
barStyle="light-content"
```

우리는 그 내부의 텍스트 색상을 흰 색으로 하기 위해 위 코드를 사용한다.

결과물을 살펴보자:

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/safearea2.png?raw=true" height="450px"/>

## 결론

Fragment를 React에서 가져와 사용함으로써 두개의 SafeAreaView를 선언하고 각기 다른 padding 색상을 적용할 수 있었다. 물론, 다른 전반적인 UI나 로직에는 변화가 없이 말이다.

## 참고자료

[React Native: Set different colors on Top and Bottom in SafeAreaView component](https://medium.com/reactbrasil/react-native-set-different-colors-on-top-and-bottom-in-safeareaview-component-f008823483f3)

{% endraw %}
