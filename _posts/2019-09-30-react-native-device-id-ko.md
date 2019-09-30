---
layout: post
title: "React Native - react-native-device-info를 이용해 기기(device) 고유 id 가져오기"
date: "2019-09-30"
categories:
  - React-Native
excerpt: |
  react-native-device-info의 getUniqueId() 메서드를 이용해 아이폰/안드로이드 폰의 고유 id 값을 가져와 본다.
feature_text: |
  ## React Native - react-native-device-info를 이용해 기기(device) 고유 id 가져오기
  react-native-device-info의 getUniqueId() 메서드를 이용해 아이폰/안드로이드 폰의 고유 id 값을 가져와 본다.
feature_image: "https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
image: "https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
---

종종 아이폰 및 안드로이드 폰에서 각 폰별로 고유한 ID 값을 알아야 하는 경우가 있다.
react-native-device-info 라는 라이브러리를 사용하면 쉽게 이 일을 처리할 수 있다.

**[react-native-device-info](https://github.com/react-native-community/react-native-device-info)** 

## 설치
React Native 버전 0.60 이상에서는 간단하게 설치 가능하다:
```
yarn add react-native-device-info
cd ios && pod install && cd ..
```

위 코드는 자동으로 라이브러리를 링크해준다. 따로 수동('manual')으로 Podfil에 코드를 추가하거나 link할 필요가 없다.

## 소개
react-native-device-info는 우리가 사용하려는 getUniqueId() 메소드 말고도 많은 유용한 메소드들을 제공한다.

- getUniqueId(): 기기별로 고유한 ID 값을 리턴한다.
- getBatteryLevel(): 배터리가 얼마나 남았는지 알려준다.
- getDeviceId(): 이것은 사실 ID라고 하기는 애매하다. "iPhone7, 2" 혹은 안드로이드의 경우 "goldfish" 와 같은 형태로 리턴된다.
- getIpAddress(): IP 주소를 알 수 있다.

## 사용법

DeviceInfo.getUniqueId()를 사용하면 우리가 원하는 기기별 고유한 ID 값을 알 수 있다.

```react
import DeviceInfo from 'react-native-device-info';

DeviceInfo.getUniqueId().then(uniqueId => {
  console.log(uniqueId)
  // iOS: "FCDBD8EF-62FC-4ECB-B2F5-92C9E79AC7F9"
  // Android: "dd96dec43fb81c97"
});
```

"getUniqueId()"는 Promise를 리턴한다.

## 참고자료
- [react-native-device-info](https://github.com/react-native-community/react-native-device-info)