---
layout: post
title: "도커(Docker)를 활용해 React 앱 배포하기"
date: "2020-03-18"
categories:
  - JavaScript
excerpt: |
  Docker 및 Docker Compose를 활용해 React 앱을 배포해 본다.
feature_text: |
  ## 도커(Docker)를 활용해 React 앱 배포하기
  Docker 및 Docker Compose를 활용해 React 앱을 배포해 본다.
feature_image: "https://images.unsplash.com/photo-1568430462989-44163eb1752f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1352&q=80"
image: "https://images.unsplash.com/photo-1568430462989-44163eb1752f?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1352&q=80"
---

<img src="https://www.docker.com/sites/default/files/d8/styles/large/public/2020-01/Moby%20Group.jpeg?itok=03bZ6d6y">
> 출처: https://www.docker.com/play-with-docker

도커(Docker)는 컨테이너를 실행하고 관리할 수 있도록 도와주는 도구입니다.

아시겠지만, 환경에 따라 명령어는 조금씩 달라집니다.

mac을 쓰신다면 `brew`를 이용해 모듈을 설치하는 것에 익숙하실 겁니다.

우분투라면 `apt-get`이 익숙하실 거구요.

CentOS에서는 `yum`이 익숙하실 거예요.

이렇듯, 환경에 따라 명령어는 달라집니다.
하지만 도커는 환경과 상관없이 도구들을 설치하고 프로젝트를 진행할 수 있게 도와주죠.

더 자세한 설명은 이 글의 범위를 벗어나니, 참고 자료를 확인해 주세요.

오늘 알아볼 것들은 다음과 같습니다.

- Docker를 활용해 React를 컨테이너에서 실행하기
- Docker Compose를 활용해 React를 컨테이너에서 실행하기

자 그럼, 서론은 이 정도에서 끝내고 본격적으로 Docker를 활용해 React를 담는 컨테이너를 만들어 봅시다.

> 이 글은 [Michael Herman의 "Dockerizing a React App"](https://mherman.org/blog/dockerizing-a-react-app/)을 의역하다시피 많이 참조한 글입니다. 핵심 구현 코드는 그대로이고, 다만 설명은 더 쉽게 하기 위해 제법 많은 가감을 했습니다.

## 프로젝트 생성

이 글에서는 React로 생성하는 앱은 신경쓰지 않을 겁니다.
그것을 Docker에 담는 것, 즉 Dockerize 하는 것에 초점을 맞출 겁니다.

그러므로, 우리의 마법같은 `create-react-app`을 이용해 React 앱을 아무렇게나 하나 생성해 사용해 봅시다~

```shell
$ npx create-react-app react-dockerized
$ cd react-dockerized
```

## Docker 적용

자, 이제 Docker에 태워 봅시다.

먼저 `Dockerfile`이라는 파일을 생성해 도커 관련 설정을 진행합니다.

```docker
# base image
FROM node:12.2.0-alpine

# set working directory
WORKDIR /app

# `/app/node_modules/.bin`을 $PATH 에 추가
ENV PATH /app/node_modules/.bin:$PATH

# app dependencies, install 및 caching
COPY package.json /app/package.json
RUN npm install
RUN npm install react-scripts@3.0.1 -g

# 앱 실행
CMD ["npm", "start"]
```

그리고 `.dockerignore`에 `node_modules`를 추가해 파일 생성 합니다.
(.gitignore를 생각하시면 되겠습니다.)

자 이미지를 생성해 봅시다.

```shell
$ docker build -t react-dockerize:dev .
```

그리고 이미지를 실행하도록 할게요.

```shell
$ docker run -v ${PWD}:/app -v /app/node_modules -p 3001: 3000 --rm react-dockerize:dev
```

자 갑자기 너무 어려운 명령어들의 향연이죠?
한번 짚고 가겠습니다.

1. `docker run` 명령은 우리가 `docker build`라는 명령으로 생성한 이미지를 바탕으로 도커 컨테이너 인스턴스(Docker Container Instance)를 생성하고 실행합니다.
2. `-v ${PWD}:/app`은 React 코드를 "/app"에 존재하는 컨테이너에 마운트 합니다.(윈도우는 {PWD}가 작동하지 않을 수 있습니다.)
3. `-v /app/node_modules`는 다른 볼륨에서 "node_modules"를 사용하도록 해줍니다.
4. `-p 3001:3000`은 포트 옵션입니다. 3000번은 같은 도커 컨테이너 내에서 접근할 수 있는 포트번호, 3001번은 외부에서 접근할 수 있는 포트번호입니다.
5. `--rm` 옵션은 컨테이너가 종료되면 자동으로 삭제하라는 의미입니다.

여기까지 이해가 되셨으면, "http://localhost:3001"로 이동해 작동하는지 확인해 보세요~

## [Docker Compose](https://docs.docker.com/compose/) 사용

Docker Compose는 여러 컨테이너를 생성하는 도커 어플리케이션을 정의하고 실행하도록 돕는 도구입니다.
"YAML" 파일을 통해 어플리케이션의 설정을 정리하고, 간단한 명령으로 모든 서비스를 설정과 함께 실행할 수 있습니다.

말로만 읽어서는 감이 오지 않죠?
백견이 불여일행입니다. 한번 실행해보면 감이 옵니다.

`docker-compose.yml` 파일을 생성하고 아래 내용을 추가해 줍니다.

```yaml
version: "3.7"

services:
  sample:
    container_name: react-dockerize
    build:
      context: .
      dockerfile: Dockerfile
    volumes:
      - ".:/app"
      - "/app/node_modules"
    ports:
      - "3001:3000"
    environment:
      - NODE_ENV=development
```

위에서는

```shell
$ docker run -v ${PWD}:/app -v /app/node_modules -p 3001: 3000 --rm react-dockerize:dev
```

요런 복잡하고 긴 명령으로 도커 컨테이너를 띄웠습니다. (도커는 고래여서 물에 둥둥 뜬다고 합니다..)
보면 대충 감이 오시겠지만, 이제는 그 복잡한 명령들을 YAML 파일로 정리해 사용하는 겁니다.

Docker Compose를 활용해 Docker 이미지를 생성하고 컨테이너를 띄워 보겠습니다.

```shell
$ docker-compose up -d --build
```

이제 아까처럼 "http://localhost:3001"에 가셔서 작동하는지 확인해 주세요.

만약 작업이 끝나서 컨테이너를 내리고 싶다면,

```shell
$ docker-compose stop
```

이렇게 명령해주면 됩니다.

## 마치며

저도 회사에서 Docker와 Kubernetes를 사용해 웹프론트엔드를 배포하게 되면서 부랴부랴 Docker부터 공부하고 있는 실정입니다.
그렇습니다.
이제 프론트엔드도 도커를 알아야 하는 것입니다...ㅠ

최대한 제가 직접 실행해본 레퍼런스들을 바탕으로 정리하였지만,
워딩이나 표현 등에서 바람직하지 않은 부분이 있을 수 있습니다.
저도 공부하는 입장이라서요.

부족한 부분이 있다면 댓글로 지적해 주세요.
반영하겠습니다.

읽어주셔서 감사합니다.

## 참고자료

- [Michael Herman, "Dockerizing a React App"](https://mherman.org/blog/dockerizing-a-react-app/)
- [nacyot, "만들면서 이해하는 도커(Docker) 이미지의 구조"](https://www.44bits.io/ko/post/how-docker-image-work)
- [raccoony, "왜 굳이 도커(컨테이너)를 써야 하나요?"](https://www.44bits.io/ko/post/why-should-i-use-docker-container)
- [Overview of Docker Compose](https://docs.docker.com/compose/)
