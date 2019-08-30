---
layout: post
title: "자바스크립트 함수 내 객체의 변경과 할당"
date: "2018-12-13"
categories:
- JavaScript
excerpt: |
  자바스크립트(JavaScript)의 함수에서 객체를 변경하거나 할당하면 함수 밖의 객체가 어떻게 변화하는지 알아본다.
feature_text: |
  ## 객체는 참조타입
  자바스크립트의 객체의 특성을 파악한다.
feature_image: "https://images.unsplash.com/photo-1488998427799-e3362cec87c3?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
image: "https://images.unsplash.com/photo-1488998427799-e3362cec87c3?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
---

## 참조 타입으로서의 객체
자바스크립트의 객체는 참조 타입이다. 

```javascript
const a = {message: "초기값"};
const b = a;

console.log(b.message);
// 초기값

b.message="수정됨";
console.log(b.message);
// 수정됨

console.log(a.message);
// 수정됨
```

왜 위와 같은 현상이 발생할까? 변수 a와 b는 모두 같은 객체를 참조하고 있기 때문이다. 즉, b에서 message를 "수정됨"으로 변경하게 되면 b가 참조하고 있던 객체 그 자체가 변하게 된다. 변수 a는 그 객체의 참조 타입이므로 객체가 바뀐 결과 a의 출력 결과 또한 변하게 되는 것이다.

## 함수 내에서의 객체
참조타입인 객체가 함수를 만나면 어떻게 될까? 더욱 예측 불가능한 상황들을 이해해 보자.

```javascript
function f(o) {
    o.message="수정함";
    o  = { message: "할당함" };
    console.log(o.message);
}

let o = { message: "초기값" };
console.log(o.message);
f(o);
console.log(o.message);
```

위 스크립트의 실행 결과는 어떻게 될까? 우선 결과를 살펴보며 이야기해보자.

```
> 초기값
> 할당함
> 수정함
```

왜 이런 결과가 나온 것일까?

먼저 스크립트의 실행 순서를 살펴보자. 함수가 가장 먼저 선언되었으나, 첫번째 console.log는 함수의 호출 전에 발생했다. 

```javascript
let o = { message: "초기값" };
```

o에 { message: "초기값" }이 담기고 그것이 출력되었으므로 당연히 첫 출력값은 "초기값"이다.

이후 함수가 호출되었다. 함수 안을 살펴보자.

```javascript
o.message="수정함";
```

우선 매개변수 o가 참조하는 객체의 message가 "수정함"으로 변경되었다.

```javascript
o = { message: "할당함" };
```

새로운 객체 { message:"할당함" }가 o에 할당되었다. 이는 새로운 객체이다.

```javascript
console.log(o.message);
```

o.message를 출력한다. 만약 함수가 호출되게 되면 o에는 새로운 객체 { message: "할당함" }이 할당되어 있으므로 "할당함"이 출력될 것이다.

이제 남은 코드들을 살펴보자.

```javascript
f(o);
console.log(o.message);
```

함수가 호출되었다. 앞서 살펴보았듯 함수 내에서 매개변수 o가 참조하던 객체는 { message: "수정함" }으로 변경되었다. 즉 let o = { message: "초기값" }으로 선언되었던 객체의 내용이 바뀌었다.

한편, 함수 내에서 선언된 변수나 객체는 함수 밖에 영향을 미치지 못하므로, o = { message: "할당함" }은 함수 스코프 외에서 유효하지 않다. 따라서 함수를 호출한 후 o.message를 출력하면 함수 내에서 변경된 객체의 내용, 즉 "수정함"이 출력되게 된다.

## 마치며
자바스크립트의 객체가 참조타입으로서 작동하는 방식은 차분히 따라가며 생각해보면 이해할 수 있다. 그러나 직관적이지는 않기에 지속적인 반복 사용과 훈련이 아니면 이해하기 어려울 수 있는 주제이기도 하다.

이러한 자바스크립트의 특징을 간과한 채 직관에 따라 개발을 하다 보면 상상하지도 못한 문제에 봉착하게 되기도 한다. 기본에 충실해야 하는 이유다.