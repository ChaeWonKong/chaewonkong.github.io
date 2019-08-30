---
layout: post
title: "트랜스컴파일러(바벨)의 목적과 사용법"
date: "2018-12-06"
categories:
- JavaScript
excerpt: |
  대표적 트랜스컴파일러 바벨(Babel)과 빌드도구 걸프(Gulp)를 이용해 ES6를 ES5로 트랜스컴파일 하기
feature_text: |
  ## 트랜스컴파일러의 목적과 사용법
  대표적 트랜스컴파일러 바벨(Babel)과 빌드도구 걸프(Gulp)를 이용해 ES6를 ES5로 트랜스컴파일 하기
feature_image: "https://images.unsplash.com/photo-1482062364825-616fd23b8fc1?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
image: "https://images.unsplash.com/photo-1482062364825-616fd23b8fc1?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
---

## 트랜스컴파일의 필요성
이제는 나름 활발하게 사용 중인 ES6. 그러나 아직 ES6는 어디에나 사용할 수 있는 '안전한' 코드는 아니다. 사용자의 환경은 다양하다. 모든 사용자가 최신 브라우저를 사용하는 것은 아니며, 최신 브라우저라고 다 모든 ES6 기능을 지원하는 것도 아니다. 따라서 브라우저 환경에서 안전하게 JavaScript를 사용하기 위해서는 Babel과 같은 트랜스컴파일러를 이용해 ES5로 변환해 배포해야 한다.

## 실습 구성
오늘은 Babel과 Gulp를 이용해 ES6로 짜여진 코드를 ES5로 트랜스컴파일 한다.
Babel은 트랜스컴파일러이며, Gulp는 빌드를 자동화해주는 도구이다. 

실제로 ES6를 ES5로 변환해주는 것은 바벨이다. 하지만, 변환을 지시하고 작동시키는 것은 Gulp를 통해서 한다. project/es6 디렉토리 내에 있는 test.js ES6로 짜여진 파일을 Babel과 Gulp를 이용해 ES5로 변환한 후 project/dist 디렉토리에 같은 이름으로 저장할 것이다.

## 실습 환경 갖추기
먼저 디렉토리를 생성한다.

```
$ mkdir project/
$ cd project/
$ mkdir es6/
$ mkdir dist/
```

그리고 npm을 이용해 환경을 생성한다.
```
$ npm init
```

우선 글로벌 환경에서 Gulp를 사용할 수 있도록 설치한다.
```
$ sudo npm install -g gulp
```
설치가 완료되면 gulp라는 명령어를 통해 gulpfile을 실행할 수 있다.

이제 Gulp와 gulp-babel을 설치한다. gulp-babel은 걸프를 이용해 Babel을 사용할 수 있게 해주는 패키지이다.
```
$ npm install --save-dev gulp gulp-babel
```

마지막으로 Babel을 설치한다.
```
$ npm install --save-dev @babel/preset-env @babel/core
```
@babel/preset-env는 최신 JavaScript를 변환해줄 수 있는 패키지이다. @babel/core는 gulp-babel의 peer dependency이다.

Babel을 설치한 후에는 어떤 프리셋을 사용할 것인지 명시해줘야 한다. project 디렉토리 내에서 다음 명령을 실행해 .babelrc 파일을 생성한다.
```
$ echo '{"presets": ["@babel/preset-env"]}' > .babelrc
```
## ES6 코드 작성
이제 ES6 코드를 작성해보자. project/es6 디렉토리에 test.js를 생성하고 다음의 코드를 입력한다.

```javascript
"use strict";

const sentences = [
  { subject: "JavaScript", verb: "is", object: "great" },
  { subject: "Elephants", verb: "are", object: "large" }
];

const say = ({ subject, verb, object }) => {
  console.log(`${subject}${verb}${object}`);
};

// sentence 배열 내에 있는 각 객체별로 subject, verb, object를 연결해 출력
for (let s of sentences) {
  say(s);
}
```

node를 이용해 코드를 실행해보자. project/es6 디렉토리 내에서 다음을 실행한다.
```
$ node test/js
```

출력결과:
```
$ JavaScriptisgreat
$ Elephantsarelarge
```

## Gulp 코드 작성
이제 본격적으로 Gulp와 Babel을 이용해 ES5로 test.js 파일을 자동 변환해주는 스크립트를 작성해보도록 한다.

project 디렉토리 내에 gulpfile.js 파일을 생성한다.
```javascript
// project/gulpfile.js
const gulp = require("gulp");
const babel = require("gulp-babel");

gulp.task("default", (done) => {
  gulp
    .src("es6/**/*.js")
    .pipe(babel())
    .pipe(gulp.dest("dist"));
  done();
});
```

\*\*는 서브디렉토리를 포함해 모든 디렉토리를 뜻하는 와일드카드이다. 
"es6/\*\*/*.js"는 es6 내에 있는 모든 js 확장자의 파일들을 불러와 작업하겠다는 뜻이다.
걸프는 이 모든 파일들을 불러와 babel() 함수에 파이프로 연결한다. 그러면 Babel이 작동해 es5로 변환된다. 이 결과물은 다시 gulp의 pipe를 통해 목적지인 dist 디렉토리로 보내진다. 

변환 후, 같은 디렉토리 구조를 유지하며 같은 이름의 파일로 결과물은 저장된다. 예를들어,
es6/b/test.js를 변환해 es5 디렉토리로 옮기는 스크립트를 작성하고 실행하게 되면, es5/b/test.js 형태로 결과물이 저장되게 된다.

한편, Gulp3와 달리 Gulp4에서는 콜백함수를 이용해 작업(task)가 끝났음을 명시해줘야만 에러가 발생하지 않는다. 비동기적 처리 문제 때문이다. 따라서 done()을 추가해 작업의 완료를 알려야 한다.

## Babel과 Gulp를 이용해 트랜스컴파일하기
자 이제, gulp를 호출해 자동화 스크립트를 실행하고, es6/test.js를 ES5로 트랜스컴파일 하자. 결과물은 dist/ 디렉토리에 test.js로 저장될 것이다. project 디렉토리에서 다음 명령을 실행한다.

```
$ gulp
```

자, 이제 project/dist/ 디렉토리를 살펴보자. test.js 파일이 생성되었다. 파일의 내용은 다음과 같을 것이다.

```javascript
"use strict";

var sentences = [{
  subject: "JavaScript",
  verb: "is",
  object: "great"
}, {
  subject: "Elephants",
  verb: "are",
  object: "large"
}];

var say = function say(_ref) {
  var subject = _ref.subject,
      verb = _ref.verb,
      object = _ref.object;
  console.log("".concat(subject).concat(verb).concat(object));
};

for (var _i = 0; _i < sentences.length; _i++) {
  var s = sentences[_i];
  say(s);
}
```
ES6의 문법이 ES5 문법으로 바뀐 것을 알 수 있다.
project/dist/test.js 파일을 실행해 같은 결과가 출력되는지 확인해보자.
project 디렉토리 내에서 다음을 실행한다.

```
$ node dist/test.js
```

출력결과:
```
$ JavaScriptisgreat
$ Elephantsarelarge
```

## 마치며
Babel과 같은 트랜스컴파일러를 사용하면 ES6로 편리하게 코드를 짜고도 범용성을 확보할 수 있다. ES6는 매우 편리하다. 비구조화 할당, 화살표함수, const와 let을 이용한 변수 선언 등은 JavaScript를 더욱 편리하고 안전하게 만들어준다. 무엇보다 ES6가 점차 보편화되는 추세이므로 결국 어느 시점에서는 ES5 대신 ES6만을 써야할 것이다.

Babel을 사용하면 ES6로만 코드를 작성하면 된다. ES5로의 변환은 Babel이 책임진다. 이로써 개발자는 ES6의 장점을 취하고 단점이었던 호환 문제를 극복할 수 있게 된다.