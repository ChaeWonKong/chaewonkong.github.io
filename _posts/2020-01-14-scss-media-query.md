---
layout: post
title: "Sass/SCSS에서 미디어 쿼리(Media Query) 다루기"
date: "2020-01-14"
categories:
  - Styling
excerpt: |
  Sass/SCSS에서 변수(Variables)와 믹스인(mixins)을 활용해 미디어 쿼리(Media Query)를 다루는 방법에 대해서 알아봅니다.
feature_text: |
  ## Sass/SCSS에서 미디어 쿼리(Media Query) 다루기
  Sass/SCSS에서 변수(Variables)와 믹스인(mixins)을 활용해 미디어 쿼리(Media Query)를 다루는 방법에 대해서 알아봅니다.
feature_image: "https://images.unsplash.com/photo-1507721999472-8ed4421c4af2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
image: "https://images.unsplash.com/photo-1507721999472-8ed4421c4af2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
---

웹 개발을 하다 보면 반응형 웹을 만들어야 하는 경우가 흔하죠.

CSS로 반응형 웹을 작성할 때 우리는 미디어 쿼리를 사용합니다.

미디어 쿼리 덕분에 비교적 편하게 반응형 웹을 만들 수 있지만, 서비스가 복잡해질수록 생각보다 이는 쉬운 방법이 아니라는 걸 깨닫게 되죠.

보통 웹 애플리케이션이 복잡해지면 하나의 CSS 파일에 모든 스타일을 작성하기 보다는 여러 파일로 나눠 작성하게 됩니다.

이 경우, 매 파일마다 미디어 쿼리를 새로 작성해야 하는 불편함이 생기죠.

특히, CSS에는 변수가 없죠. 디바이스별 `min-width`나 `max-width`의 픽셀 값을 외우고 있다가 적용해야 합니다.

매우 불편하지 않을 수 없습니다.

오늘은 **SCSS를 이용해서 간편하게 미디어 쿼리를 적용하는 방법**을 살펴보겠습니다.

## Sass/SCSS의 변수와 믹스인(Variables & mixins)

먼저, **SCSS는 Sass의 상위호환(Superset)으로 CSS와 호환될 수 있는 문법**을 가지고 있습니다.

CSS와 문법이 좀 다른 Sass에 비해 SCSS는 거의 완벽히 CSS 문법과 호환이 되어 편리합니다.

우리는 SCSS를 기반으로 살펴보겠습니다.

### 변수(Variables)

변수를 사용할 수 있습니다!

```scss
$font-large: 3rem;
$font-medium: 1.6rem;
$font-default: 1.2rem;
$font-small: 0.8rem;
```

이런 식으로 말이죠.

그럼 변수는 어떻게 사용하면 될까요?

```scss
body {
  font-size: #{$font-large};
}
```

네, 이런식으로 변수로 값을 가져다 쓸 수 있습니다.

통일성 있고 일체감 있는 스타일링이 매우 편해집니다.

### 믹스인(Mixins)

믹스인은 CSS를 묶어서 재사용할 수 있는 모듈로 만들어 줍니다.

```scss
@mixin card-view {
  font-size: 22px;
  border: 1px solid grey;
  border-radius: 4px;
}
```

이렇게 선언한 믹스인은 다음과 같이 가져다 쓸 수 있습니다.

```scss
div {
  @include card-view;
}
```

컴파일하면 아래처럼 바뀌죠.

```css
div {
  font-size: 22px;
  border: 1px solid grey;
  border-radius: 4px;
}
```

## Sass/SCSS 미디어쿼리 작성

자, 이제 믹스인과 변수의 개념을 활용해 미디어쿼리를 작성해 보겠습니다.

### 변수 작성

먼저 변수를 선언하겠습니다.

우리는 모바일, 태블릿, 데스크톱 세 환경을 대응하도록 변수를 선언하겠습니다.

**\_variables.scss**

```scss
// Breakpoints
$breakpoint-mobile: 335px;
$breakpoint-tablet: 758px;
$breakpoint-desktop: 1024px;
```

Scss 파일명 앞에 언더바 `_` 가 보이시나요? 언더바를 붙여서 작성해야 파일단위로 분리되어 컴파일 되지 않습니다.

사실 `_variables.scss` 는 변수만 따로 저장해놓을 파일이기에 별도의 CSS 파일로 컴파일될 필요가 없습니다.

이런 경우, 언더바를 파일명 앞에 붙이면, 성능 및 관리 상의 이득을 얻을 수 있겠죠.

### 믹스인 작성

다음으로 미디어쿼리를 이용하기 쉽도록 믹스인으로 작성할게요.

먼저, 좀 전에 작성한 변수를 불러와(import) 줍니다.

**\_mixin.scss**

```scss
@import "./variables";
```

불러올 때는 언더바나 확장자를 제외해도 정상 작동합니다! 참 편하죠?

다음으로 본격적인 믹스인 작성을 진행해 보겠습니다.

**\_mixin.scss**

```scss
@import "./variables";

@mixin mobile {
  @media (min-width: #{$breakpoint-mobile}) and (max-width: #{$breakpoint-tablet - 1px}) {
    @content;
  }
}

@mixin tablet {
  @media (min-width: #{$breakpoint-tablet}) and (max-width: #{$breakpoint-desktop - 1px}) {
    @content;
  }
}

@mixin desktop {
  @media (min-width: #{$breakpoint-desktop}) {
    @content;
  }
}
```

자, 이렇게 모바일, 태블릿, 데스크톱에 대한 믹스인 작성을 완료했습니다.

미디어 쿼리에서 `min-width`와 `max-width`를 변수를 활용해 적용했고 미디어 쿼리 내부에는 믹스인이 사용될 때 넘겨받은 콘텐트(content)가 들어가도록 했습니다.

### 믹스인을 활용한 미디어 쿼리

자, 그럼 믹스인으로 작성한 미디어 쿼리를 사용해볼까요?

**main.scss**

```scss
@import "../../Styles/mixins";

@include mobile {
  .img-card {
    width: 100px;
  }
}

@include tablet {
  .img-card {
    width: 200px;
  }
}

@include desktop {
  .img-card {
    width: 300px;
  }
}
```

이렇게 믹스인을 사용하면 기기별로 손쉽게 미디어쿼리를 이용할 수 있습니다.

## 마치며

SCSS를 이용하면 삶이 참 편리해집니다.

React에서도 `node-sass` 만 설치하면 Sass/SCSS를 이용할 수 있어 매우 편리하죠.

앞으로는 쉽게 반응형 웹을 만들 수 있을 것 같습니다.

## 참고자료

- [HEROPY Tech, "Sass(SCSS) 완전 정복!"](https://heropy.blog/2018/01/31/sass/)
- [Eduardo Buoças, "Approaches to Media Queries in Sass"](https://css-tricks.com/approaches-media-queries-sass/)
