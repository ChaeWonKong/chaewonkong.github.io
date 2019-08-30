---
layout: post
title: "JavaScript 참조 타입과 원시 타입"
date: "2018-12-07"
categories:
- JavaScript
excerpt: |
  JavaScript에서 초심자를 헷갈리게 만드는 참조 타입과 원시 타입에 대해 알아본다.
feature_text: |
  ## JavaScript의 참조 타입과 원시 타입
  자바스크립트에서 초심자를 헷갈리게 만드는 참조 타입과 원시 타입에 대해 알아본다.
feature_image: "https://picsum.photos/2560/600?image=733"
image: "https://picsum.photos/2560/600?image=733"
---

## JavaScript의 변수와 상수
ES6 부터는 var 대신 let을 사용한다. let을 통한 변수의 선언은 한 번만 가능하다. let으로 선언한 변수에는 여러 값을 할당할 수 있다.

```javascript
let tempC;
tempC = 27.5;
tempC = 14.5;
console.log(tempC);
// 14.5
```
tempC라는 변수를 선언했다. 초깃값을 할당하지 않으면 암시적으로 undefined가 할당된다. 여기서 tempC는 변수이므로 여러 값을 할당해 저장할 수 있다. 다만 var과는 다르게 같은 식별자로 두번 선언하는 것은 불가능하다.

```javascript
let tempC = 27.5;
let tempC = 22.5;

// SyntaxError: Identifier 'tempC' has already been declared
```

다음으로 상수(Constant)에 대해 살펴본다. JavaScript에서 상수는 const로 선언한다. 상수는 불변성(immutable)을 가진다. 즉, 바꿀 수 없다.

```javascript
const a = 12;
a = 14;
// TypeError: Assignment to constant variable.
```
상수로 선언한 식별자에 다른 값을 할당하면 에러가 발생한다.

다만 객체의 경우엔 조금 특별하다.
```javascript
const obj = {};
obj.a = 14;
console.log(obj);
// {a: 14}
```
분명 const로 obj를 선언했는데 a라는 키에 14라는 값을 할당했다. 이는 JavaScript의 객체가 원시 타입이 아닌 참조 타입이라서 그렇다.

JavaScript에서 객체의 식별자는 객체 그 자체가 아니라 객체라는 '달'을 가리키는 '손가락'이다. 손가락이 가리키는 방향은 결코 달라져선 안된다. 하지만 손가락이 가리키는 것(달) 그 자체는 바뀔 수 있다.

따라서 obj가 가리키는 빈 객체 {}는 바뀔 수 있다. 다만 다음은 불가능하다.
```javascript

const obj = {};
obj = {a: 4};
// TypeError: Assignment to constant variable.
```
에러가 발생한 이유는 간단하다. 상수로 선언된 obj에 다른 값을 대입하려고 했기 때문이다. 즉, 달을 가리키던 손가락으로 태양을 가리키려고 했기 때문이다.

## 원시 타입과 참조 타입
원시 값은 불변이고, 원시 값을 복사하고 전달할 때는 값 자체를 복사 및 전달한다. 따라서 원본이 바뀌더라도 사본은 바뀌지 않는다.

```javascript
let a = 1;
let b = a;
a = 'korea';
console.log(b);
// 1
console.log(a);
// korea
```
함수 안에서 변수의 값이 바뀌더라도 함수 외부에서는 바뀌지 않은 상태로 존재한다.
```javascript
const change = (a) => {
  a = 5;
}

a = 3;
change(a);
console.log(a);
// 3
```

한편, 객체는 참조 타입이다. 객체는 원본을 가리키는 손가락으로, 원본이 바뀌면 객체 자신에 담긴 값도 바뀐다.
```javascript
let obj = {a: 1};
let copied = obj;
obj.a = 'korea';
console.log(copied);
// {a: 'korea'}
```
참조하던 원본의 a라는 key의 값이 1에서 'korea'로 바뀌었다. obj를 복사한 copied 역시 이에 따라 {a: 'korea'}로 변화한 것을 알 수 있다.

한편 객체를 가리키는 변수(혹은 상수)는 객체를 가리키는 손가락일 뿐, 결코 객체 자체가 될 수 없다. 즉, 다음은 거짓이다.
```javascript
let obj = {a: 1};
obj === {a: 1};
// false
```

마지막으로, 함수 안에서 객체를 변경하면 함수 외부에서도 변경된다.

```javascript
const changeObj = (obj) => {
  obj.a = 9999;
}

const obj = {a: 1};
changeObj(obj);
console.log(obj);
/// 9999
```

## 마치며
JavaScript에서 변수와 상수, 그래고 객체가 어떤 타입을 가지는지는 헷갈리기 쉬운 이슈다. 코드가 복잡해질수록 이런 타입을 정확하게 인지하고 프로그래밍 하지 않으면, 버그가 발생할 확률이 높아진다. 참조 타입과 원시 타입을 잘 인지하고 적절하게 사용하는 것이 필요한 이유다.