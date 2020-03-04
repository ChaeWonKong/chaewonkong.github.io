---
layout: post
title: "JavaScript로 Deep Copy 하는 여러 방법"
date: "2020-03-04"
categories:
  - JavaScript
excerpt: |
  JSON 객체의 메소드를 이용하거나 재귀적 함수, Lodash를 이용해 JavaScript에서 Deep Copy(깊은 복사)를 하는 방법을 알아본다
feature_text: |
  ## JavaScript로 Deep Copy 하는 여러 방법
  JSON 객체의 메소드를 이용하거나 재귀적 함수, Lodash를 이용해 JavaScript에서 Deep Copy(깊은 복사)를 하는 방법을 알아본다
feature_image: "https://images.unsplash.com/photo-1516541793220-5a9ea492ef70?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=3150&q=80"
image: "https://images.unsplash.com/photo-1516541793220-5a9ea492ef70?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=3150&q=80"
---

오늘은 JavaScript에서 깊은 복사(Deep Copy)를 진행하는 여러 방법들에 대해 살펴보려고 합니다.
웹 개발을 하면서 사실 깊은 복사를 사용하는 경우는 많지 않았던 것 같아요.
그래서 이렇게 정리해 보기 전까지는 생각보다 다양한 방법들이 있고, `object.assign()`이나 스프레드 문법(Spread Syntax) 처럼 얕은 복사(Shallow Copy)만 지원하는 매서드나 문법을 제대로 이해하지 못했다는 것을 알게 되었습니다.

자 그럼, 시작해 보겠습니다.

## 1. JSON 객체의 메소드 이용

Dates, functions, undefined, Infinity, RegExps, Maps, Sets, Blobs, FileLists, ImageDatas, sparse Arrays, Typed Arrays or other complex types within your object

먼저 가장 간단하고 쉽게 구현하는 방법을 살펴보겠습니다.
`JSON.stringify`와 `JSON.parse`를 번갈아 사용하면서 깊은 복사(Deep Copy)를 실현하는 방법입니다.

**JSON.stringify**: JS 오브젝트를 스트링 포멧으로 변환

**JSON.parse:** 스트링 포멧을 JS 오브젝트로 변환.

그럼 어떻게 하냐고요?
코드를 살펴보겠습니다.

```javascript
const obj1 = {
  a: 1,
  b: "string",
  c: {
    name: "Leon",
    age: "29"
  }
};

// Deep Copy 방법
const obj2 = JSON.parse(JSON.stringify(obj1));
```

핵심은 이렇습니다. 문자열(String)으로 변환되었다가 다시 객체(Object)로 변환되기 때문에 이전 객체에 대한 참조가 없어지는 원리를 이용하죠.
다만 한계는 뚜렷합니다.
먼저, **깊은 복사가 불가능한 타입들이 꽤 많습니다.** 함수(functions), Date 객체, 정규표현식, Infinity 등등의 데이터는 복사되지 않고 유실되고 맙니다.

또, **속도가 느리다는 단점도 있습니다.**
벤치마크를 참조해 보겠습니다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/deepcopy.png?raw=true" width="80%">

> 테스트 직접 해보기: [http://jsben.ch/2KRm3](http://jsben.ch/2KRm3)

JSON 메소드 자체가 성능 면에서 다른 방법에 비해 느리기 때문에 주의해야 함을 알 수 있습니다.

이후 살펴볼 재귀 함수를 직접 작성하는 방법이나 Lodash의 `cloneDeep()`이 더 나은 선택지라는 것을 확인할 수 있죠.

## 2. 커스텀 재귀 함수 사용

가장 바닥부터 해결하는 방법으로는 직접 깊은 복사(Deep Copy)를 실현하는 함수를 짜는 것이겠지요.

**복사를 진행하다가 객체를 만나면 함수를 재귀적으로 실행해 깊은 복사(Deep Copy)를 실현**하는 함수를 짜보겠습니다.

```javascript
function cloneObject(obj) {
  var clone = {};
  for (var key in obj) {
    if (typeof obj[key] == "object" && obj[key] != null) {
      clone[key] = cloneObject(obj[key]);
    } else {
      clone[key] = obj[key];
    }
  }

  return clone;
}
```

오브젝트의 프로퍼티를 순환하며 빈 오브젝트에 더하고, 프로퍼티가 오브젝트일 경우 재귀적으로 함수를 실행하며 복사를 진행합니다.

## 4. lodash의 cloneDeep() 사용

마자막으로 가장 손쉽게 Lodash를 사용해 해결하는 방법을 살펴보겠습니다.
보통 웹 개발을 하다 보면 Lodash는 흔히 사용하게 됩니다.
이미 **코드 내에서 기존에 Lodash를 사용하고 있었다면 Lodash를 활용해 깊은 복사(Deep Copy)를 진행하는 것도 좋은 선택지일 것**입니다.

```javascript
const original = { a: { b: 2 } };
let copy = _.cloneDeep(original);
copy.a.b = 100;
console.log(original.a.b); //2
```

## 마치며

얕은 복사와 깊은 복사는 프로그래머라면 당연히 구분하고 설명하고 구현할 수 있으리라 기대되는 개념인 것 같습니다.
이 외에도 jQuery를 이용하는 방법이나 immutable.js를 이용하는 방법 등 다양한 방법이 있다고 합니다.
JavaScript의 세계도 넓고 알아야 할 것도 참 많은 것 같습니다.

## 참고자료

- [What is the best and most efficient way to deep clone an object in JavaScript?](https://frontbackend.com/javascript/what-is-the-the-fastest-way-to-deep-clone-an-object-in-javascript)
- [ddalpange, "자바스크립트 객체 복사하기"](https://velog.io/@ddalpange/%EC%9E%90%EB%B0%94%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8-%EA%B0%9D%EC%B2%B4-%EB%B3%B5%EC%82%AC%ED%95%98%EA%B8%B0)
- [마이구미, "Javascript:Shallow and Deep Copy :: 마이구미"](https://mygumi.tistory.com/322)
- [Stackoverflow, "What is the most efficient way to deep clone an object in JavaScript?"](https://stackoverflow.com/questions/122102/what-is-the-most-efficient-way-to-deep-clone-an-object-in-javascript)
- [hyunseob, "JavaScript에서 객체 복사하기"](https://hyunseob.github.io/2016/02/08/copy-object-in-javascript/)
- [hochulshin, "Javascript - deep copy가 필요할 때 무얼 사용해야 할까?"](http://hochulshin.com/javascript-best-deepcopy/)
- [한영재, "JAVASCRIPT Shallow Copy(얕은 복제)와 Deep Copy(깊은 복제)의 차이!"](https://medium.com/@han7096/shallow-copy-%EC%96%95%EC%9D%80-%EB%B3%B5%EC%A0%9C-%EC%99%80-deep-copy-%EA%B9%8A%EC%9D%80-%EB%B3%B5%EC%A0%9C-%EC%9D%98-%EC%B0%A8%EC%9D%B4-9d11458b4da1)
