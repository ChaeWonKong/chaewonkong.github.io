---
layout: post
title: "[TIL] AWS S3/CloudFront에 배포된 React 앱의 Routing 설정법"
date: "2020-10-25"
categories:
  - TIL
excerpt: |
  기본적으로 React는 싱글 페이지 어플리케이션이죠. 보통 React Router를 이용해 라우팅을 합니다. 이 경우, S3와 CloudFront에 배포하였을 때, routing이 안 되는 문제가 발생할 수 있습니다. 오늘은 S3/CloudFront에서 React Router가 작동하도록 설정해봅니다. S3/CloudFront에 배포한 React 앱에서 Access Denied 에러를 해결합니다.
feature_text: |
  ## S3 및 CloudFront에 배포된 React 앱이 React Router를 사용할 수 있게 AWS 설정하기
  기본적으로 React는 싱글 페이지 어플리케이션이죠. 보통 React Router를 이용해 라우팅을 합니다. 이 경우, S3와 CloudFront에 배포하였을 때, routing이 안 되는 문제가 발생할 수 있습니다. 오늘은 S3/CloudFront에서 React Router가 작동하도록 설정해봅니다. S3/CloudFront에 배포한 React 앱에서 Access Denied 에러를 해결합니다.
feature_image: "https://images.unsplash.com/photo-1532003885409-ed84d334f6cc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80"
image: "https://images.unsplash.com/photo-1532003885409-ed84d334f6cc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80"
---

React에서 보통 react-router-dom을 이용해 라우팅을 합니다. 이 경우, 실제 route는 `index.html` 하나이죠(Single Page App이니까요).
클라이언트 쪽에서 route를 나눠서 사용할 수 있는 것은, react-router-dom 라이브러리가 도와주기 때문입니다.

따라서, **AWS S3와 CloudFront에 배포할 때엔, 이 점을 잘 고려해 주어야 특정 route로 접속했을 때 403, 404 에러가 발생하지 않습니다.**

아래 설정을 해주지 않은 상태에서 React 앱을 배포하여 사용하면, 403 에러가 발생합니다. `access denied`라는 페이지를 마주하게 되죠.
따라서, S3와 CloudFront를 이용해 배포할 때는 꼭 아래 설정을 함께 해줘야 합니다. 

오늘은 **React Router를 사용 중인 React 앱을 AWS S3와 CloudFront를 이용해 배포할 때, routing 과정에서 `access denied` 에러를 해결하는 방법**에 대해 알아봅니다.

## S3 설정하기
S3에서 "정적 웹 사이트 호스팅"을 설정할 때, 아래 사진처럼 **인덱스 문서**와 **오류 문서**를 모두 `index.html`로 설정해 주세요.

이렇게 하면, route를 찾지 못하면 `index.html`로 트래픽이 routing되어 react-router-dom의 routing을 탈 수 있게 됩니다.
<img src='https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/s3-router/1.png' alt='s3 호스팅 설정' />

## CloudFront 설정하기
CloudFront에서는 403과 404 케이스를 `index.html`로 대응하도록 연결해 주면 됩니다.
또, 오류 메세지는 `200 OK`로 수정하도록 설정해 주면 됩니다.

<img src='https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/s3-router/2.png' alt='cloudfront 에러 설정' />

위 사진처럼 CloudFront > Error Pages로 이동하여 **Create Custom Error Response** 버튼을 클릭해 403과 404를 추가해 줍니다.
<img src='https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/s3-router/3.png' alt='cloudfront 에러 설정' />

설정하는 방법은 위 사진과 같습니다.

## 참고: Firebase Hosting에서의 해결방법
Firebase에서 CLI를 이용해 hosting을 추가할 때엔,

```cmd
$ firebase init
```
이 CLI를 따라가는 과정에서
 `configure single page app`에 "yes"라고 응답함으로써 이를 해결할 수 있습니다.

이 설정을 해준 후, `firebase.json`을 확인해보면 아래와 같은 내용을 확인할 수 있습니다.
```json
"rewrites": [
  {
    "source": "**",
    "destination": "/index.html"
  }
]
```
즉 모든 요청을 `index.html`로 연결한다는 것이죠.

## 참고자료
- [_junukim, "CloudFront로 React앱 배포하기 - 2"](https://velog.io/@_junukim/CloudFront%EB%A1%9C-React%EC%95%B1-%EB%B0%B0%ED%8F%AC%ED%95%98%EA%B8%B0-2)
- [SEIA SOTO, "AWS S3와 CloudFront 그리고 React.JS, React Router 웹 사이트 배포하기"](https://typed.sh/aws-s3wa-cloudfront-cdneuro-statichan-web/)
