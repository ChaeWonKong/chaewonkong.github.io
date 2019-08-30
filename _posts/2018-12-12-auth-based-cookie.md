---
layout: post
title: "쿠키(Cookie) 기반의 사용자 인증(Auth)"
date: "2018-12-11"
categories:
- DevLog
excerpt: |
  쿠키 기반으로 사용자 인증을 진행하는 방법에 대해서 살펴본다.
feature_text: |
  ## 쿠키 기반의 사용자 인증
  쿠키 기반으로 사용자 인증을 진행하는 방법에 대해서 살펴본다.
feature_image: "https://images.unsplash.com/photo-1509228468518-180dd4864904?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80"
image: "https://images.unsplash.com/photo-1509228468518-180dd4864904?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80"
---
## 사용자 인증시 쿠키를 사용해야 하는 이유
사용자가 로그인 해야만 이용할 수 있는 페이지가 있다고 가정하자. 왜 쿠키를 이용해야 하는 것일까? 그것은 HTTP의 특성에 기인한다. HTTP는 Stateless이다. 즉, 서로 다른 두 리퀘스트(Request) 간에 정보나 데이터를 공유할 수 없다. 로그인을 요청하는 리퀘스트와 로그인 해야만 볼 수 있는 정보를 요청하는 리퀘스트가 동시에 발생한다고 하자. 첫번째 리퀘스트는 리스폰스(Response)로 로그인이 완료되었다는 사실을 받을 것이지만, 두번째 리퀘스트의 경우, 그 사실을 확인할 수 없다.

## 쿠키 기반 사용자 인증의 작동 방식
쿠키는 서버에서 발급되며 브라우저의 메모리에 저장된다. 로그인 시 토큰 등을 쿠키로 제공하면, 이후 브라우저를 통해 리퀘스트를 진행할 때마다 HTTP 헤더(Header)에 해당 쿠키가 포함되어 전송된다. 서버는 그 쿠키를 확인함으로써 로그인된 적합한 유저라는 사실을 확인할 수 있고, 로그인이 필요한 페이지를 보여줄 수 있게 된다.


<img src="https://github.com/ChaeWonKong/chaewonkong.github.io/blob/master/assets/post_img/auth.png?raw=true" />

위 그림은 쿠키 기반의 사용자 인증이 어떻게 작동하는지를 설명하고 있다.

로그인을 하게 되면, 서버에서 쿠키가 제공된다. 쿠키는 브라우저의 메모리에 저장되며, 이후 사용자가 접근할 때마다 HTTP 헤더로써 전송된다. 서버는 사용자가 인증되었으므로 요청된 정보를 제공하면 된다.

<img src="https://github.com/ChaeWonKong/chaewonkong.github.io/blob/master/assets/post_img/cookie.png?raw=true" />

사용자의 로그인과 로그아웃은 위 그림처럼 작동하게 된다. 로그인을 할 경우 DB에 저장된 사용자 정보와 일치하는지 여부를 확인한 후 서버가 쿠키를 발급한다. 이후, 매 Request마다 쿠키는 헤더에 포함된다.

사용자가 로그아웃하면 서버는 쿠키를 빈 문자열(=null)로 초기화한다. 이렇게되면 쿠키에 정보가 없으므로 로그인이 필요한 정보에 대한 접근 권한을 상실하게 된다.

## 쿠키 기반 사용자 인증의 장점
쿠키를 기반으로 사용자 인증을 하게 되면, 쿠키를 저장하고 헤더에 포함시켜 리퀘스트를 전송하는 역할은 브라우저가 도맡아 담당한다. 서버를 개발하는 과정에서 쿠키를 발급해 주기만 하면 서버 측에서는 별 달리 신경쓸 것이 없는 것이다.