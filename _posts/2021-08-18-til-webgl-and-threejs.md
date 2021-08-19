---
layout: post
title: "[TIL] WebGL과 three.js 가볍게 알아보기"
date: "2021-08-18"
categories:
  - TIL
excerpt: |
  인터렉티브한 그래픽이 제공되는 웹 사이트는 어떻게 만들까? 궁금증을 해소하기 위해 WebGL과 WebGL을 사용하는 JavaScript 라이브러리 three.js를 가볍게 알아본다.
feature_text: |
  ## 애플의 웹사이트처럼 인터렉티브하고 영상처럼 화려한 웹페이지는 어떻게 만들어질까?
  인터렉티브한 그래픽이 제공되는 웹 사이트는 어떻게 만들까? 궁금증을 해소하기 위해 WebGL과 WebGL을 사용하는 JavaScript 라이브러리 three.js를 가볍게 알아본다.
feature_image: "https://images.unsplash.com/photo-1524351543168-8e38787614e9?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=967&q=80"
image: "https://images.unsplash.com/photo-1524351543168-8e38787614e9?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=967&q=80"
---

우연히 고도로 인터렉티브한 그래픽이 제공되는 웹사이트는 어떻게 개발되는 것인지 궁금해졌다. 예를들면 [Apple의 아이폰 12 소개 페이지](https://www.apple.com/kr/iphone-12/) 처럼 말이다. 스크롤을 할 때마다, 마치 영화를 보듯 우리는 인터렉션에 감탄하게 된다.

그래서, 이런 인터렉티브하고 영상과 웹이 구분되지 않는 페이지를 만드는 방법을 알아보다가 WebGL과 three.js까지 다다르게 된 것이다.

<img src='https://github.com/ChaeWonKong/image-resource/blob/master/threejs.png?raw=true' width='100%'/>

애플 웹사이트에도 three.js가 사용된 것을 알 수 있다.

## WebGL

WebGL은 브라우저에서 인터렉티브한 3D 그래픽을 사용할 수 있도록 돕는 웹 기반 그래픽 라이브러리다. **별도의 플러그인을 사용하지 않고, 2D, 3D 그래픽을 표현할 수 있도록 돕는 JavaScript API다.** CPU만으로 그래픽을 처리하는 것에는 무리가 있으므로 브라우저에서 사용자의 장치의 그래픽카드를 이용해 표현을 진행하게 된다.

**Unity 기반의 웹 게임, 고도로 인터렉티브 한 웹 사이트 등을 제작하는데 사용**된다. 내부적으로 OpenGL을 이용한다.

## three.js

**three.js는 JavaScript 3D 라이브러리다. 쉽고, 가볍고, 브라우저 호환성이 좋은 3D 라이브러리를 지향**한다. WebGL을 이용한다.

three.js를 이용하면 인터렉티브 한 그래픽이나 VR 콘텐트도 웹에서 제공할 수 있다.

### 설치

npm을 이용해 설치하여 사용할 수도 있고,

```shell
npm i three
```

CDN에서 가져와 쓸 수도 있다.

```html
<script type="module">
  // Find the latest version by visiting https://cdn.skypack.dev/three.
  import * as THREE from "https://cdn.skypack.dev/three@<version>";
  const scene = new THREE.Scene();
</script>
```

### Compatibility

아쉽게도 **Node.js에서는 사용하기 어렵다.** three.js는 DOM API를 기반으로 제작된 웹용 라이브러리기 때문이다.

참고로, WebGL은 사용자 device에 따라 compatibility 이슈가 있을 수 있다. 따라서 코드 단 내에서 이를 체크해 주는 것이 보다 안전한 개발이 될 수 있다.

```javascript
if (WEBGL.isWebGLAvailable()) {
  // Initiate function or other initializations here
  animate();
} else {
  const warning = WEBGL.getWebGLErrorMessage();
  document.getElementById("container").appendChild(warning);
}
```

## Reference

- [[GitHub] three.js](https://github.com/mrdoob/three.js/)
- [[three.js] documentations](https://threejs.org/docs/index.html#manual/en/introduction/Creating-a-scene)
- [[Youtube] WebGL 3D Graphic in 100 seconds](https://www.youtube.com/watch?v=f-9LEoYYvE4)
