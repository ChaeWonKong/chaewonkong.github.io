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
TypeScript와 함께 react-native-vector-icons 사용하기


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

## iOS

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

다음을 추가

```gradle
apply from: "../../node_modules/react-native-vector-icons/fonts.gradle"
```



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



위 사용 예 처럼 Icon컴포넌트는 반드시 Text 컴포넌트 내에서 사용해야 한다.



**name**: [아이콘 검색 사이트](https://oblador.github.io/react-native-vector-icons/) 에서 필요한 아이콘을 검색하고 이름을 복사해 넣으면 된다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/rn-vector-icons2.png?raw=true" width="100%" />

**size**: 사용할 아이콘의 사이즈를 입력한다.

**color**: 사용할 아이콘의 컬러를 입력한다.

## 참고자료
- [react-native-vector-icons](https://github.com/oblador/react-native-vector-icons)