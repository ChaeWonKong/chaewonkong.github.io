---
layout: post
title: "React - TypeScript로 React에서 Kakao Maps 사용하기"
date: "2019-12-23"
categories:
  - React
excerpt: |
  React에서 TypeScript로 카카오 지도 API(Kakao Maps API)를 사용하는 방법을 알아본다.
feature_text: |
  ## React - TypeScript로 React에서 Kakao Maps 사용하기
  React에서 TypeScript로 카카오 지도 API(Kakao Maps API)를 사용하는 방법을 알아본다.
feature_image: "https://images.unsplash.com/photo-1548345680-f5475ea5df84?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1652&q=80"
image: "https://images.unsplash.com/photo-1548345680-f5475ea5df84?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1652&q=80"
---

React에서 지도를 사용하는 것은 조금 불편하다. 



카카오맵이나 네이버 지도 API는 iOS/Android/JavaScript 이렇게 세 분류로 나눠 제공되는데, **Web을 의미하는 JavaScript의 경우 DOM을 직접 접근하여 지도를 그리는 방식을 채택하고 있어서다.**



예를들어,

```html
<html>
<head>
	<title>Kakao 지도 시작하기</title>
</head>
<body>
	<div id="map" style="width:500px;height:400px;"></div>
	<script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=발급받은 APP KEY를 넣으시면 됩니다."></script>
	<script>
		var container = document.getElementById('map');
		var options = {
			center: new kakao.maps.LatLng(33.450701, 126.570667),
			level: 3
		};

		var map = new kakao.maps.Map(container, options);
	</script>
</body>
</html>
```

이렇게 div 엘리먼트를 가져와 지도를 렌더링하는 방식으로 동작한다.



따라서, React에서 카카오맵스 API를 이용하려면 라이프사이클을 응용하는 등 약간의 커스터마이징 작업이 필요하다.



오늘은 TypeScript 기반으로 CRA(Create React App)을 통해 생성된 React 어플리케이션에서 카카오맵스 API를 사용하는 방법을 알아본다.



## 준비

먼저 [KakaoDevelopers_](https://developers.kakao.com/)에서 회원가입을 하고 어플리케이션 등록을 하자.

지도 API를 사용하기 위해서는 당연히 **카카오로부터 앱 등록을 하고 키 값을 발급받아야** 한다.



<img src="https://github.com/ChaeWonKong/image-resource/blob/master/kakao-maps/1.png?raw=true" width="100%"/>

사이트에 들어간 후, 로그인을 하고 위 사진에서처럼 "내 애플리케이션"을 눌러 관리 화면으로 이동한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/kakao-maps/2.png?raw=true" width="100%"/>

다음으로 "앱 만들기"를 클릭해 앱 만들기 화면으로 이동한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/kakao-maps/3.png?raw=true" width="100%"/>

앱 이름과 회사명 등을 추가하고 "앱 만들기"를 클릭한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/kakao-maps/4.png?raw=true" width="100%"/>

앱을 생성하였다면, 좌측 메뉴 탭에서 "설정" -> "일반" 으로 이동한 후 중간 부분의 "플랫폼 추가"를 클릭하여 "웹"을 추가해준다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/kakao-maps/5.png?raw=true" width="100%"/>





웹을 추가한 후 웹을 선택하고 위 사진처럼 사이트 도메인을 추가해준다.

여기서 **사이트 도메인을 제대로 추가해주지 않으면 CORS 에러가 발생할 수 있으니, 개발환경과 배포환경의 도메인을 모두 추가해 준다.**



우리는 CRA로 앱을 생성할 것이므로 우선 "http://localhost:3000"을 추가해 준다.



## 프로젝트 생성

다음으로 프로젝트를 생성한다.



```shell
$ yarn create react-app map-app --template typescript
```



본인이 원하는 형태로 앱을 생성해준다.

본 포스팅에서는 타입스크립트를 사용하도록 설정해 생성한다.



## 카카오맵스 연동

먼저 index.html에 카카오맵스 소스를 링크해야 한다.



```public/index.html``` 에 다음을 추가한다.

```html
<body>
  <noscript>You need to enable JavaScript to run this app.</noscript>
  <div id="root"></div>
  <script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=[발급받은 javascript 키]"></script>

</body>
```

Body 태그의 가장 밑에 위처럼 

```html
<script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=[발급받은 javascript 키]"></script>
```

이 스크립트 태그를 추가한다.



여기서 주의할 점이 있다.

**발급받은 키는 다른 사람에게 공개될 경우, 타인이 악의적으로 우리의 쿼터로 카카오 지도를 사용할 가능성이 있으므로 숨기는 것이 좋다.**

우리는 CRA가 제공하는 **"Custom Environment Variable"을 이용해 추가**해 줄 것이다.



먼저 root에 ```.env``` 라는 파일을 생성한다.

그리고 그 파일에 다음처럼 추가한다.

```env
REACT_APP_KAKAO_MAP_KEY=[JavaScript 키]
```



그리고 ```.gitignore``` 에 ".env"를 추가해 커밋 시 ignore될 수 있도록 한다.



다음으로 좀 전에 ```public/index.html```  에 추가했던 스크립트 태그의 내용을 아래처럼 수정한다.

```html
<script type="text/javascript" src="//dapi.kakao.com/v2/maps/sdk.js?appkey=%REACT_APP_KAKAO_MAP_KEY%"></script>
```

위 코드는 이전 코드에 비해

`appkey=%REACT_APP_KAKAO_MAP_KEY%`


이 부분이 변경되었다.



**혹시나 실수로 key가 유출되었거나 커밋 후 깃허브에 푸시된 경우 KakaoDevelopers의 본인 계정에서 키를 새로 발급 받을 수 있다.**



## 지도 생성

다음으로 ```App.tsx``` 에서 지도를 생성해 보자.



우리는 TypeScript 기반의 Functional Component로 지도를 생성할 것이다.



여기서는 가장 기본적인 예제인 지도 생성만 다룰 예정이니,

[Kakao Maps API Guide](http://apis.map.kakao.com/web/guide/) 를 참고하자.



`src/App.tsx`

{% raw %}
```tsx
import React, { useEffect } from 'react';



const App: React.FC = () => {
  return (
    <div className="App">
      <div id="map" style={{ width: "100vw", height: "100vh" }} />
    </div>
  );
}

export default App;
```
{% endraw %}


먼저 위처럼 div 태그에 'map'이라는 id를 부여해준다.



**여기서 중요한 것은 반드시 width와 height가 명시적으로 제공되어야 지도가 렌더링 된다는 점이다.**

width나 height 중 어느 하나라도 누락되거나 "%"등의 상대 값으로 제공되면 지도는 렌더링되지 않는다.



다음으로, ```id="map"``` 을 동적으로 가져와 지도를 생성하는 코드를 추가하자.

여기서 라이프사이클을 이용해야 한다.



만약 클래스형 컴포넌트를 사용 중이라면 ```componentDidMount``` 를 이용하면 되고, 우리는 ```useEffect``` 를 이용할 것이다.

```tsx
  useEffect(() => {
    let container = document.getElementById('map');
    let options = {
      center: new window.kakao.maps.LatLng(33.450701, 126.570667),
      level: 3
    };

    let map = new window.kakao.maps.Map(container, options);

  }, [])
```



여기까지 잘 따라 왔다면 에러가 뜰 것이다. ```kakao``` 가 존재하지 않는다고 말이다.

정상이다.



```public/index.html```에 링크된 스크립트 덕분에 kakao는 존재한다. 다만 우리 앱은 그 사실을 알지 못한다.
따라서 귀찮더라도 kakao가 존재한다는 것을 앱에 알려줘야 한다.



먼저 ```kakao``` 를 글로벌로 선언해 줘야 한다.

```tsx
declare global {
  interface Window {
    kakao: any;
  }
}
```



이제 앱을 실행해 보자.

```shell
$ yarn start
```

아마 지도를 확인할 수 있을 것이다.

{% raw %}

```src/App.tsx``` 의 전체 코드는 아래와 같다.

```tsx
import React, { useEffect } from 'react';

declare global {
  interface Window {
    kakao: any;
  }
}

const App: React.FC = () => {
  useEffect(() => {

    let container = document.getElementById('map'); //지도를 담을 영역의 DOM 레퍼런스
    let options = { //지도를 생성할 때 필요한 기본 옵션
      center: new window.kakao.maps.LatLng(33.450701, 126.570667), //지도의 중심좌표.
      level: 3 //지도의 레벨(확대, 축소 정도)
    };

    let map = new window.kakao.maps.Map(container, options); //지도 생성 및 객체 리턴

  }, [])

  return (
    <div className="App">
      <div id="map" style={{ width: "100vw", height: "100vh" }} />
    </div>
  );
}

export default App;
```
{% endraw %}
