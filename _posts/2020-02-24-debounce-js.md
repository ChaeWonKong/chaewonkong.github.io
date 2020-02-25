---
layout: post
title: "JavaScript로 Debounce 함수 구현하기"
date: "2020-02-24"
categories:
  - JavaScript
excerpt: |
  JavaScript로 Debounce 함수 구현하기
feature_text: |
  ## JavaScript로 Debounce 함수 구현하기
  Lodash의 Debounce 함수를 JavaScript로 구현해 특정 시간 동안연이어 호출되는 이벤트 함수 중 가장 마지막 함수를 호출하기
feature_image: "https://images.unsplash.com/photo-1465804575741-338df8554e02?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2253&q=80"
image: "https://images.unsplash.com/photo-1465804575741-338df8554e02?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2253&q=80"
---

외부 API가 호출되는 횟수에 비례해 과금하게 되는 경우, API의 호출을 최소화할 필요가 있죠.

예를 들어, 카카오나 네이버의 API를 활용해 input에 대해 자동완성을 구현하는 경우, 자모음 하나하나가 입력될 때마다 API 요청이 이루어진다면 이는 너무 큰 비효율일 겁니다.

이런 경우, **Lodash의 debounce라는 메서드**를 이용하여 특정 시간 동안 발생된 이벤트를 묶어 가장 마지막 이벤트에 대해서만 API 요청을 진행함으로써 해결할 수 있습니다.

오늘은 이런 **debounce 함수를 직접 작성**해 봄으로써, 어떻게 동작하는지 한번 제대로 알아보려고 합니다.

매번 외부 라이브러리에서 갖다 쓰기만 하니까, 정작 생각하는 능력은 성장하지 않는 것 같다고 생각해서요.

## Lodash의 Debounce 부터 살펴보기

먼저 Lodash의 debounce 부터 간단히 살펴보고 넘어가죠.

Lodash는 JavaScript 개발자라면 한번 이상은 사용해 봤을 것입니다.
JavaScript 개발자를 위한 유틸리티 라이브러리죠. 딥카피(deep copy)와 같이 배열이나 객체를 처리할 수 있는 다양한 메서드를 지원합니다.

[Lodash - Debounce 링크](https://lodash.com/docs/4.17.15#debounce)

```javascript
_.debounce(func, [(wait = 0)], [(options = {})]);
```

보통 Lodash는 `_`로 가져와 사용합니다.

Lodash의 debounce는 leading, trailing을 options으로 받습니다.
만약 defualt는 `trailing = true` 입니다.

`_.debounce`는 `_.throttle`과 다르게 주어진 매 밀리세컨드 마다 실행되는 것이 아니라, 주어진 밀리세컨드 이내에 연속으로 이벤트가 발생할 경우, 더 이상 이벤트가 발생하지 않을 때까지 일단 대기합니다.

마지막 이벤트가 끝난 시점을 기준으로 주어진 밀리세컨드 이후 콜백으로 주어진 함수가 실행됩니다.

이벤트가 꾸준히 발생할 때, 주어진 인터벌마다 주기적으로 이벤트가 발생하게 하고 싶다면 `_.throttle`을 사용하면 됩니다.

- `leading = true` 이면 첫 이벤트 발생시 그 이벤트는 일단 반영됩니다. 이후의 연속하는 이벤트는 묶어서 처리됩니다.
- `trailing = true` 이면 연속된 이벤트의 마지막 이벤트가 발생 후 wait으로 대입된 밀리세컨트 만큼이 지난 후 마지막 이벤트 하나가 반영됩니다.

## JavaScript로 Debounce 구현하기

먼저 leading 옵션이 없는 Debounce 함수부터 살펴볼게요.

### 간단한 버전

```javascript
function debounce(func, wait) {
  let inDebounce;
  return function() {
    const context = this;
    const args = arguments;

    // setTimeout이 실행된 Timeout의 ID를 반환하고, clearTimeout()으로 이를 해제할 수 있음을 이용
    clearTimeout(inDebounce);
    inDebounce = setTimeout(() => func.apply(context, arguments), wait);
  };
}
```

네. **클로저**를 사용해 사이드이팩트인 timeout을 관리하는 구조입니다.
클로저는 **스코프체인** 상의 변수들에 접근할 수 있죠.

이벤트가 주어진 `wait` 보다 짧은 간격으로 지속해 발생하면 timeout이 계속 더 뒤의 이벤트의 발생시점을 기점으로 갱신됩니다.

### leading 옵션 추가

이제 우리가 만들 debounce 함수는 leading만을 Boolean 값으로 받아 처리합니다.
`leading = true` 라면 매 이벤트 그룹의 첫 이벤트부터 반영되는 구조죠.

```javascript
const debounce = (func, wait, leading = false) => {
  let inDebounce;
  return function() {
    const context = this;
    const args = arguments;

    // inDebounce 값이 변하기 전에 미리 저장하기 위해 사용
    let callNow = leading && !inDebounce;

    // leading이 아닌 경우에만 wait 이후 func가 실행되도록 함
    const later = () => {
      inDebounce = null;
      if (!leading) func.apply(context, args);
    };

    // setTimeout이 실행된 Timeout의 ID를 반환하고, clearTimeout()으로 이를 해제할 수 있음을 이용
    clearTimeout(inDebounce);
    inDebounce = setTimeout(later, wait);

    // 만약 leading=true이고 inDebounce가 없으면 func를 실행
    if (callNow) func.apply(context, args);
  };
};
```

구조가 제법 복잡해 졌습니다.
이제는 leading 옵션도 추가되었죠.

클로저를 사용하는 구조는 전과 동일합니다.
다만 `callNow`라는 변수와 `later()` 함수가 추가 되었습니다.

우선 `leading = true` 이면서 `inDebounce` 값이 정의되지 않은 경우,
`if (callNow) func.apply(context, args);` 문에 의해 debounce 함수가 호출될 때 즉시 우리의 `func`는 호출됩니다.

하지만, 이 과정에서 `inDebounce`에는 `setTimeout(later, wait)`의 결과로 timeout의 ID 값이 담기죠.
그래서 `wait` 이라는 인터벌 내에 동일 이벤트가 반복되면 `callNow = false`가 되어 우리가 위에서 작성한 간단한 버전의 debounce 함수처럼 작동하게 됩니다.

그럼 `inDebounce = null;` 이 부분은 왜 추가했을까요?

살펴보면 later가 호출되는 setTimeout보다 위에서 `clearTimeout(inDebounce);`로 timeout을 없애고 있죠.
따라서 `later()`가 호출되면 `inDebounce != null` 입니다.

그렇게 된다면, `wait` 이후아 이벤트가 발생하더라도 leading 이벤트가 반영되지 않게 되죠.
그래서 굳이 `inDebounce = null;`을 통해 `inDebounce`를 초기화 해 주는 것입니다!

## 간단하게 테스트 해보기

브라우저에서 버튼을 클릭하면 debounce로 버튼 클릭 이벤트를 받는 간단한 예제를 만들어 봤습니다.

**index.html**

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <button id="button">Click Me!</button>
    <p>total counts: <span id="count"></span></p>
    <script src="index.js"></script>
  </body>
</html>
```

addEventListener에 콜백으로 제공된 debounce 함수의 wait 부분의 숫자(밀리세컨드 단위)와 leading 옵션을 바꿔가며 테스트 해 보세요!

**index.js**

```javascript
const debounce = (func, wait, leading = false) => {
  let inDebounce;
  return function() {
    const context = this;
    const args = arguments;

    // inDebounce 값이 변하기 전에 미리 저장하기 위해 사용
    let callNow = leading && !inDebounce;

    // leading이 아닌 경우에만 wait 이후 func가 실행되도록 함
    const later = () => {
      inDebounce = null;
      if (!leading) func.apply(context, args);
    };

    // setTimeout이 실행된 Timeout의 ID를 반환하고, clearTimeout()으로 이를 해제할 수 있음을 이용
    clearTimeout(inDebounce);
    inDebounce = setTimeout(later, wait);

    // 만약 leading=true이고 inDebounce가 없으면 func를 실행
    if (callNow) func.apply(context, args);
  };
};

let count = 0;
let button = document.getElementById("button");
let counter = document.getElementById("count");
counter.innerText = count;

function handleClick() {
  count += 1;
  counter.innerText = count;
}

button.addEventListener("click", debounce(handleClick, 500, true));
```

## 참고 자료

- [David Corbacho, "Debouncing and Throttling Explained Through Examples"](https://css-tricks.com/debouncing-throttling-explained-examples/)
- [Jhey Tompkins, "Throttling and Debouncing in JavaScript"](https://codeburst.io/throttling-and-debouncing-in-javascript-b01cad5c8edf)
- [yhe228, "Debounce"(한글자료)](https://velog.io/@yhe228/Debounce)
- [David Walsh, "JavaScript Debounce Function"](https://davidwalsh.name/javascript-debounce-function)
