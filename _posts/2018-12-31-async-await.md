---
layout: post
title: "ES 2017 async와 await 사용법"
date: "2018-12-31"
categories:
- JavaScript
excerpt: |
  JavaScript ES 2017의 새 기능 async와 await로 비동기적인 함수를 구현하고 promise를 다루는 방법에 대해 알아본다. ES6의 .then()과는 어떻게 다른지 알아본다. ES5와 비교하며 차이점 또한 살펴본다.
feature_text: |
  ## ES2017 async와 await 사용법
  JavaScript ES 2017의 새 기능 async와 await로 비동기적인 함수를 구현하고 promise를 다루는 방법에 대해 알아본다.
feature_image: "https://images.unsplash.com/photo-1514228742587-6b1558fcca3d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
image: "https://images.unsplash.com/photo-1514228742587-6b1558fcca3d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
---

## ES 2017의 새 기능, async와 await
기존에도 promise를 .then()을 이용해 충분히 다룰 수 있었지만, 직관적이지 않은 경우가 종종 있었다.

이제 async와 await을 이용한다면 **더욱 직관적으로 비동기적(asynchronous) 프로그래밍**을 할 수 있다. promise를 다루는 것도 훨씬 아름답고 간결해진다.

먼저 https://rallycoding.herokuapp.com/api/music_albums 로 가보자. JSON 형식으로 데이터가 제공되는 URL이다.

[링크를 클릭해 API 확인하러 가기](https://rallycoding.herokuapp.com/api/music_albums)
<br><br>

## 기존 코드
```javascript

const fetchAlbums = () => {
    fetch("https://rallycoding.herokuapp.com/api/music_albums")
        .then(res => res.json())
        .then(json => console.log(json));
};

fetchAlbums();
```

기존에는 fetch()를 사용하는 과정에서 .then()을 활용했다. fetch()를 이용해 URL에서 데이터를 가져오게 되면 promise를 건네 받는다. 데이터를 fetching하는 데에는 시간이 소요되고, 따라서 이를 비동기적으로 우선 promise를 건넨 후 fetching이 완료된 후 데이터를 사용하는 것이다.

여기서 주의할 점은 **fetch의 결과로 가져온 res는 JSON이 아니라는 것이다.** 여기서 JSON을 읽어오려면 .json()으로 가져와야 한다. 그리고 .json()은 **또 다른 promise**를 던져준다. 다시 한번 .then()을 사용해야 하는 이유다.

<img src="https://github.com/ChaeWonKong/chaewonkong.github.io/blob/master/assets/post_img/async-await.png?raw=true"
/>

그렇다면 이 문법을 async와 await을 활용해 개선하면 어떻게 될까?<br><br>

## async와 await 사용해 리팩토링 하기
```javascript
const fetchAlbums = async () => {
    const res = await fetch("https://rallycoding.herokuapp.com/api/music_albums");
    const json = await res.json();
    console.log(json);
}

fetchAlbums();
```

(1) 먼저 함수 앞에 async를 붙인다. 이로써 JavaScript는 비동기적으로 작동하는 함수라는 것을 인식한다.

(2) 모든 promise들을 찾고 이를 변수나 상수에 할당한다. fetch()와 json()이 여기서는 각각 promise를 반환한다.

(3) promise 앞에 await을 붙인다. 작업이 완료될 때 까지 기다린 후 저장하라는 의미라고 보면 된다.

이렇게해서 async와 await을 이용해 같은 기능을 구현할 수 있다. <br><br>

## async와 await을 이용할 때 장점
async와 await을 이용하면 누구든지 한 번에 이것이 비동기적으로 실행되는 함수임을 알 수 있다. 또, await을 보면 promise가 resolve 되면 상수에 담길 것을 이해할 수 있다.

즉, **직관적**이다. .then()을 사용하는 경우보다 직관적으로 무슨 일이 일어나는지를 확인할 수 있다. 그래서 유지보수 시에도 더욱 빛을 발한다.<br><br>

## ES5에서의 코드
```javascript
"use strict";

var fetchAlbums = function () {
  var _ref = _asyncToGenerator( /*#__PURE__*/regeneratorRuntime.mark(function _callee() {
    var res, json;
    return regeneratorRuntime.wrap(function _callee$(_context) {
      while (1) {
        switch (_context.prev = _context.next) {
          case 0:
            _context.next = 2;
            return fetch("https://rallycoding.herokuapp.com/api/music_albums");

          case 2:
            res = _context.sent;
            _context.next = 5;
            return res.json();

          case 5:
            json = _context.sent;

            console.log(json);

          case 7:
          case "end":
            return _context.stop();
        }
      }
    }, _callee, this);
  }));

  return function fetchAlbums() {
    return _ref.apply(this, arguments);
  };
}();

function _asyncToGenerator(fn) { return function () { var gen = fn.apply(this, arguments); return new Promise(function (resolve, reject) { function step(key, arg) { try { var info = gen[key](arg); var value = info.value; } catch (error) { reject(error); return; } if (info.done) { resolve(value); } else { return Promise.resolve(value).then(function (value) { step("next", value); }, function (err) { step("throw", err); }); } } return step("next"); }); }; }

fetchAlbums();
```

babeljs.io에서 Try it out에 방금 작성한 코드를 넣어보면 위 코드와 같다. 단순한 fetching을 위해서도 ES5로는 위처럼 복잡한 코드를 짜야만 했다. 

매해 더욱 간결하고, 직관적으로 발전해 나가는 JavaScript에게 감사하자.
