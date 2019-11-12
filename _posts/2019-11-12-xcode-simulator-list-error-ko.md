---
layout: post
title: "React Native - iOS Error: Could not parse the simulator list output."
date: "2019-11-12"
categories:
  - React-Native
excerpt: |
  iOS 디버그 빌드 에러 - "Could not parse the simulator list output."
feature_text: |
  ## React Native - iOS Error: Could not parse the simulator list output.
  iOS 디버그 빌드 에러 - "Could not parse the simulator list output."
feature_image: "https://images.unsplash.com/photo-1534802046520-4f27db7f3ae5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2001&q=80"
image: "https://images.unsplash.com/photo-1534802046520-4f27db7f3ae5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2001&q=80"
---

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/xcode1.png?raw=true" width="100%" />

| Environments |   버전   |
| :----------: | :----: |
| react-native | 0.60.4 |
|    Xcode     |  11.2  |

최근 Xcode를 11.1에서 11.2로 업데이트 하면서 React Native의 ios debug 빌드가 아래와 같은 에러와 함께 실패하게 되었다.
```react-native run-ios``` 명령을 실행하면 아래 에러 메세지를 확인할 수 있었다.

```shell
info Found Xcode workspace "Native_Makkcha.xcworkspace"
2019-11-12 11:55:22.239 simctl[12139:74526] CoreSimulator detected version change.  Framework version (681.15) does not match existing job version (681.5.4).  Attempting to remove the stale service in order to add the expected version.
error Could not parse the simulator list output. Run CLI with --verbose flag for more details.
SyntaxError: Unexpected token I in JSON at position 0
    at JSON.parse (<anonymous>)
    at runOnSimulator (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/@react-native-community/cli-platform-ios/build/commands/runIOS/index.js:128:23)
    at Object.runIOS [as func] (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/@react-native-community/cli-platform-ios/build/commands/runIOS/index.js:97:12)
    at Command.handleAction (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/@react-native-community/cli/build/cliEntry.js:160:21)
    at Command.listener (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/commander/index.js:315:8)
    at Command.emit (events.js:198:13)
    at Command.parseArgs (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/commander/index.js:651:12)
    at Command.parse (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/commander/index.js:474:21)
    at setupAndRun (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/@react-native-community/cli/build/cliEntry.js:210:24)
    at Object.run (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/@react-native-community/cli/build/cliEntry.js:173:11)
```

## 해결책
해결책은 매우 간단하다.
일단 **[프로젝트이름].xcworkspace 파일을 열기만 하면 된다. 나머지는 Xcode가 알아서 필요한 컴포넌트들을 설치하면서 저절로 해결된다.**

Xcode를 한 번도 실행하지 않아서 생기는 에러라고 한다.

## 발생 가능한 에러
.xcworkspace 파일을 열기만 해도, ```Could not parse the simulator list output``` 에러는 바로 사라졌다.

그러나, 새로운 에러가 발생했다.

```shell
info Found Xcode workspace "Native_Makkcha.xcworkspace"
error Could not find "iPhone X" simulator. Run CLI with --verbose flag for more details.
```

이 에러는 Xcode를 버전 11.1로 업데이트 할 때부터 발생했던 에러였다.
에러의 발생 이유는 **React Native의 타겟 빌드 디바이스 이름과 Xcode의 디바이스 이름이 서로 다르기 때문**이다.

자세한 내용은 이 포스트를 참고하자: [React Native - Could Not Find 'iPhone X' Error
](https://chaewonkong.github.io/posts/xcode-iphonex-not-found.html)

그래서 Xcode에 "iPhone X"를 수동으로 등록했다..

자세한 내용은 아래 사진을 참고하자.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/xcode2.png?raw=true" width="100%" />
왼쪽 아래에 있는 "+" 버튼을 클릭한 후 아래 사진처럼 "iPhone X"를 등록하면 된다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/xcode3.png?raw=true" width="100%" />


## 참고자료
- [StackOverFlow, "Hello, World! program for React Native throws “Could not parse the simulator list output” on Mac"](https://stackoverflow.com/questions/39810470/hello-world-program-for-react-native-throws-could-not-parse-the-simulator-lis)