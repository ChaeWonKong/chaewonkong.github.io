---
layout: post
title: "[TIL] Nest.js - Middleware"
date: "2021-09-14"
categories:
  - TIL
excerpt: |
  Nest.js에서 미들웨어에 대해 documentation을 정독했다. 기본적으로 express의 미들웨어와 동일한 컨셉이지만, DI와 같이 Nest.js에 필요한 컨셉들이 추가되어 있다.
feature_text: |
  ## Nest.js - Middleware
  Nest.js에서 미들웨어에 대해 documentation을 정독했다. 기본적으로 express의 미들웨어와 동일한 컨셉이지만, DI와 같이 Nest.js에 필요한 컨셉들이 추가되어 있다.
feature_image: "https://images.unsplash.com/photo-1514888286974-6c03e2ca1dba?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1927&q=80"
image: "https://images.unsplash.com/photo-1514888286974-6c03e2ca1dba?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1927&q=80"
---

> 미들웨어는 route handler가 호출되기 직전에 실행되는 함수를 말한다. 미들웨어 함수는 request, response 객체, `next()` 미들웨어 함수에 접근할 수 있다.
>
> [Nest.js - Middleware](https://docs.nestjs.com/middleware)

Express.js 문서에 따르면 미들웨어는 아래와 같은 기능들을 할 수 있다.

> - 어떠한 코드든 실행 가능하다
> - request, response 객체를 변경할 수 있다
> - request-response 사이클을 종료할 수 있다
> - 스택에 있는 next 미들웨어 함수를 호출할 수 있다
> - 만약 실행되는 미들웨어 함수가 request-response 사이클을 종료하지 않는다면, 반드시 `next()` 를 호출해 제어를 next 미들웨어 함수에 넘겨야 한다. 그렇지 않는다면 request는 종료되지 않은 채 남겨진다.
>
> [Express.js - Middleware](https://expressjs.com/en/guide/using-middleware.html)

Nest.js에서 미들웨어는 `@Injectable()` 데코레이터를 붙여 클래스형으로 구현할 수도 있고, 그냥 함수형으로도 구현할 수 있다.

Dependency가 필요 없는 상황이라면 단순한 함수형 미들웨어를 고려하라고 한다.

클래스의 경우 `NestMiddleware` 인터페이스를 도입해야 한다.

Nest.js의 미들웨어는 DI(Dependency Injection)을 완벽하게 지원한다.

프로바이더나 컨트롤러를 구현할 때 처럼, `constructor` (생성자함수)를 이용해 주입한다.

미들웨어는 `configure()` 메서드를 이용해 모듈 클래스에 설정을 등록한다.

당연히 미들웨어는 원하는 route를 특정해 bind 할 수 있다. `forRoutes()` 를 체이닝해 활용한다.

미들웨어 컨슈머는 헬퍼 클래스로 미들웨어를 관리하기 위한 몇가지 빌트인 메서드들을 제공한다. 빌트인 메서드들은 fluent style처럼 메서드 체인 형태로 이용 가능하다.

## References

- [Nest.js - Middleware](https://docs.nestjs.com/middleware)
- [Express.js - Middleware](https://expressjs.com/en/guide/using-middleware.html)
