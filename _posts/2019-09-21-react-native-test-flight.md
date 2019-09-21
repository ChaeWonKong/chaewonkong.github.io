---
layout: post
title: "React Native - iOS TestFlight"
date: "2019-09-21"
categories:
  - React-Native
excerpt: |
  React Native - iOS TestFlight
  TestFlight을 활용해 React Native iOS 앱 베타 테스트 진행하기
feature_text: |
  ## React Native - iOS TestFlight
  React Native로 작성한 iOS 앱으로 앱 스토어 커넥트(App Store Connect)에서 TestFlight을 활용해 베타테스트를 진행해보자.
feature_image: "https://images.unsplash.com/photo-1556656793-08538906a9f8?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80"
image: "https://images.unsplash.com/photo-1556656793-08538906a9f8?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80"
---

## iOS TestFlight 소개

TestFlight은 아직 정식으로 앱스토어에 출시되지 않은 앱을 베타테스트 할 수 있도록 돕는 도구이다.
정식 심사보다 간결한 심사를 통과하기에, 심사 전 앱을 테스트해 보는 것에 적격이다.

앱스토어에 TestFlight을 검색하여 앱을 설치할 수 있고, 베타 테스트 링크를 메일로 전송받은 사용자들이 TestFlight앱을 활용해 베타 테스트에 참여할 수 있다.

[iOS TestFlight 애플 공식 문서](https://developer.apple.com/kr/testflight/)

## TestFlight 신청하기

먼저 TestFlight을 활용하기 위해 준비해야할 것들을 알아본다.

1. 빌드 및 업로드된 iOS 앱
2. 테스트 그룹

[App Store Connect](https://appstoreconnect.apple.com/)에서 로그인을 한 후 "나의 앱"으로 이동한다.

여기서 TestFlight 탭을 아래 사진과 같이 선택한다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/test2.png?raw=true" width="100%" />

이동하면 이런 페이지가 뜬다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/test1.png?raw=true" width="100%" />

모달을 닫고 먼저 테스터부터 등록해 보자.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/test3.png?raw=true" width="100%" />

"새 그룹"을 클릭하여 먼저 그룹을 생성한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/test4.png?raw=true" width="100%" />

그룹을 선택한 후 테스트할 빌드를 선택하고 추가하여 심사를 기다린다.

심사는 1 ~ 3일 정도 소요되며 심사가 끝나면 테스터들에게 메일이 발송된다.
TestFlight 앱을 설치하여 테스트를 진행하면 된다.

## 참고자료

- [dev-yakuza: iOS TestFlight](https://dev-yakuza.github.io/ko/react-native/ios-testflight/)
- [App Store Connect - TestFlight Docs](https://appstoreconnect.apple.com/)
