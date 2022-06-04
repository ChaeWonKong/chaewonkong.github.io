---
layout: post
title: "[TIL] AWS S3 presigned URL을 위한 이미지 업로드"
date: "2022-04-25"
categories:
  - TIL
excerpt: |
  AWS S3 presigned URL을 위한 이미지 업로드
feature_text: |
  ##  AWS S3 presigned URL을 위한 이미지 업로드
  서버에서 파일을 처리할 필요 없이, 클라이언트에서 바로 S3로 파일을 업로드하는 방법 (multer 필요 X)
feature_image: "https://images.unsplash.com/photo-1554801857-d08d42e23218?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2340&q=80"
image: "https://images.unsplash.com/photo-1554801857-d08d42e23218?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2340&q=80"
---

## AWS S3의 presigned URL을 이용해 이미지나 파일을 업로드하자

![presigned url 동작원리](https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/presigned.jpeg?raw=true)

multer/s3를 이용해 서버를 경유해 파일을 업로드하면 모든 파일 데이터가 서버를 거치게 되어, 서버에 부하가 생기고 네트워크 비용이 증가한다.

presigned URL은 S3에서 객체가 업로드될 장소를 URL로 미리 지정해 내려주고,
client에서 해당 지정 URL로 객체를 업로드하는 개념으로,
파일 데이터를 서버에서 처리할 필요가 없다.

클라이언트에서 직접 presigned URL을 발급받는 것도 가능하다.
다만, 보안 상 AWS에 대한 접근은 서버에서 최대한 담당하고, API 형태로 클라이언트에 제공하는 것이 옳다.

따라서 위 그림과 같은 아키텍처를 일반적으로 사용하는 것 같다.

클라이언트에서 파일 업로드를 하려할 때, 서버에 presigned URL을 요청하고,
서버는 AWS S3에 접근해 presigned URL을 생성하고 클라이언트에 응답으로 제공한다.

클라이언트는 AWS S3의 presigned url을 이용해 파일을 업로드한다.

### TODO

presigned URL은 기간한정 URL이지만, 해당 기간 내에는 얼마든지 해당 URL로 파일을 재업로드하고 덮어쓸 수 있다.
따라서 보안이슈가 있을 수 있다. 제3자가 URL을 탈취하면 전혀다른 파일을 업로드할 수도 있는 것이다.

따라서 lambda@edge를 이용해 한번 업로드되면 다시는 수정이 불가능하도록 적용하기도 하는 것 같다.
이 부분은 추가 공부가 필요하다.
