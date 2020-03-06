---
layout: post
title: "JavaScript 웹 크롤링 하기"
date: "2019-06-11"
categories:
  - NodeJS
excerpt: |
  Node.js와 axios, cheerio를 활용해 웹 크롤러를 만들어 본다.
feature_text: |
  ## JavaScript 웹 크롤링 하기
  Node.js와 axios, cheerio를 활용해 웹 크롤러를 만들어 본다.
feature_image: "https://images.unsplash.com/photo-1556157382-4e063bb26661?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80"
image: "https://images.unsplash.com/photo-1556157382-4e063bb26661?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80"
---

## 목차

1. 사전 준비
2. 크롤러 구현
3. 참고 자료

## 1. 사전 준비

1.1. npm 설치 및 프로젝트 시작하기
먼저 npm이 무엇인지 알아보자. npm은 node package manager를 의미하며 Java의 gradle 혹은 Python의 pip과 유사한 패키지 관리자이다.

[https://nodejs.org/en/download/](https://nodejs.org/en/download/) 에서 운영체제별 인스톨러를 설치할 수 있다.

다음으로 package.json 파일을 생성해 보자.

```cmd
npm init
```

묻는 내용에 일단은 계속 엔터를 치고 넘어가면 npm에 의해 개발 환경이 생성된다. 이후 npm을 통해 설치하는 모든 라이브러리들은 node_modules라는 디렉토리에 저장되며, package.json을 통해 관리할 수 있다.

### 1.2. 관련 라이브러리 설치

먼저 크롤러를 만들기 위해 필요한 라이브러리들을 정리해 보자.

우리는 axios와 cheerio를 사용해 크롤링을 할 것이다.

axios는 자바스크립트를 이용해 AJAX 리퀘스트를 할 수 있는 라이브러리이다. AJAX란 Asynchronous JavaScript and XML을 의미하는 것으로 비동기적으로 JSON이나 XML 등의 데이터를 받을 수 있는 방법을 의미한다.

cheerio는 JQuery처럼 DOM Selector 기능을 제공한다. axios로 받은 HTML 데이터에서 실제로 필요한 정보를 발라내는 데 사용할 것이다. 참고로, cheerio는 Python의 BeautifulSoup과 유사한 기능을 한다.

```cmd
npm i axios cheerio
```

## 2. 크롤러 구현

### 2.1. axios와 cheerio import

이제 본격적으로 크롤러를 구현해보자.

```javascript
const axios = require("axios");
const cheerio = require("cheerio");
```

먼저 axios와 cheerio를 import한다. Node는 require 함수의 인자를 node_modules에서 부터 디렉토리 명으로 검색하므로 구체적인 경로를 적지 않아도 설치한 라이브러리는 간단하게 import 할 수 있다.

### 2.2. 크롤링 대상 사이트 분석

다음으로 크롤링 하고자 하는 사이트를 살펴보자.

윤리적인 이유에서 본인의 블로그를 대상으로 한 크롤러를 짜 보고자 한다. chaewonkong.github.io에서 포스트 제목들을 가져오도록 해 보자.

```
body > main > div > section > ul > li > article > h2 > a
```

우리가 가져오고자 하는 포스트 제목들은 위와 같은 DOM 구조를 이루고 있다.

### 2.3. 코드 작성

이제 대충 구조를 알았으니 코드를 작성해 보자.

**scraper.js**

```javascript
const axios = require("axios");
const cheerio = require("cheerio");

// axios를 활용해 AJAX로 HTML 문서를 가져오는 함수 구현
async function getHTML() {
  try {
    return await axios.get("https://chaewonkong.github.io");
  } catch (error) {
    console.error(error);
  }
}

// getHTML 함수 실행 후 데이터에서
// body > main > div > section > ul > li > article > h2 > a
// 에 속하는 제목을 titleList에 저장
getHTML()
  .then(html => {
    let titleList = [];
    const $ = cheerio.load(html.data);
    // ul.list--posts를 찾고 그 children 노드를 bodyList에 저장
    const bodyList = $("ul.list--posts").children("li.item--post");

    // bodyList를 순회하며 titleList에 h2 > a의 내용을 저장
    bodyList.each(function(i, elem) {
      titleList[i] = {
        title: $(this)
          .find("h2 a")
          .text()
      };
    });
    return titleList;
  })
  .then(res => console.log(res)); // 저장된 결과를 출력
```

### 2.4. 실행

자 이제 코드를 실행해 보자.

```cmd
node scraper.js
```

아래는 위 파일을 실행했을 경우의 결과다.

```javascript
[
  { title: "Simple Chat App - React with Socket.IO" },
  { title: "Sort method in JavaScript" },
  { title: "Object Oriented Programming 1 - Basic Concepts" },
  { title: "웹과 HTTP" },
  { title: "DOM과 Object Model" },
  { title: "Node.js의 특징" },
  { title: "사이트 간 스크립팅" },
  { title: "파이썬(Python) 데코레이터(Decorator)" },
  { title: "카운팅 정렬(계수정렬, Counting Sort) - 백준 알고리즘 10989번" },
  { title: "그래프를 활용한 알고리즘 01 - PICNIC" },
  { title: "자바스크립트에서 클로저 사용하는 법" },
  { title: "분할정복을 이용한 자바스크립트 병합정렬 알고리즘" },
  {
    title:
      "반응형(Responsive) 웹과 적응형(Adaptive) 웹, 그리고 모바일 퍼스트 디자인"
  },
  { title: "ES 2017 async와 await 사용법" },
  { title: "자바스크립트 알고리즘 - n개의 원소 중 m개를 뽑는 모든 조합" },
  { title: "리액트(React)란 무엇인가" },
  { title: "Express 서버와 React: Proxy 활용과 빌드 및 헤로쿠(Heroku) 배포" },
  { title: "N 팩토리얼에서 1의자리부터 연속하는 0의 개수 구하기" },
  { title: "자바스크립트 이진탐색 알고리즘" },
  { title: "타입스크립트(TypeScript)의 타입들" },
  { title: "자바스크립트의 배열(Array in JavaScript)" },
  { title: "자바스크립트 함수 내 객체의 변경과 할당" },
  { title: "쿠키(Cookie) 기반의 사용자 인증(Auth)" },
  { title: "파이썬 이진탐색 알고리즘" },
  { title: "JavaScript 참조 타입과 원시 타입" },
  { title: "트랜스컴파일러(바벨)의 목적과 사용법" },
  { title: "React & Redux 1" },
  { title: "파이썬으로 웹 크롤러 만들기 2" },
  { title: "파이썬으로 웹 크롤러 만들기 1" }
];
```

이처럼 간단하게 Node.js와 axios, cheerio를 이용해 크롤러를 구현해 보았다.

Node.js에서도 excel 파일로 크롤링한 정보를 파싱(Parsing)하는 방법이 존재하고, 그런 라이브러리가 있다.

이 부분은 다음에 다뤄보기로 하고, 오늘은 이만 마치고자 한다.

## 3. 참고 자료

- **Node.js에서 웹 크롤링 하기** ([https://velog.io/@yesdoing/Node.js-%EC%97%90%EC%84%9C-%EC%9B%B9-%ED%81%AC%EB%A1%A4%EB%A7%81%ED%95%98%EA%B8%B0-wtjugync1m](https://velog.io/@yesdoing/Node.js-%EC%97%90%EC%84%9C-%EC%9B%B9-%ED%81%AC%EB%A1%A4%EB%A7%81%ED%95%98%EA%B8%B0-wtjugync1m))
- **cheerio** ([https://github.com/cheeriojs/cheerio](https://github.com/cheeriojs/cheerio))
