---
layout: post
title: "React Native 앱을 빌드해 앱스토어(App Store)에 배포하기"
date: "2019-09-21"
categories:
  - ReactNative
excerpt: |
  React Native로 작성한 앱을 빌드하고 앱 스토어(App Store)에 배포하기
feature_text: |
  ## React Native로 작성한 앱을 빌드하고 앱 스토어(App Store)에 배포하기
  React Native로 작성한 앱을 앱 스토어(Apple App Store)에 업로드하고 배포하는 방법을 알아본다. 그 과정에서 X-Code 에러를 핸들링하고 성공한 빌드를 앱스토어에 올린 후 심사를 요청하는 전 과정을 알아본다.
feature_image: "https://images.unsplash.com/photo-1457803097035-3ace37af34a7?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80"
image: "https://images.unsplash.com/photo-1457803097035-3ace37af34a7?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80"
---

알고 있겠지만, iOS 빌드를 위해서는 맥OS가 필요하다. 아이패드나 아이폰 같은 iOS로 된 기기도 필요하다. 부당하지만 애플의 방침이니 따를 수 밖에 없다.

## 빌드 준비

먼저 "ios/[프로젝트명].xcworkspace"를 연다.

"ios/[프로젝트명].xcodeproj"를 열어도 되지만, 빌드가 안되는 에러가 발생하는 경우가 많다. 따라서 "[프로젝트명].xcworkspace"로 빌드를 진행하는 것을 추천한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/xcode1.png?raw=true" width="100%" />
먼저 General 탭에서 정보들이 정상적으로 기입되었는지 확인한다. 제일 중요한 것은 Sinning 부분이다.
팀이 제대로 설정되어 있는지 확인한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/xcode2.png?raw=true" width="100%" />
info.plist도 잘 확인한다. 문제가 있을 경우 빌드가 실패할 수도 있고, 빌드가 성공하더라도 앱스토어에서 리젝(reject) 당할 수 있다.
저자의 경우 사진의 "Privacy - Location Always Usage Description" 항목을 영어로 작성하였는데, "위치정보 사용 동의"를 요청하는 모달의 경고문구에 해당하는 항목이다. 한국인을 대상으로 한국어로 서비스 되는 앱에서 경고 모달이 영어로 뜬 것은 접근성에 문제가 있는 것이라는 메세지와 함께 앱 심사 리젝을 당한 바 있다.

눈여겨 볼 포인트는 다음과 같다:

- string으로 들어간 것들 중 서비스 하는 언어가 아닌 언어로 작성된 경고 문구 등이 있는지
- 혹시 커스텀 폰트를 사용하였다면 "Fonts provided by application" 부분에 제대로 명시되어 있는지
- "Bundle display name", 버전명 등이 제대로 명시되어 있는지 등을 확인해야 한다.
- 특히 "Bundle version"은 새 버전 업로드 시마다 반드시 1씩 증가시켜야 한다. 버전 숫자는 양의 정수 형태인데, 1부터 시작한다. 같은 버전 번호의 앱을 한번 더 업로드 하려고 하면 에러가 발생한다.

ios 빌드용 인증서가 필요한데, 이 부분은 자세히 설명한 다른 블로그가 있어 해당 링크로 설명을 대신한다.
[iOS 인증서(Certification)](https://dev-yakuza.github.io/ko/react-native/ios-certification/)

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/schema.png?raw=true" width="100%" />
상단 메뉴 탭에서 scheme을 변경할 것이다.
products > scheme > edit scheme의 Build Configuration을 "Release"로 변경한다.

마지막으로 빌드하는 target이 "Generic iOS Device"로 설정되어 있는지 한번 더 확인한다.

또, RN의 경우 디버그 과정에서 로컬호스트 서버를 이용하므로 http 및 로컬호스트가 허용되어 있는데, ios/[프로젝트명]/info.plist를 코드 에디터로 열어 다음 코드 부분을 삭제해야 한다.

```
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
    <key>NSExceptionDomains</key>
    <dict>
        <key>localhost</key>
        <dict>
            <key>NSExceptionAllowsInsecureHTTPLoads</key>
            <true/>
        </dict>
    </dict>
</dict>
```

## 빌드

이제 빌드를 시작해보자.
빌드는 간단하다. 약간의 기도와 몇번의 클릭이면 충분하다.

메뉴 탭의 Product > Archive를 클릭하면 배포용 빌드가 시작된다.

## 빌드 트러블슈팅

빌드 과정에서 IDoubleConversion 에러가 발생한 바 있다. 이 경우 xcworkspace의 General/Capabilities...탭의 Build Settings 탭에서 "Linking"부분을 찾은 후 "Dead Code Stripping"을 "NO"로 설정하여 해결하였다.

빌드 과정에서 errSecInternalComponent와 관련된 에러가 발생하였다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/errSecInternalComponent.png?raw=true" width="100%" />
이 에러의 경우 Launchpad에서 기타 > 키체인 접근에 들어간 후, 메뉴 탭의 파일 > 모든 키체인 잠금 을 클릭한 후 다시 빌드를 시도해 보면 해결된다.

## 앱 스토어 업로드

앱스토어 업로드 및 심사 신청은 두 단계로 나눠진다.
첫 단계는 X-Code에서 업로드를 진행하는 것이다. 업로드는 시간이 좀 소요되며 보통은 개발자 계정 메일로 업로드 완료를 알려준다.

위에서 Product > Archive를 클릭해 빌드를 완료하면 X-Code의 오른쪽 상단에 파란 버튼이 활성화된다. "Upload to App Store"라는 파란 버튼을 클릭하고 아래 사진처럼 차례대로 진행하면 된다.

만약 앱스토어 커넥트(AppStoreConnect)에 새 앱을 추가하지 않았다면 아래의 "신규 앱 추가" 부분을 먼저 진행한 후 업로드 절차를 진행하도록 한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/upload1.png?raw=true" width="100%" />
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/upload2.png?raw=true" width="100%" />
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/upload3.png?raw=true" width="100%" />
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/upload4.png?raw=true" width="100%" />
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/upload5.png?raw=true" width="100%" />
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/upload6.png?raw=true" width="100%" />
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/upload7.png?raw=true" width="100%" />

## 앱스토어 커넥트를 통한 앱 제출과 심사

[앱스토어 커넥트](https://appstoreconnect.apple.com/)에 들어가 업로드 된 앱을 제출해보자.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/connect1.png?raw=true" width="100%" />
여기서 "나의 앱"을 클릭한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/connect2.png?raw=true" width="100%" />
본인의 앱을 만들었으면 본인의 앱을, 만들지 않았으면 "+"를 클릭해 추가한다.

### 신규 앱 추가

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/connect4.png?raw=true" width="100%" />
신규 앱을 추가하는 화면이다.
번들 ID는 X-code의 General에서 제공한 번들 Identifier와 정확히 동일해야 한다.
SKU는 적당히 지어내서 올리면 된다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/appstore/connect3.png?raw=true" width="100%" />
이제 앱을 추가하자. 중요한 부분은 아래에 자세히 정리해 두었다.

### 스크린샷

스크린샷은 최소 5.5형 iPhone과 6.5형 iPhone 두 종류의 것을 업로드해야 앱을 심사 요청할 수 있다.
이미지의 형식은 jpg 또는 png여야 한다. 앱 미리보기는 mov 영상 파일로 500MB 이하여야 한다.

#### 6.5형 픽셀 사이즈

- 1242 X 2688 픽셀

#### 5.5형 픽셀 사이즈

- 1242 X 2208 픽셀

스크린샷 사이즈가 위 사이즈와 정확히 일치하지 않으면 업로드 자체가 실패한다.
스크린샷 최소 개수가 있는지는 알 수 없으나, 본인의 경우 각 2개씩 업로드하였는데 심사에 통과하였다.

### 개인정보 처리방침

애플은 개인정보처리방침을 반드시 제출해야 한다.
개인정보 처리방침은 [여기 클릭](https://www.privacy.go.kr/a3sc/per/inf/perInfStep01.do)에서 자동으로 생성할 수 있다. html 파일로 생성해 주므로, 바로 서버에 올려 주소를 만들면 된다. URL을 앱스토어 커넥트에 입력해야 한다.

### 빌드

빌드 라는 탭에서 좀 전에 X-Code에서 업로드한 빌드 버전을 선택해 주어야 한다. 빌드 버전을 선택하면 앱 아이콘까지 정상적으로 들어간 것을 확인할 수 있을 것이다.

### 저작권

"2019 CoddingHippo Inc"와 같은 식으로 입력하면 된다. 반드시 입력해야 심사 신청을 할 수 있다.

## 심사

심사 기간은 최장 2주까지 걸릴 수 있다고 한다.
본인의 경우 1회 리젝을 먹은 끝에 2회만에 심사를 통과하였는데, 첫 심사는 약 3일, 두번째 심사는 12시간 정도가 소요되었다.
사람마다 개인차가 있을 수 있으나, 1달 이상 걸리는 경우는 없다고 한다. 보통은 3-7일 소요되는 것 같다.

심사는 심사 준비 중 > In Review(심사 중) > Ready for Sale(판매 준비됨) 단계로 나눠진다.
심사 결과는 시시각각 메일로 알려준다. "판매 준비됨" 상태가 되면 심사에 통과한 것이고 3 - 12시간 이내에 앱스토어에 앱이 나타나기 시작한다.

앱 빌드시 최신 iOS만 지원하도록 빌드하면 최신 iOS에서만 검색이 가능한 것 같고, 사용할 수 없는 디바이스에서는 검색이 안 되는 것 같다.

### Test Flight

테스트 플라이트가 생소한 개발자를 위해 몇 자 적는다.
Test Flight는 심사를 통과하지 않은 앱이라도 베타테스트를 진행할 수 있게 도와주는 서비스이다.
사전에 추가된 사용자들이 Test Flight앱을 설치하고 그 앱을 통해 우리가 만든 앱을 베타 테스트할 수 있다.

테스트 플라이트를 하기 위해서도 심사가 필요하다. 하지만 정식 심사보다는 간단한 것 같다.
테스트 플라이트 심사는 1일 이내에 끝난다고도 하는데, 본인의 경우 3일 정도가 소요되었다.

## 마치며

앱스토어 심사는 생각보단 까다롭지만, 생각만큼 까다롭지 않다.
말장난이긴 하지만, 너무 쉽게 생각하지도 너무 겁먹지도 말자는 것이다.

스플래쉬, 앱 아이콘, SafeAreaView, 퍼미션 등을 착실히 개발하였다면 도전해 보자!

앱이 통과되지 않았다고 실망하진 말자. Test Flight과 같은 방법을 통해서 정식 배포 전에도 테스트를 해볼 수 있으니 참고하자!

빌드 과정과 심사 과정은 생각보단 순탄치 않다. 에러나 버그가 발생하면 구글링을 통해 답을 찾아보고, 안되면 X-Code에서
Product > Clean Build Folder를 한 후 다시 빌드에 도전해보자.
