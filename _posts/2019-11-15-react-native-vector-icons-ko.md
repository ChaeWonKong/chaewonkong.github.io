---
layout: post
title: "React-Native - TypeScript와 함께 react-native-vector-icons 사용하기"
date: "2019-11-15"
categories:
  - React-Native
excerpt: |
  React Native 프로젝트에서 TypeScript로 FontAwesome, Ionicons, MaterialIcons 등의 아이콘을 쉽게 사용하도록 도와주는 react-native-vector-icons 라이브러리를 사용해보자.
feature_text: |
  ## React-Native - TypeScript와 함께 react-native-vector-icons 사용하기
  React Native 프로젝트에서 TypeScript로 FontAwesome, Ionicons, MaterialIcons 등의 아이콘을 쉽게 사용하도록 도와주는 react-native-vector-icons 라이브러리를 사용해보자.
feature_image: "https://images.unsplash.com/photo-1506729623306-b5a934d88b53?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
image: "https://images.unsplash.com/photo-1506729623306-b5a934d88b53?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
---

React Native에서 아이콘(Icon)을 사용하는 방법은 여러가지가 있겠지만, **react-native-vector-icons를 사용하면 Material Icons, Font Awesome, Ionicons 등 다양한 라이브러리를 한번에 사용할 수 있기에, 이 라이브러리를 추천**하고 싶다. 사용법도 간단하다.

**TypeScript와 함께 react-native-vector-icons 사용하는 방법**을 알아보자.

## 지원하는 아이콘
- **AntDesign** by AntFinance (297 icons)
- **Entypo** by Daniel Bruce (411 icons)
- **EvilIcons** by Alexander Madyankin & Roman Shamin (v1.10.1, 70 icons)
- **Feather** by Cole Bemis & Contributors (v4.21.0, 279 icons)
- **FontAwesome** by Dave Gandy (v4.7.0, 675 icons)
- **FontAwesome** 5 by Fonticons, Inc. (v5.7.0, 1500 (free) 5082 (pro) icons)
- **Fontisto** by Kenan Gündoğan (v3.0.4, 615 icons)
- **Foundation** by ZURB, Inc. (v3.0, 283 icons)
- **Ionicons** by Ben Sperry (v4.2.4, 696 icons)
- **MaterialIcons** by Google, Inc. (v3.0.1, 932 icons)
- **MaterialCommunityIcons** by MaterialDesignIcons.com (v4.0.96, 4416 icons)
- **Octicons** by Github, Inc. (v8.4.1, 184 icons)
- **Zocial** by Sam Collins (v1.0, 100 icons)
- **SimpleLineIcons** by Sabbir & Contributors (v2.4.1, 189 icons)

이처럼 다양한 아이콘들을 지원하니 필요한 아이콘을 골라서 사용하기만 하면 된다!

## 설치 환경 안내
이 블로그는 다음의 환경을 기반으로 씌여졌다.
환경이 다르다고 꼭 에러가 발생하는 것은 아니다. 혹시라도 문제가 생길 경우 참고하자.
|            Dependency            | Version  |
| :------------------------------: | :------: |
|           react-native           |  0.61.2  |
|       @types/react-native        | ^0.60.22 |
|    react-native-vector-icons     |  ^6.6.0  |
| @types/react-native-vector-icons |  ^6.4.4  |

## 설치

```shell
yarn add react-native-vector-icons
yarn add --dev @types/react-native-vector-icons
```
@types/react-native-vector-icons는 타입 설정을 위해 필요하다. 즉, dev 환경에서만 사용하므로 --dev 플래그를 붙여 설치한다.

**이제 iOS와 Android에서 사용할 수 있도록 약간의 추가작업을 진행해야 한다.**

## iOS
여기서는 CocoaPods를 사용해 설치하는 것만 살펴본다.
React Native 0.60.0 이상의 버전이라면 CocoaPods를 사용하는 것이 일반적이다.

수동 설치와 `react-native link`를 활용한 설치는 [라이브러리의 도큐먼테이션](https://github.com/oblador/react-native-vector-icons)을 참고하자.
#### Podfile

```podfile
pod 'RNVectorIcons', :path => '../node_modules/react-native-vector-icons'

```


Podfile에 위 코드를 추가하였으면 설치 프로세스를 진행하자.

```shell
cd ios && pod install && cd..
```


### Info.plist
Info.plist를 열고 "Fonts provided by application"을 추가한다.
이후 사진처럼 item으로 필요한 파일을 추가한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/rn-vector-icons1.png?raw=true" width="100%" />


## Android

### android/app/build.gradle

다음을 추가한다.

```gradle
apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
```

안드로이드에서는 이외의 설정이 필요하지 않다.


## 사용

```tsx
import Icon from 'react-native-vector-icons/dist/Ionicons'

const App () => {
  return(
  <View>
  	<Text>
    	<Icon name="rocket" size={30} color="#900" />
    </Text>  
  </View>
  )
}
```



위 사용 예 처럼 **Icon 컴포넌트는 반드시 Text 컴포넌트 내에서 사용해야** 한다.



**name**: [아이콘 검색 사이트](https://oblador.github.io/react-native-vector-icons/) 에서 필요한 아이콘을 검색하고 이름을 복사해 넣으면 된다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/rn-vector-icons2.png?raw=true" width="100%" />

**size**: 사용할 아이콘의 사이즈를 입력한다.

**color**: 사용할 아이콘의 컬러를 입력한다.

## 참고자료
- [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons)