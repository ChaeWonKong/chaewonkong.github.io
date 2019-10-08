---
layout: post
title: "React Native - 안드로이드 앱을 빌드하고 구글 플레이 스토어에 배포하자"
date: "2019-10-08"
categories:
  - React-Native
excerpt: |
  React Native - 안드로이드 앱을 빌드하고 구글 플레이 스토어에 배포하자
  React Native 프로젝트를 안드로이드 앱으로 빌드하고 구글 플레이 스토어에 배포하는 방법을 알아본다. 안드로이드(구글) 개발자 등록부터 서명 키 발급, 빌드, 업로드 및 배포까지 차근차근 살펴본다.
feature_text: |
  ## React Native - 안드로이드 앱을 빌드하고 구글 플레이 스토어에 배포하자
  React Native 프로젝트를 안드로이드 앱으로 빌드하고 구글 플레이 스토어에 배포하는 방법을 알아본다. 안드로이드(구글) 개발자 등록부터 서명 키 발급, 빌드, 업로드 및 배포까지 차근차근 살펴본다.
feature_image: "https://images.unsplash.com/photo-1551808525-51a94da548ce?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2196&q=80"
image: "https://images.unsplash.com/photo-1551808525-51a94da548ce?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2196&q=80"
---

React Native CLI로 만든 어플리케이션을 안드로이드 앱으로 빌드하고 구글 플레이 스토어에 업로드 하는 과정을 알아본다.

## 준비할 것

- [구글 개발자 등록](https://play.google.com/apps/publish/signup/)
- 안드로이드 서명 키 생성
- 스크린샷(최소 2개의 스크린샷 필요): 측면 최소 320픽셀 측면 최대 3840픽셀의 JPG 또는 24비트 PNG(알파 없음)
- 그래픽 이미지: 1024X500 픽셀의 JPG 또는 24비트 PNG(알파 없음)

### 구글 개발자 등록

[구글 개발자 등록 페이지](https://play.google.com/apps/publish/signup/)

개발자 등록에는 \$25가 필요하며 매년 재등록을 해야 하는 iOS와는 달리 1회 등록으로 평생 이용이 가능하다. 위의 링크에서 차근차근 따라가면 된다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/join1.png?raw=true" width="100%" />
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/join2.png?raw=true" width="100%" />
카드를 등록하면 바로 결제가 이루어진다.

자세한 내용은 [Dev Yakuza - 안드로이드 개발자 등록](https://dev-yakuza.github.io/ko/react-native/android-enroll-google-play-developer/) 블로그를 참고 바란다.

### 안드로이드 서명 키 생성

[Facebook - Publishing to Google Play Store](https://facebook.github.io/react-native/docs/signed-apk-android)

안드로이드 앱을 빌드하고 구글 플레이 스토어에 업로드(배포)하기 위해서는 서명 키를 생성해야 한다. 생성한 서명 키는 잘 보관해야 하며, 서명 키가 바뀔 경우 빌드는 할 수 있으나, 스토어에 업로드 할 수 없게 된다.

**[중요]**
부득이하게 서명 키를 변경해 앱을 업로드하고자 한다면, 구글에 메일로 문의하여 서비스를 기다려야 한다. 변경된 서비스 키가 적용되는 데에도 시간이 꽤 소요되므로 통상 키 변경과 적용에는 5일 정도 걸릴 수 있다고 알아두자. (**웬만하면 서명 키는 잘 보관하여 문제가 생기지 않게 하자.**)

자, 이제 키를 생성하는 방법을 알아보자.
우선 루트 디렉토리에서 android/app으로 이동하여 키를 생성한다. 다른 디렉토리여도 상관 없으나 생성된 서명 키를 쉽게 사용하기 위해 우리는 저 디렉토리에 키를 생성하려 한다.

```cmd
cd android/app
keytool -genkeypair -alias [키 별명] -keyalg RSA -keysize 2048 -validity 9125 -keystore keystore.jks
```

키 별명 부분에는 영문으로 별명(alias)을 지정해 주면 된다.

이 키는 2048비트 RSA 키이어야 하며 유효기간은 25년이어야 한다. 자세한 내용은 [Android Studio 고객센터 - 앱 서명](https://developer.android.com/studio/publish/app-signing.html#generate-key)을 확인하자.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/genkey1.png?raw=true" width="100%" />
Terminal에서 차근차근 입력하며 key를 생성해 나간다.

중간에 맞는지를 질문하는데 맞으면 y, 틀리면 n을 입력하면 된다. 국가 코드는 ko를 입력한다(큰 상관은 없는 것 같다).
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/genkey2.png?raw=true" width="100%" />
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/genkey3.png?raw=true" width="100%" />
다 생성하고 나면 위와 같은 화면이다. 성공했다.

## 안드로이드 빌드

이제 안드로이드를 빌드해 보자. android 폴더로 이동해 다음 명령어를 입력한다.

```cmd
cd android
./gradlew assembleRelease
```

빌드된 파일은 `android/app/build/outputs/apk/release/` 디렉토리에 생성되며 생성된 모든 파일을 구글 플레이 스토어에 모두 올려주어야 한다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/and-build.png?raw=true" width="100%" />
위 사진 기준으로 output.json을 제외한 4개 파일을 모두 업로드 하게 된다.

## 구글 플레이 스토어에 앱 업로드하기

구글 플레이 스토어(Google Play Store)에 앱을 업로드 하기 위해서는 구글 플레이 콘솔(Google Play Console)을 사용한다.
[구글 플레이 콘솔](https://developer.android.com/distribute/console?hl=ko)에 들어가 로그인 한다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/and-store1.png?raw=true" width="100%" />
로그인 한 후, 화면 오른쪽 상단의 "애플리케이션 만들기"라는 파란색 버튼을 클릭해 앱을 생성한다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/and-store2.png?raw=true" width="100%" />
앱 이름과 언어를 설정하고 만들기를 클릭한다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/and-store3.png?raw=true" width="100%" />
위 사진의 왼쪽 메뉴에서 빨간 사각형으로 표시된 것들이 기입해야 할 정보들이다. "앱 버전"을 제외한 나머지를 먼저 하나씩 채워 나가보자.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/and-store4.png?raw=true" width="100%" />
빌드한 앱은 "앱 버전"에서 업로드 한다. 위 사진의 프로덕션 트랙이 앱을 업로드 할 수 있는 페이지이다. "관리" 버튼을 눌러보자.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/and-store5.png?raw=true" width="100%" />
"새 버전 출시하기" 버튼을 클릭한다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/and-store6.png?raw=true" width="100%" />
"계속"을 선택한 후 "추가할 Android App Bundle 및 APK" 섹션에서 "파일 찾아보기" 버튼을 클릭해, 아까 빌드했던 모든 파일(json 제외)을 추가해준다.

APK 파일이 모두 추가되었으면 출시명(버전이름)과 "이전 버전의 새로운 기능" 부분을 채워 넣은 후 맨 밑에 "저장" 버튼, 이후 "검토" 버튼을 클릭하면 된다.
검토가 끝나면 앱을 게시하면 된다.

## 심사 기간

안드로이드 앱을 처음 등록할 경우 심사에는 1-3일 정도가 소요되고, 웬만하면 통과된다. 심지어 시작부터 버그로 작동하지 않는 경우도 통과되는 것 같다.
이후 업데이트 시에는 약 12-24시간이면 업데이트가 반영되는 것 같다. 빠른 경우 3시간 만에도 업데이트가 반영된다.

## 앱 업데이트

앱 업데이트는 아까 살펴보았던 구글 플레이 콘솔의 "앱 버전" 메뉴 - "프로덕션 트랙"에서 진행한다.
여기서 주의할 점이 있다. 업데이트를 위해 빌드하는 경우, 버전코드를 반드시 increment해야 한다.

android/app/build.gradle을 열고 스크롤을 내리면 `versionCode`라는 것을 찾을 수 있다. 매 업로드 시마다 이 숫자는 1씩 증가해야 한다. 깜빡해서 이 숫자를 증가시키지 않고 빌드한 채 업로드를 하려고 하면 구글 플레이 콘솔에서 업로드가 되지 않는 에러가 발생한다. `versionName`은 굳이 바꾸지 않더라도 `versionCode`만큼은 반드시 1씩 증가시킨 후, 빌드하고 업데이트를 진행해야 한다.

```gradle
android {
    compileSdkVersion rootProject.ext.compileSdkVersion

    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }

    defaultConfig {
        applicationId "com.native_makkcha"
        // multiDexEnabled true
        minSdkVersion rootProject.ext.minSdkVersion
        targetSdkVersion rootProject.ext.targetSdkVersion
        versionCode 15 // 반드시 매 업데이트 목적의 빌드 시마다 1씩 증가시켜야 함
        versionName "1.1.1"
    }
    ...
}
```

## 참고자료

- [Facebook - Publishing to Google Play Store](https://facebook.github.io/react-native/docs/signed-apk-android)
- [Dev Yakuza - 안드로이드 개발자 등록](https://dev-yakuza.github.io/ko/react-native/android-enroll-google-play-developer/)
- [Dev Yakuza - 안드로이드 빌드 및 테스트](https://dev-yakuza.github.io/ko/react-native/android-running-on-device/)
- [Dev Yakuza - 안드로이드 앱 스토어 등록](https://dev-yakuza.github.io/ko/react-native/android-google-play/)
- [Android Studio 고객센터 - 앱 서명(업로드 키 및 키 저장소 생성)](https://developer.android.com/studio/publish/app-signing.html#generate-key)
- [i-Cell - React Native 배포용 빌드](https://blog.nezen.co.kr/react-native-baepoyong-bildeu/)
