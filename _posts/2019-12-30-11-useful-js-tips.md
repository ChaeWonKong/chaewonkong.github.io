---
layout: post
title: "JavaScript - [번역]11가지 극도로 유용한 JavaScript 팁"
date: "2019-12-29"
categories:
  - JavaScript
excerpt: |
  극도로 유용한 11가지 비법을 통해 JavaScript 게임에서 레벨 업 하기[번역]
feature_text: |
  ## JavaScript - [번역]11가지 극도로 유용한 JavaScript 팁
  JavaScript 게임에서 레벨 업 하기
feature_image: "https://miro.medium.com/max/5059/1*5YinhYlaDbUtS1Y33yV4yg.jpeg"
image: "https://miro.medium.com/max/5059/1*5YinhYlaDbUtS1Y33yV4yg.jpeg"
---


>  다음은 Medium  ["11 Extremely Useful JavaScript Tips"](https://medium.com/better-programming/11-extremely-useful-javascript-tips-4484429a5655) 을 번역한 글이다.


![Image by David Monjefrom Unsplash](https://miro.medium.com/max/5059/1*5YinhYlaDbUtS1Y33yV4yg.jpeg)
Image by [David Monje](https://unsplash.com/@davidmonje?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) from Unsplash




## 1. !! 연산자를 사용해 불리언 값으로 바꾸기

종종 우리는 어떤 변수가 존재하거나 유효한 값을 가지는지를 참/거짓 여부로 판별해야 할 때가 있다. `!!`(이중부정 연산자)를 이용해 이런 검증을 할 수 있다.



**간단히 `!!variable` 이라는 형태로 표현하는 것 만으로도 어떤 형태의 데이터도 불리언 값으로 변화시킬 수 있고, `0`, `null`, `""`, `undefined`, `NaN` 등의 경우엔 `false` 를, 이외의 경우엔 `true` 를 반환하게 할 수 있다.**



아래 예제를 보며 사례를 통해 이해해보자.



```javascript
function Account(cash) {
	this.cash = cash;
	this.hasMoney = !!cach;
}

var account = new Account(100.50);
console.log(account.cash); // 100.50
console.log(account.hasMoney); // true

var emptyAccount = new Account(0);
console.log(emptyAccount.cash); // 0
console.log(emptyAccount.hasMoney); // false
```



이 경우, `account.cash`가 0이 아니라면 `account.hasMoney`는 `true` 가  된다.



## 2. + 연산자를 이용해 Number 타입으로 바꾸기

이 방법은 마법같다! 매우 간단하지만 문자열 숫자에서만 사용 가능하고, 숫자로된 문자 또는 문자열이 아닐 경우엔 `NaN` 이 리턴된다. 예제를 살펴보자.



```javascript
function toNumber(strNumber) {
  return +strNumber;
}

console.log(toNumber("1234")); // 1234
console.log(toNumber("ACB")); // NaN

// 이 마법은 Date 클래스와도 작동하고, 이 경우엔 Number 타입의 타임스탬프를 반환한다.
console.log(+new Date()) // 1461288164385
```



## 3. 단락형 조건문

만약 다음과 같은 코드는

```javascript
if (connected) {
    login();
}
```



(검증하고자 하는) 변수와 함수 사이에  `&&`(AND 연산자)를 사용함으로써 짧게 축약할 수 있다.

예를 들어, 위의 코드는 아래처럼 한 줄로 줄여 쓸 수 있다.



```javascript
connected && login();
```



마찬가지로 어떤 요소(Attribute)나 함수가 특정 객체(Object) 내에 존재하는지도 확인할 수 있다.

아래 코드처럼 말이다.



```javascript
user && user.login();
```



## 4. || 연산자를 이용한 디폴트 값

오늘 날 ES6는 디폴트 전달인자(Default Argument)라는 기능을 제공한다. 이와 유사한 기능을 예전 버전의 브라우저에서 지원하기 위해 `||` (OR 연산자)를 이용해 디폴트 값을 두번째 매개변수로 제공할 수 있다.



만약 첫 번째 매개변수(전달인자, Parameter)가 `false` 값을 가진다면, 두 번째 매개변수가 디폴트 값으로 사용되게 된다.

예제를 살펴보자.



```javascript
function User(name, age) {
    this.name = name || "Oliver Queen";
    this.age = age || 27;
}

var user1 = new User();
console.log(user1.name); // Oliver Queen
console.log(user1.age); // 27

var user2 = new User("Barry Allen", 25);
console.log(user2.name); // Barry Allen
console.log(user2.age); // 25
```



## 5. `array.length` 를 루프 내에 캐싱하기

이 팁은 매우 단순함에도 큰 배열을 루프 문으로 처리할 때 엄청나게 큰 성능 차이를 가져온다. 기본적으로 배열을 순회할 때, 대부분은 아래와 같이 코드를 작성한다.



```javascript
for(var i = 0; i < array.length; i++) {
    console.log(array[i]);
}
```



만약 작은 배열을 처리한다면 상관 없지만, 큰 배열을 처리할 때는 이런 형태의 코드는 매 루프 시작시 마다 배열의 크기를 계산하여 지연을 발생시킨다.



이러한 경우에  `array.length` 를 변수에 할당해 캐싱하여, 매 루프 진입 시점마다  `array.length` 를 계산하는 것을 피할 수 있다.



```javascript
var length = array.length;
for(var i = 0; i < length; i++) {
    console.log(array[i]);
}
```



더 간단하게는 다음처럼 작성하면 된다.

```javascript
for(var i = 0, length = array.length; i < length; i++) {
    console.log(array[i]);
}
```



## 6. 배열에서 마지막 아이템에 접근하기

`Array.prototype.slice(begin, end)` 는 배열의 시작 아이템과 끝 아이템을 잘라오는 기능을 가지지만, 끝 아이템을 명시하지 않으면 이 매서드는 자동으로 가능한 가장 긴 형태의 배열을 반환한다.



내 생각에 대부분의 사람들은 이 매서드가 음수도 받을 수 있고, 음수를 통해 배열의 뒤쪽부터 접근할 수 있다는 사실을 모르는 것 같다.



```javascript
var array = [1,2,3,4,5,6];
console.log(array.slice(-1)); // [6]
console.log(array.slice(-2)); // [5,6]
console.log(array.slice(-3)); // [4,5,6]
```



## 7. 배열 크기 축소하기(Truncating)

이 테크닉은 배열의 사이즈를 제한하는 데, 배열의 크기를 기준으로 다른 배열의 엘리먼트들을 제거하고 싶은 경우에 매우 효과적이다.



예를 들어, 만약 10개의 엘리먼트가 있는 배열을 가지고 있지만, 5개의 엘리먼트만 남기고 싶을 때, `array.length = 5` 로 설정하여 그 배열의 크기를 작게 줄일 수 있다. 예제를 보자.



```javascript
var array = [1,2,3,4,5,6];
console.log(array.length); // 6
array.length = 3;
console.log(array.length); // 3
console.log(array); // [1,2,3]
```



## 8. 전부 치환하기

`String.replace()` 메서드는 문자열과 정규표현식을 활용해 문자열을 치환할 수 있게 한다. 기본적으로는 처음으로 발견되는 문자열이 치환된다. 하지만, `/g` 를 정규표현식 끝에 추가하면 `replaceAll()` 과 같은 효과를 만들어낼 수 있다.



```javascript
var string = "john john";
console.log(string.replace(/hn/, "ana")); // "joana john"
console.log(string.replace(/hn/g, "ana")); // "joana joana"
```



## 9. 노드리스트를 배열로 전환하기

만약  `document.querySelectorAll("p")` 를 실행한다면 DOM 엘리먼트의 배열을 `노드리스트(NodeList)` 라는 객체로 리턴할 것이다. 하지만, 이 객체에서는 `sort()`, `reduce()`, `map()`, `filter()` 등 일반적인 배열에서 사용 가능한 매서드들의 상당수가 사용 불가능하다.



이러한 배열의 여러 메서드들이 기능하게 하기 위해서는 `노드리스트` 를 `배열` 로 변환해야 한다. 단지 `[].slice.call(elements)` 를 실행하는 것이면 충분하다.



```javascript
var elements = document.querySelectorAll("p"); // 노드리스트
var arrayElements = [].slice.call(elements); // 이제 노드리스트는 배열로 변환됨

// 노드리스트를 배열인 변수로 변환하는 또 다른 방법
arrayElements = Array.from(elements);
```



## 10. 배열을 병합하기

배열을 병합할 때, `Array.concat()` 을 사용한다.



```javascript
var array1 = [1,2,3];
var array2 = [4,5,6];
console.log(array1.concat(array2)); // [1,2,3,4,5,6];
```



하지만 이 방법은 크기가 큰 배열을 병합할 경우, 새 배열을 생성하는 과정에서 많은 메모리 낭비가 발생하여 적합하지 않다.



이런 경우, `Array.push.apply(arr1, arr2)` 로 새 배열을 생성할 수 있다. 이 방법은 두 번째 배열을 첫 번째 배열로 병합하여 메모리 사용량을 절감한다.



```javascript
var array1 = [1,2,3];
var array2 = [4,5,6];
console.log(array1.push.apply(array1, array2)); // [1,2,3,4,5,6];
```



## 11. 배열의 엘리먼트 뒤섞기

<u>Lodash</u>(로다쉬) 같 외부 라이브러리에 의존하지 않고 배열의 엘리먼트를 뒤섞고 싶다면, 다음처럼 마술을 부려보자.



```javascript
var list = [1,2,3];
console.log(list.sort(function() { Math.random() - 0.5 })); // [2,1,3]
```



Resource : [JavaScript Tips](https://github.com/maketroli/js-hacks/blob/master/README.md)

Follow [Javascript Jeep🚙💨](https://medium.com/u/f9ffc26e7e69?source=post_page-----4484429a5655----------------------)

https://sitepoint.tapfiliate.com/p/payout-methods/new/



## 출처

- 영어 원문: ["11 Extremely Useful JavaScript Tips", by Javascript Jeep](https://medium.com/better-programming/11-extremely-useful-javascript-tips-4484429a5655)