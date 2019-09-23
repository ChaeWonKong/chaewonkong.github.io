---
layout: post
title: "React Native - 'use_frameworks!' Generates 'double-conversion.h file not found' Build Error for RN >= 0.60"
date: "2019-09-21"
categories:
  - React-Native
excerpt: |
  React Native - 'use_frameworks!' Generates 'double-conversion.h file not found' Build Error for RN >= 0.60
  TestFlight을 활용해 React Native iOS 앱 베타 테스트 진행하기
feature_text: |
  ## React Native - 'use_frameworks!' Generates 'double-conversion.h file not found' Build Error for RN >= 0.60
  'use_frameworks!' in Podfile generates build error for RN >= 0.60. Error "'double-conversion/double-conversion.h' file not found" Occurred.
feature_image: "https://images.unsplash.com/photo-1556656793-08538906a9f8?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80"
image: "https://images.unsplash.com/photo-1556656793-08538906a9f8?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80"
---

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/doubleconversion.png?raw=true" width="100%" />

For React Native version above **0.60**:
Using "use_frameworks!" in ios/Podfile generates following error while building the app:

```
'double-conversion/double-conversion.h' file not found
```

This issue has been tracked by Facebook and Facebook is prepairing workarounds and a future patch.

[SOURCE: StackOverflow LINK](https://stackoverflow.com/questions/57152569/double-conversion-double-conversion-h-file-not-found)

> CocoaPods are now part of React Native's iOS project. If you weren't already, be sure to open iOS platform code using the xcworkspace file from now on (protip: try xed ios from the root project directory). Also, the podspecs for the internal packages have changed to make them compatible with the Xcode projects, which will help with troubleshooting and debugging. Expect to make some straightforward changes to your Podfile as part of the upgrade to 0.60 to bring this exciting support. Note that we are aware of a compatability issue with use_frameworks!, and we're tracking an issue with workarounds and a future patch.
> [SOURCE: Official React Native 0.60 Release Note](https://facebook.github.io/react-native/blog/2019/07/03/version-60#cocoapods-by-default)

## 한국어

React Native 버전 0.60 이상에서는 'use_frameworks!'를 ios/Podfile 사용하여 앱을 빌드할 경우 다음과 같은 에러가 발생한다:

```err
'double-conversion/double-conversion.h' file not found
```

이 이슈는 페이스북에 의해 트래킹 되고 있으며 향후 패치될 것이라고 한다.

[출처: 스택오버플로우(영문) LINK](https://stackoverflow.com/questions/57152569/double-conversion-double-conversion-h-file-not-found)

> CocoaPods는 이제 React Native의 한 부분이다. 아직 사용하고 있지 않았다면, 앞으로는 xcworkspace 파일을 이용해 iOS 플랫폼 코드를 열도록 하자. 더불어, 내부 패키지를 위한 podspecs가 Xcode 프로젝트와 호환되게 변경되어 트러블슈팅과 디버깅을 돕고 있다. 0.60으로 업데이트하며 Podfile에 몇가지 수정을 통해 이와 같은 멋진 개선을 맛볼 수 있다.**우리는 use_frameworks!와 관한 호환성 이슈를 인지하고 있으며 해당 이슈의 해결 방법과 추후 패치 업데이트를 염두에 두고 있다.** > [출처: React Native 공식 릴리즈 노트(영문)](https://facebook.github.io/react-native/blog/2019/07/03/version-60#cocoapods-by-default)
