---
layout: post
title: "React Native - 안드로이드(Android)에 헤르메스(Hermes) 적용하기"
date: "2019-09-30"
categories:
  - React-Native
excerpt: |
  React Native에서 Android에 헤르메스(Hermes) 자바스크립트 엔진을 적용해 메모리 사용량을 최소화하고 앱 사이즈를 줄이며 앱 작동 성능을 개선해 본다.
feature_text: |
  ## React Native - 안드로이드(Android)에 헤르메스(Hermes) 적용하기
  React Native에서 Android에 헤르메스(Hermes) 자바스크립트 엔진을 적용해 메모리 사용량을 최소화하고 앱 사이즈를 줄이며 앱 작동 성능을 개선해 본다.
feature_image: "https://cdn.pixabay.com/photo/2019/03/24/21/15/hermes-4078702_960_720.jpg"
image: "https://cdn.pixabay.com/photo/2019/03/24/21/15/hermes-4078702_960_720.jpg"
---

헤르메스(Hermes)는 오픈소스 기반 자바스크립트 엔진으로 안드로이드에서 React Native 앱을 구동하는 것에 최적화 되어 있다.

## 헤르메스 특징
### 빌드 시 바이트코드(bytecode) 컴파일
헤르메스(Hermes)를 사용하지 않은 React Native 앱은 실행 시점에 JavaScript를 바이트코드(bytecode)로 컴파일한다.

하지만 **헤르메스(Hermes)를 사용하면 JavaScript가 빌드시 바이트코드(bytecode)로 컴파일한다.** 결과적으로 **빌드의 결과물의 크기가 줄어들고, 실행시점에 컴파일이 필요 없어 초기 실행 속도가 개선된다.**

### JIT 배제
일반적으로 JavaScript 엔진들은 JIT(Just-in-time) 컴파일러를 사용해 컴파일 성능을 높인다. 하지만, 페이스북에 따르면 이는 모바일 앱에는 해당되지 않는 이야기라고 한다. 오히려, **JIT가 워밍 업 타임을 필요로 하여 앱의 초기로딩(TTI)를 악화시키는 결과를 가져왔다고 한다** 그래서 **헤르메스(Hermes)는 JIT가 배제되어 있다.**

### 가비지 컬렉션 전략(Garbage Collection Strategy)
모바일 앱에서는 특히 효율적인 메모리 사용이 중요하다. 데스크톱에 비해 기본적으로 성능이 부족하기도 하거니와 OS 시스템이 적극적으로 메모리를 많이 사용하는 앱을 종료시키기 때문이기도 하다. 이렇게 앱이 종료되면 앱을 새로 시작하는 데 많은 시간이 소요되고 이는 고스란히 사용자에게 부담으로 작용한다.

Facebook은 헤르메스(Hermes)에서 메모리 사용량을 최소화하고 VA 공간을 최소화하기 위해 새로운 가비지 콜렉터(Garbage Collector)를 도입했다.

**개선된 가비지 컬렉터(Garbage Collector)**
- 필요에 따라 청크(Chunk) 단위로 배분되는 VA 공간
- 불연속적인 VA 공간 배분을 통한 32-bit 기기에서의 한계 극복
- Moving: Being able to move objects means memory can be defragmented and chunks that are no longer needed are returned to the operating system.
- 가비지 컬렉션(Garbage Collection)마다 모든 JavaScript Heap을 스캔하지 않음으로써 스캔에 소요되는 시간 감축


## 헤르메스(Hermes) 이점
헤르메스(Hermes) 사용하면,
1. 앱 실행(Launch) 속도가 빨라지고(TTI 개선),
2. APK 사이즈를 줄일 수 있으며,
3. 메모리 사용량을 줄일 수 있다.

Facebook에 따르면 MatterMost 앱의 초기 실행(TTI)은 4.30초에서 2.29초 빨라지고, 앱 사이즈는 41MB에서 19MB 줄어들었으며, 메모리는 185MB에서 49MB 줄어들었다고 한다.

무엇보다 헤르메스(Hermes) 사용하기가 정말 쉽다. 단 코드 두 줄만 수정하면 된다.

## 헤르메스(Hermes) 사용하기
**android/app/build.gradle**을 연다.
```gradle
project.ext.react = [
    entryFile: "index.js",
    enableHermes: false,  // clean and rebuild if changing
]
```

이 부분에서 'enableHermes'를 'false'에서 'true'로 바꿔준다.

아래로 조금만 내려가다 보면
```gradle
def enableHermes = project.ext.react.get("enableHermes", false);
```
이런 부분이 있다.
여기도 역시 'false'를 'true'로 바꿔준다.

## 결과
헤르메스(Hermes) 사용 전 우리 안드로이드 앱은 12.8MB 정도의 용량이었다. 헤르메스(Hermes) 사용하여 빌드한 결과 10.8MB 수준으로 앱 다운로드 크기를 줄일 수 있었다. 대략 16% 정도의 용량이 절감된 것이다.

## 참고 자료
- [Facebook Official React Native Docs](https://facebook.github.io/react-native/docs/hermes/)
- [Hermes Official Site](https://hermesengine.dev/)
- [Facebook Engineering Blog](https://engineering.fb.com/android/hermes/)