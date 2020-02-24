---
layout: post
title: "JavaScript로 Debounce 함수 구현하기"
date: "2020-02-24"
categories:
  - Node.js
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

이런 경우, Lodash의 debounce라는 메서드를 이용하여 특정 시간 동안 발생된 이벤트를 묶어 가장 마지막 이벤트에 대해서만 API 요청을 진행함으로써 해결할 수 있습니다.

오늘은 이런 debounce 함수를 직접 작성해 봄으로써, 어떻게 동작하는지 한번 제대로 알아보려고 합니다.

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

우리가 만들 debounce 함수는 leading과 trailing 옵션은 없습니다.
연속된 이벤트의 가장 마지막 이벤트만 반영되는 간단한 구조죠.

```javascript
const debounce = (func, delay) => {
  let inDebounce;
  return function() {
    const that = this;
    const args = arguments;

    // setTimeout이 실행된 Timeout의 ID를 반환하고, clearTimeout()으로 이를 해제할 수 있음을 이용
    clearTimeout(inDebounce);
    inDebounce = setTimeout(() => func.apply(that, args), delay);
  };
};
```

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

addEventListener에 콜백으로 제공된 debounce 함수의 delay 부분의 숫자(밀리세컨드 단위)를 바꿔가며 테스트 해 보세요!

**index.js**

```javascript
const debounce = (func, delay) => {
  let inDebounce;
  return function() {
    const that = this;
    const args = arguments;

    // setTimeout이 실행된 Timeout의 ID를 반환하고, clearTimeout()으로 이를 해제할 수 있음을 이용
    clearTimeout(inDebounce);
    inDebounce = setTimeout(() => func.apply(that, args), delay);
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

button.addEventListener("click", debounce(handleClick, 500));
```

## 참고 자료

- [David Corbacho, "Debouncing and Throttling Explained Through Examples"](https://css-tricks.com/debouncing-throttling-explained-examples/)
- [Jhey Tompkins, "Throttling and Debouncing in JavaScript"](https://codeburst.io/throttling-and-debouncing-in-javascript-b01cad5c8edf)
- [yhe228, "Debounce"(한글자료)](https://velog.io/@yhe228/Debounce)
- [David Walsh, "JavaScript Debounce Function"](https://davidwalsh.name/javascript-debounce-function)
