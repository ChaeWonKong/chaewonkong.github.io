---
layout: post
title: "React Native - App Store Connect에 스크린샷 업로드시 잘못된 GeoJSON 에러"
date: "2019-10-02"
categories:
  - React-Native
excerpt: |
  React Native - 크롬에서 App Store Connect에 앱 미리보기 및 스크린샷 업로드시 잘못된 GeoJSON 에러가 발생하는 경우 해결 방법을 알아본다.
feature_text: |
  ## React Native - App Store Connect에 스크린샷 업로드시 잘못된 GeoJSON 에러
  React Native - 크롬에서 App Store Connect에 앱 미리보기 및 스크린샷 업로드시 잘못된 GeoJSON 에러가 발생하는 경우 해결 방법을 알아본다.
feature_image: "https://cdn.pixabay.com/photo/2019/03/24/21/15/hermes-4078702_960_720.jpg"
image: "https://cdn.pixabay.com/photo/2019/03/24/21/15/hermes-4078702_960_720.jpg"
---

App Store Connect에 스크린샷 업로드시 잘못된 GeoJSON 에러가 발생하는 경우 해결 방법을 알아본다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/geojson.png?raw=true" height="450px" />

크롬에서 App Store Connect에 접속해 "앱 미리보기 및 스크린샷"을 업로드 하는 과정에서 위와 같은 에러가 발생했다.

```
잘못된 GeoJSON: 라우팅 앱 적용 범위 파일이 잘못되었습니다. 자세한 내용은 다음의 가이드를 참조하십시오. Location and Maps Programming Guide
```

## 해결방법

크롬에서 접속한 경우에 발생한 버그로, Safari에서 접속하여 재시도하면 정상적으로 업로드 된다.

## 참고자료

- [게으른 부엉이의 개발노트 - 잘못된 GeoJSON 애플 앱 이미지 첨부 시 에러 해결 방법](https://lazyowl.tistory.com/23)
