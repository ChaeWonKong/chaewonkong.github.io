---
layout: post
title: "[NestJS] class-validator를 이용해 request에 validation pipe 적용하기"
date: "2021-09-21"
categories:
  - NestJS
excerpt: |
  서버에서 요청을 처리할 때, 종종 클라이언트가 잘못된 request body나 query param을 넘기는 경우가 있다. NestJS에서는 ValidationPipe를 이용해 이런 오류 케이스를 우아하게 핸들할 수 있다. class-validator를 이용하면 단 몇 줄의 추가적인 코드 만으로도 쉽고 간단하게 잘못된 요청에 대응할 수 있다.

feature_text: |
  ## class-validator를 이용해 전역적(globally)으로 validation pipe 적용하기
  NestJS에서 validation pipe를 global하게 사용하는 법을 알아본다. class-validator를 이용해 더 쉽고 간단하게 요청의 타입을 검증(validation)해본다. 서버에서 요청을 처리할 때, 종종 클라이언트가 잘못된 request body나 query param을 넘기는 경우가 있다. NestJS에서는 ValidationPipe를 이용해 이런 오류 케이스를 우아하게 핸들할 수 있다. class-validator를 이용하면 단 몇 줄의 추가적인 코드 만으로도 쉽고 간단하게 잘못된 요청에 대응할 수 있다.
feature_image: "https://images.unsplash.com/photo-1516382799247-87df95d790b7?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=2000&q=80"
image: "https://images.unsplash.com/photo-1516382799247-87df95d790b7?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=2000&q=80"
---

서버는 종종 클라이언트의 잘못된 요청에 대해서도 응답해야 하는 경우가 있다.
이러한 **예외처리를 정확히 해야만, 서버는 보다 견고하게 작동**하게 된다.

NestJS는 잘못된 요청들을 쉽게 검증할 수 있는 툴들을 제공한다.
바로 `ValidationPipe` 다.

```typescript
// Controller
import { ParseIntPipe } from '@nestjs/common';

@Get(':id')
async findOne(@Param('id', ParseIntPipe) id: number) {
  return this.catsService.findOne(id);
}
```

> 출처: <<https://docs.nestjs.com/pipes>

이렇게 각각의 route handler에 validation 로직을 넣을 수도 있다.
이것만으로도 이미 너무 편리하다.
이 경우, 만약 `id` 값으로 잘못된 타입의 값이 전달될 경우, 아래처럼 오류 메세지가 클라이언트로 전달된다.

`id` 값으로 'abc'가 전달된 경우,

```shell
GET localhost:3000/abc
```

응답:

```json
{
  "statusCode": 400,
  "message": "Validation failed (numeric string is expected)",
  "error": "Bad Request"
}
```

잘 작동한다.
하지만 굳이 찾자면 아직 **불편한 점들**이 있다.

1. 매번 request handler를 만들 때 validation 로직이 추가되어야 한다.
2. request로 제공되는 param이 복잡한 구조일 경우, validation 로직도 복잡해져야 한다. 심지어 별도의 custom validation pipe가 필요할 수도 있다.

NestJS는 이런 문제를 해결할 수 있도록 `joi` 라는 라이브러리를 이용해 schema 기반의 validation을 할 수 있게 돕고 있다.
또, `class-validator` 를 이용해 decorator 기반으로 간편하고 강력하게 validation을 할 수 있게 지원하고 있다.

오늘은 `class-validator` 를 이용해, 편하고 강력하게 validation을 할 수 있는 방법을 알아보고자 한다.

## class-validator

[class-validator](https://github.com/typestack/class-validator)

**데코레이터와 데코레이터를 사용하지 않는 방식 모두로 validation을 할 수 있도록 지원하는 라이브러리**다.
워낙 많은 validation 데코레이터를 지원하지만, 몇가지만 소개하자면 다음과 같다.

`@IsEmpty()`, `@IsIn(values: any[])`, `@IsNumber(options: IsNumberOptions)`, `@IsEnum(entity: object)`, `@IsDateString()`, `@IsNumberString(options?: IsNumericOptions)`, `@IsCurrency(options?: IsCurrencyOptions)`, `@IsHexColor()`, `@IsISIN()`...

> [지원하는 Validation 데코레이터 전체 보기](https://github.com/typestack/class-validator/blob/develop/README.md#validation-decorators)

웬만한 것들은 다 지원한다고 보면 된다. 심지어 커스텀 validator도 만들어 쓸 수 있다.

NestJS와도 나름 찰떡궁합이어서 이것을 써 보기로 했다. NestJS의 창조주?인 [Kamil Mysliwiec이 직접 제공하는 튜토리얼 영상](https://vimeo.com/447088958)도 있어 쉽게 접근 가능하다.

## NestJS에 class-validator 적용

이제 본격적으로 NestJS에 `class-validator` 를 적용해 보자. 매번 controller에 적용하기 보다는, 전역 설정으로 등록해 사용하는 쪽으로 알아본다.

### 1. 전역(global) 설정으로 ValidationPipe 추가

전역 스코프에서 validation pipe가 동작하도록 하려면, `useGlobalPipes` 라는 메서드를 이용해 `ValidationPipe` 를 `bootstrap()` 함수에 설정해 주어야 한다.

앱의 entry point가 되는 `main.ts` 파일을 열고 `ValidationPipe` 를 import한다.

```typescript
import { ValidationPipe } from "@nestjs/common";
```

다음으로, `app`이 `ValidationPipe`를 사용하도록 `bootstrap()` 함수 내에 아래 코드를 삽입한다.

```typescript
app.useGlobalPipes(new ValidationPipe());
```

완성된 코드는 아마 아래와 유사할 것이다.

```typescript
import { ValidationPipe } from "@nestjs/common";
import { NestFactory } from "@nestjs/core";
import { AppModule } from "./app.module";

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
  app.useGlobalPipes(new ValidationPipe());
}
bootstrap();
```

### 2. module에 ValidationPipe 추가

다음으로 module에 `ValidationPipe` 를 추가해 준다.
왜 이 작업을 해야 하는지에 대해서는 NestJS에서 친절히 설명을 해주고 있다.

> 의존성 주입과 관련하여 모듈 외부에서 등록된 전역 파이프(위의 예에서와 같이 useGlobalPipes() 사용)는 바인딩이 모듈 컨텍스트 외부에서 수행되었으므로 종속성을 주입할 수 없습니다.
> 출처: [https://docs.nestjs.kr/pipes#global-scoped-pipes](https://docs.nestjs.kr/pipes#global-scoped-pipes)

설정하는 방법은 간단하다.
먼저 필요한 모듈들을 import한다.

```typescript
import { ValidationPipe } from "@nestjs/common";
import { APP_PIPE } from "@nestjs/core";
```

다음으로 module의 provider로 `ValidationPipe` 를 추가해준다.

```typescript
@Module({
  providers: [
    {
      provide: APP_PIPE,
      useClass: ValidationPipe,
    },
  ],
})
```

이 작업을 끝낸 후의 실제 코드는 아래와 같았다.

```typescript
import { Module, ParseIntPipe, ValidationPipe } from "@nestjs/common";
import { AppController } from "./app.controller";
import { AppService } from "./app.service";
import { HttpModule } from "@nestjs/axios";
import { APP_PIPE } from "@nestjs/core";
@Module({
  imports: [HttpModule],
  controllers: [AppController],
  providers: [
    AppService,
    {
      provide: APP_PIPE,
      useClass: ValidationPipe,
    },
  ],
})
export class AppModule {}
```

### 3. DTO에 class-validator 추가해 validation 구현

이제 설정(configuration)은 끝났다. 사용만 하면 된다.
어떻게 사용하냐고?

DTO 클래스에 아래 코드 예시처럼 추가해 사용하면 되는 거다.

```typescript
import { IsIn, IsNumberString } from "class-validator";

export class CandleDto {
  @IsIn(MARKET_KEYS)
  market: MARKETS;

  @IsNumberString()
  minutes?: number;

  @IsNumberString()
  count?: number;
}
```

간단히 설명하자면, `MARKET_KEYS`는 `string[]`다. 만약 request로 온 `market`이 `MARKET_KEYS` 배열에 없는 문자열이라면 에러가 발생한다.

`@IsNumberString()`은 number로 변환할 수 있는 문자열(string)인지 여부를 본다.
`123`은 가능하고, `abc`나 `123a`는 에러가 발생하게 된다.

이게 끝이다.
DTO에서만 설정해 주면 나머지는 알아서 프레임워크가 다 한다.

참고로 내 컨트롤러는 아래와 같은 코드로 되어 있다.

```typescript
  @Get('candles/minutes')
  async getMinuteCandles(@Query() query: CandleDto): Promise<MinuteCandle[]> {
    const { market, minutes, count } = query;

    const res = await this.appService.getMinuteCandles(market, minutes, count);

    return res.data;
  }
```

클라이언트는 `CandleDto`를 Query Param으로 추가해 GET 요청을 한다.
서버는 Query Param을 destructuring해 `const { market, minutes, count } = query;`와 같이 필요한 param을 발라낸다.
이어서 service 단에서 제3의 API로 market, minutes, count를 포함한 요청을 보내고 응답값을 클라이언트에게 응답으로 제공한다.

### validation 테스트

자, 이제 **잘못된 값을 요청하면 어떤 결과가 돌아오는지** 테스트 해보자.

#### 1. count에 'abc'를 넣은 경우

count는 `@IsNumberString()` 데코레이터가 붙어 있으므로, `'123'`과 같이 number로 변환할 수 있는 숫자형태의 데이터만 받을 수 있다.
만약 query string으로 `count=abc`를 넘기면 어떤 응답이 오는지 시도해 보았다.

```shell
{
    "statusCode": 400,
    "message": [
        "count must be a number string"
    ],
    "error": "Bad Request"
}
```

#### 2. market에 `MARKET_KEYS`에 존재하지 않는 문자열을 대입한 경우

```typescript
const MARKET_KEYS = ["KOSPI", "KOSDAQ"];
```

`MARKET_KEYS`에는 `'KOSPI'`, `'KOSDAQ'`만 있다. ('NASDAQ'은 없음)
만약 query string으로 `market=NASDAQ`를 넘기면 어떤 응답이 오는지 시도해 보았다.

```shell
{
    "statusCode": 400,
    "message": [
        "market must be one of the following values: KOSPI, KOSDAQ"
    ],
    "error": "Bad Request"
}
```

`'NASDAQ'`는 없다고 잘 에러로 알려주고 있다.

### 3. 서로 다른 2개의 param이 잘못된 타입으로 전달될 경우

만약 market도 잘못 오고, count도 잘못 온다면?
`market=NASDAQ&count=abc`와 같이 query string을 짠다면 어떻게 될까?
즉, 서로 다른 2개의 param이 잘못된 타입으로 전달될 경우 어떻게 될까?

```shell
{
    "statusCode": 400,
    "message": [
        "market must be one of the following values: KOSPI, KOSDAQ",
        "count must be a number string"
    ],
    "error": "Bad Request"
}
```

두 에러를 모두 응답으로 반환한다.
클라이언트는 따라서 무엇이 잘못되었는지를 명확히 인지할 수 있게 된다.

## 결론

NestJS에서 `class-validator`를 사용하는 방법을 알아보았다. **전역 스코프로 한 차례만 모듈을 등록하고, main에 설정해 두면 이후에는 간단하게 DTO를 작성하면서 데코레이터 기반으로 타입을 강제할 수 있어 편리**하다.

NestJS를 며칠째 사용해 보고 있는데, 생각보다 강력하다. Node의 단점이라고 생각했던 많은 부분들이 NestJS를 사용하면서 해소되는 느낌이다. 마치 개비스콘 짤이 생각난다.
<img width='100%' src='http://health.chosun.com/site/data/img_dir/2018/04/12/2018041202376_0.jpg'>

아직은 NestJS를 엔터프라이즈 레벨에서 사용해도 될지 검토하고 있다.
하지만, 충분히 강력하고 충분히 탄탄한 것은 맞는 것 같다.

## 참고자료

- [NestJS 한글 문서-global-scoped-pipes](https://docs.nestjs.kr/pipes#global-scoped-pipes)
- [class-validator](https://github.com/typestack/class-validator/blob/develop/README.md)
- [NestJS 인강 by 창조주 Kamil Mysliwiec](https://vimeo.com/447088958)
