---
layout: post
title: "Nest.js에서 in-memory cache 사용하기"
date: "2022-06-04"
categories:
  - NestJS
excerpt: |
  Nest.js에서 in-memory cache 사용하기
feature_text: |
  ## Nest.js에서 in-memory cache 사용하기
  Nest.js에서 in-memory cache 사용하기
feature_image: "https://images.unsplash.com/photo-1543966888-7c1dc482a810?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1506&q=80"
image: "https://images.unsplash.com/photo-1543966888-7c1dc482a810?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1506&q=80"
---

캐싱은 상황에 따라 강력할 수 있다. 데이터가 자주 변하지 않는 반면, 요청이 빈번하다면 캐싱은 매우 강력해진다.

Nest.js는 in-memory cache를 정말 간단하게 사용할 수 있도록 돕는다.

먼저 `cache-manager`를 설치해 준다.

```
$ npm install cache-manager
$ npm install -D @types/cache-manager
```

다음으로 캐싱을 사용하고 싶은 controller와 module에 캐싱을 적용하면 된다.

이번에는 간단히 응답을 auto-caching하게 해본다.

```typescript
import { CacheModule, Module } from "@nestjs/common";
import { CatController } from "./app.controller";

@Module({
  imports: [CacheModule.register()],
  controllers: [CatController],
})
export class CatModule {}
```

매번 필요에 맞게 controller와 module을 선택해 설정해도 되고, AppController, AppModule에 설정해 모든 module에 적용되게 해도 된다.

```typescript
@Controller()
@UseInterceptors(CacheInterceptor) // 이 부분 주목
export class CatController {
  @Get()
  findAll(): string[] {
    return [];
  }
}
```

controller에는 위와 같이 지정해준다. 반드시 동일한 모듈의 컨트롤러에 적용을 해줘야 동작한다. (CatModule에 설정해주었다면 CatController)

## 벤치마크

최대한 실제 운용상황과 유사하게 하기 위해 docker로 컨테이너를 띄워 진행했다. Docker-compose로 nginx와 app을 동시에 띄우고 nginx가 app으로 요청을 proxy한다.

```
$ autocannon -c 100 http://localhost/review?productId=1
```

간단한 벤치마크이므로 autocannon을 사용한다.

### 캐싱을 하지 않은 경우

![no cache result](https://leonkong.cc/public/no_cache.png)

|     항목     |   결과    |
| :----------: | :-------: |
| Avg. Latency | 170.76 ms |
| Avg. Req/Sec |  578.71   |
|  total Req   |    6K     |

### 캐싱 적용을 한 경우

![in-memory cache result](https://leonkong.cc/public/with-cache.png)

|     항목     |   결과   |
| :----------: | :------: |
| Avg. Latency | 87.58 ms |
| Avg. Req/Sec |  1132.3  |
|  total Req   |   11K    |

간단한 벤치마크이므로 이를 전적으로 신뢰할 수는 없지만, 캐시가 적용된 후 초당 응답량이 2배 가까이 증가했음을 알 수 있다.

더불어 latency도 절반 정도로 줄어들었음을 알 수 있다.
