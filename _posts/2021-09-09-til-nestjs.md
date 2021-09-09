---
layout: post
title: "[TIL] NestJS를 더 깊게 이해하기 위하여"
date: "2021-09-09"
categories:
  - TIL
excerpt: |
  NestJS는 보다 효율적이고 확장가능한 서버사이드 NodeJS 어플리케이션 프레임워크다. NestJS를 깊게 알고자 한다면, IoC, DI, Clean Architecture, Node/ExpressJS를 알면 좋다.
feature_text: |
  ## NestJS를 더 알고싶다면 공부해야할 것들
  NestJS는 보다 효율적이고 확장가능한 서버사이드 NodeJS 어플리케이션 프레임워크다. NestJS를 깊게 알고자 한다면, IoC, DI, Clean Architecture, Node/ExpressJS를 알면 좋다.
feature_image: "https://images.unsplash.com/photo-1514888286974-6c03e2ca1dba?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1927&q=80"
image: "https://images.unsplash.com/photo-1514888286974-6c03e2ca1dba?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1927&q=80"
---

NestJS를 공부하고 있다.

Node/Express를 사용하며 느낀 불편함 때문이다.

뛰어난 개발자라면, 분명 자신만의 훌륭한 아키텍처를 가지고 있을 것이지만 아쉽게도 나는 그런 훌륭한 개발자가 아니다.

여전히, Express로 복잡한 서비스를 효율적이고 확장이 용이하게 구조화 하는 것에 어려움을 느낀다.

이런 나에게 NestJS는 참 단비같은 존재다.

NestJS는 SpringMVC 혹은 AngularJS와 좀 비슷하다.

보다 엄격한 프레임워크고 IoC, DI등을 활용하며 클린 아키텍처가 녹아 있다.

보통의, 평범한 개발자가 충분히 프레임워크를 숙지만 한다면 나름 평균적인 아웃풋을 낼 수 있다는 것이 이러한 프레임워크의 장점이라 생각한다. 즉, 나같은 개발자에게 딱 맞는다는 얘기다.

## 공부할거리

프론트엔드 개발자이며, ReactJS를 주로 사용해왔던 나에게, DI나 IoC는 조금 생소한 개념이었다. 클린 아키텍처의 경우 React에서 다뤄본 경험이 있지만, 회사에서 이미 존재하는 코드를 해석하고 유지보수하는 정도였지 스스로 구현해 본 것은 아니었다.

물론, 적당히 문서를 읽고 블로그 몇개 찾아보며 NestJS로 그럴싸한 서버 어플리케이션을 만들어낼 수는 있다.

하지만, 그런 식으로는 코더를 뛰어넘어 엔지니어가 될 수 없다는 것을 이제는 얼추 아는 것 같다.

이제는 철이 좀 들었기로, 모르는 개념들이 나오면 더 깊게 파고들어 이해될 때까지 쫓아가는 법도 배웠다.

## 의존성 주입? 제어 역전?

사실 Java나 Spring 경험이 없고, AngularJS나 Android를 접해보지도 않은 나에게 꽤나 생소한 개념이었다.

그런만큼 공부가 필요하다.

아직 글로 정리할 만큼 "의존성은 이것이지", "제어 역전(IoC; Inversion of Control)은 이런 의미지"를 명확하게 이해하지는 못한 것 같다. 정확히 알아야 타인을 정확히 이해시킬 수 있다. 아직 나의 앎은 완벽하지 않기에, 스스로도, 타인에게도 납득되지 않는 설명만 가능하다.

찾아보며 참고할 자료들을 정리해본다.

김종하 님의 **WiseWiredBooks** 에 NestJS가 정리되어 있는 것을 많이 참고하며 공부했다. 의존성 주입과 IoC 개념의 이해에 큰 도움이 되었다. [쉽게 풀어 쓴 Nest.js - 프로바이더](https://www.wisewiredbooks.com/nestjs/overview/04-provider.html)

제어 역전은 "나 대신 프레임워크가 제어한다"는 의미라고 한다.

그리고 의존성 주입(Dependency Injection)이라는 개념은 클래스 안에서 인스턴스를 생성해 쓰지 않고, 프레임워크가 인스턴스를 관리하며 클래스 내에서 필요할 경우, 이를 프레임워크가 주입해주는 개념이다.

의존성을 주입하는 데에는 아래처럼 3가지 방법이 있다.

- 생성자를 이용한 의존성 주입(Constructor Injection)
- 수정자를 이용한 의존성 주입(Setter Injection)
- 필드를 이용한 의존성 주입(Field Injection)

NestJS에서는 생성자를 통해 의존성을 주입하는 것을 추천한다.

실제로 어떤식으로 DI가 진행되는 것인지, 와닿지가 않을 때는 아래 글을 보면 좋다.

- [Mastering Angular dependency injection with @Inject, @Injectable, tokens and providers](https://ultimatecourses.com/blog/angular-dependency-injection)

Angular를 기반으로 설명하고 있는 것이지만, 애초에 NestJS가 AngularJS를 참고해 만들어진 것이므로 이해하는 데에는 큰 어려움은 없다.

이 글에서는 컴파일이 완료된 결과 코드를 보여줌으로써 어떻게 프레임워크가 의존성을 관리하고 적절히 주입하는지 알아갈 수 있었다.

참고로, spring을 이해하면NestJS의 DI와 IoC는 보다 쉽게 이해할 수 있는 것 같다.

- [Spring Framework Documentation - 1. The IoC Container](https://docs.spring.io/spring-framework/docs/current/reference/html/core.html#beans)

spring을 제대로 공부하지 않았다도 도움되는 부분이 있을 것 같아 메모.

## 클린 아키텍처

어쩌다가 클린 아키텍처까지 가게 되었을까?

사실 그것은 어찌보면 당연한 일이었다.

NestJS를 쓰는 목적 자체가 효율적이고 확장이 용이한 아키텍처를 NodeJS 기반으로 구축해보기 위해서였기 때문이다.

NestJS에도 많은 클린 아키텍처 개념들이 도입되어 사용되고 있는 것 같았다.

(사실 아직 클린 아키텍처를 제대로 공부하지 않았기에 확실한 것은 아니다)

결국 NestJS도 도구이고, 목적은 아니다. 목적은 응집도 높고, 결합도 낮은, 소위 [SOLID 원칙](<https://ko.wikipedia.org/wiki/SOLID_(%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%EC%84%A4%EA%B3%84)>)에 부합하는 코드를 작성하는 것. 보통의, 평범한 개발자가 적절한 훈련을 통해 안정적인 코드를 작성할 수 있게 하는 것. 이것이 난 목적이라고 생각했다.

그런 측면에서 클린 아키텍처는 공부해볼만 하다고 생각했다.

먼저 매운맛.

엉클 밥(밥 아저씨)으로 불리는 Richard C. Martin의 블로그를 정리해둔다.

- [The Clean Architecture](https://blog.cleancoder.com/uncle-bob/2012/08/13/the-clean-architecture.html)

원서도 구매했다. 다음주 중에 공부를 시작할 수 있을 것 같다.

다음으로는 한국 분들이 정리해 놓은 것들도 좀 봤다.

국산 앨런 님의 블로그가 도움이 되었다.

- [Clean Architecture란?](https://k-elon.tistory.com/38)

아직 클린 아키텍처는 어렵다. 왜 필요한 것인지는 이해했지만, SOLID 원칙 조차 나에겐 이해하기 버겁다.

꾸준히 공부해 나가며 미지의 영역을 밝혀 나가야 겠다.
