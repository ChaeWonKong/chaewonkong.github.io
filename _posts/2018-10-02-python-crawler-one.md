---
layout: post
title: "파이썬으로 웹 크롤러 만들기 1"
date: "2018-10-02"
categories:
- DevLog
excerpt: |
  파이썬과 Requests 라이브러리를 이용해 간단한 크롤러를 만들어본다.
feature_text: |
  ## 파이썬으로 웹 크롤러 만들기 1
  파이썬 웹 크롤러를 이용해 홈페이지의 정보 긁어오기
feature_image: "https://picsum.photos/2560/600?image=733"
image: "https://picsum.photos/2560/600?image=733"
---

오늘은 파이썬으로 초 간단 웹 크롤러를 만들어보기로 한다. 

웹 크롤러, 웹 스크레이퍼란 웹에서 필요한 정보를 찾아 저장하는 프로그램 코드를 말한다.

흔히 웹 크롤러라고 하지만, 정확히는 웹 스크레이퍼라고 부르는 것이 맞다. 크롤링 역시 웹 스크레이핑이라고 부르는 것이 맞다.

<img src="https://searchenginewatch.com/wp-content/uploads/sites/25/2017/11/web-crawler.jpg" width="1040px" />

크롤러를 사용하면 다음과 같은 것들을 자동화할 수 있다.

- __부동산114에 올라온 새로운 분양정보__: 분양일, 분양세대수, 건설사, 주소 등 새로운 분양정보에 대한 정보를 분양정보 공지 게시판에서 긁어와 엑셀에 저장하게 한다.
- __삼성전자의 주가 추이__: 장 개장 후 매 1시간마다 KRX 홈페이지에서 삼성전자의 주가를 검색해 긁어오고 이를 그래프로 그려 추이를 알 수 있게 한다.
- __중고나라 카페 맥북 매물 알리미__: 한 시간에 한 번씩 중고나라에 "맥북 프로 15인치"를 검색해 새 매물이 있으면 나에게 이메일을 보내 알려준다.

그렇다면 이런 웹 스크레이퍼는 어떻게 만들 수 있을까?

### 파이썬(Python) 언어 설치하기
- [*파이썬 공식 문서* 설치 가이드](https://docs.python.org/3/using/windows.html#installing-python)
- [*점프 투 파이썬* 설치 가이드](https://wikidocs.net/8)
- [*점프 투 파이썬* 파이썬 기초 실습 준비하기](https://wikidocs.net/9)

### Requests 라이브러리 설치하기

- [Requests 라이브러리 공식문서](http://docs.python-requests.org/en/master/)
  
```
pip install requests
```

pip을 이용해 requests 라이브러리를 설치한다. 이 라이브러리는 웹으로부터 데이터를 GET하거나 POST할 수 있도록 도와준다.

requests 라이브러리가 설치되었다면, 한번 사용해보자. 

```python
import requests

req = requests.get("http://leonkong.surge.sh")
html = req.text
print(html)
```
> req.py

이제 cmd에서 이 파이썬 파일을 실행해보자.

    python req.py

굉장히 긴 내용이 프린트 되었다. 실제 출력 결과는 너무 기므로 생략한다. 스크롤을 위로 올려보면 html 코드를 가져온 것을 알 수 있다.


이처럼 Requests 라이브러리를 이용하면 간단하게 웹페이지의 코드를 통째로 가져올 수 있다.


### Beautiful Soup 라이브러리 설치하기
- [Beautifulsoup 설치 방법](https://shaeod.tistory.com/900)
  

```
pip install beautifulsoup4
```

Beautiful Soup는 html을 파싱해서 편하게 다룰 수 있도록 도와주는 라이브러리이다.

```html
<html>
  <head>
    <title>세상의 모든 딜, 몬스터 딜</title>
  </head>
  <body>
    <h1>MonsterDeals</h1>
    <div class="item-container">
      <ul>
        <li>MacBook Pro</li>
        </li>iPhone X</li>
      </ul>
    </div>
  </body>
</html>
```

보통 requests 라이브러리로 웹페이지를 가져오면 위와 같은 코드로 되어있음을 알 수 있다.

만약 여기서 나에게 필요한 정보는 사이트 이름과 그 사이트에 올라온 제품들의 이름이라고 하자.

우리에게 필요한 정보를 뽑아내면 다음과 같을 것이다.
- 사이트 이름: "세상의 모든 딜, 몬스터 딜"
- 제품 이름: "MacBOOK Pro", "iPhone X"

Beautiful Soup는 위와 같은 html 코드를 받아 title, div, ul, li 태그 등 불필요한 태그들을 모두 지우고 우리에게 꼭 필요한 정보만을 정제(refine)해서 넘겨주는 라이브러리이다.

### 초간단 웹 스크레이퍼 만들기
그럼 실제로 사용 가능한 초간단 웹 스크레이퍼를 만들어 보기로 한다.

연습이 목적이므로 스크레이핑 대상은 필자의 블로그로 한다. 재미있는 내용을 스크레이핑 하는 것은 아니지만, 기본을 다지기 위함이니 조금만 참아주길 바란다.

> http://leonkong.surge.sh

위 링크가 오늘의 해부학 실습 대상이다. 마음껏 해부해주길 바란다.

오늘은 leonkong.surge.sh 라는 블로그에서 포스트들의 제목만을 가져와 print하는 프로그램을 파이썬으로 작성해볼 것이다.

완성된 프로그램의 코드는 다음과 같다.

> simple_crawler.py

```python
import requests
from bs4 import BeautifulSoup

# Get html text by using requests library
req = requests.get("http://leonkong.surge.sh")
html = req.text

# Parse html text by using Beautiful Soup 4
soup = BeautifulSoup(html, 'html.parser')
post_titles = soup.select('a > h3')

# Delete date info from the title and print
for title in post_titles:
    print(title.text.split("-")[0])
```

그러면 한 줄 한줄 작성해 나가 보도록 하자.

먼저 Requests와 Beautiful Soup 라이브러리를 Import 해준다. 

```python
import requests
from bs4 import BeautifulSoup
```

Requests를 이용해 leonkong.surge.sh 의 html 데이터를 가져온다.

```python
# Get html text by using requests library
req = requests.get("http://leonkong.surge.sh")
html = req.text
```

다음으로는 Beautiful Soup를 이용해 html을 파싱하고, 우리에게 필요한 포스트 제목을 발라낸다.

```python
# Parse html text by using Beautiful Soup 4
soup = BeautifulSoup(html, 'html.parser')
```
soup는 requests로 가져온 html을 Beautiful Soup를 이용해 python에서 읽을 수 있게 파싱한 정보를 담은 변수이다.

여기서 파싱이란 '번역'정도로 생각하면 된다. 파이썬에서 html을 직접 읽을 수는 없다. 이는 마치 로제타 석에 쓰인 이집트 상형문자 같은 것이기에, '파싱(parsing)'이라는 작업을 통해 현대의 언어로 번역하는 작업이 필요하다. 파싱은 사실 번역과는 조금 다르지만 번역 정도로 이해해도 충분하다.

다음 코드를 이어서 작성한다.
```python
post_titles = soup.select('a > div > h3')
```
이 코드를 통해 'a'태그 안에 있는 'h3'태그를 html 전체에서 모두 찾고, 그 결과물을 모아 'post_titles'라는 리스트(list)에 저장하게 된다.

html을 공부해보면 알 수 있는 사실이지만, html의 각 테그는 nested 형태를 띈다. 둥지의 알처럼 둥지 안에 보관되어 있다.

```html
<div>
  <a>
    <h3>블로그1</h3>
  </a>
  <a>
    <h3>블로그2</h3>
  </a>
</div>
```
오늘 실습하는 leonkong.surge.sh 를 살펴보면 대충 위와 같은 구조로 되어 있다. 블로그 제목을 담고 있는 h3 태그는 a 태그로 감싸져 있다. 또, 그 a 태그는 div 태그로 감싸져 있다.

우리는 Beautiful Soup를 이용해 a 태그 안에 있는 h3 태그를 모두 찾고 그 h3 태그 안의 내용('블로그1', 블로그2' 등의 내용)을 가져오는 것이다.

자 거의 다 왔다.

두과지 관문이 남아 있다. 먼저 다음과 같이 작성하고, 프로그램을 실행해보자.

```python
# Delete date info from the title and print
for title in post_titles:
    print(title)
```

> 실행결과

```html
<h3 class="index-h3">프론트엔드와 백엔드에 대하여1<!-- --> <span class="index-date" style="color:#bbb"> <!-- -->- <!-- -->30 September, 2018</span></h3>
<h3 class="index-h3">MonsterDeal 웹 어플리케이션 구축 청사진<!-- --> <span class="index-date" style="color:#bbb"> <!-- -->- <!-- -->26 September, 2018</span></h3>
<h3 class="index-h3">프로그래밍 언어<!-- --> <span class="index-date" style="color:#bbb"> <!-- -->- <!-- -->25 September, 2018</span></h3>
<h3 class="index-h3">네트워크와 웹이란 무엇인가 2<!-- --> <span class="index-date" style="color:#bbb"> <!-- -->- <!-- -->23 September, 2018</span></h3>
<h3 class="index-h3">네트워크와 웹이란 무엇인가 1<!-- --> <span class="index-date" style="color:#bbb"> <!-- -->- <!-- -->21 September, 2018</span></h3>
<h3 class="index-h3">Second Log<!-- --> <span class="index-date" style="color:#bbb"> <!-- -->- <!-- -->10 September, 2018</span></h3>
<h3 class="index-h3">First Log<!-- --> <span class="index-date" style="color:#bbb"> <!-- -->- <!-- -->10 August, 2017</span></h3>
```
실행해보니 a 태그 안의 h3 태그를 찾아주긴 찾아주었지만, h3 태그나 span 태그가 어지럽게 나와서 무엇이 제목인지 알기 힘들다. 아 망했다. 이럴바엔 크롤링을 하지 말걸 그랬다.

아니다. Beautiful Soup의 강력함은 바로 여기에서 온다.
단 한 줄의 코드, 아니, 정확히는 단 5 글자만 추가하면 우리는 제목을 가져올 수 있다.

방금 작성한 코드를 다음과 같이 바꿔보자.
```python
for title in post_titles:
    print(title.text)
````
print 문의 'title'을 'title.text'로 바꿔주었다.

프로그램을 실행해 출력된 결과를 확인해보자.

```
프론트엔드와 백엔드에 대하여1  - 30 September, 2018
MonsterDeal 웹 어플리케이션 구축 청사진  - 26 September, 2018
프로그래밍 언어  - 25 September, 2018
네트워크와 웹이란 무엇인가 2  - 23 September, 2018
네트워크와 웹이란 무엇인가 1  - 21 September, 2018
Second Log  - 10 September, 2018
First Log  - 10 August, 2017
```
훨씬 깔끔해졌다. 인간이 읽을 수 있는 상태가 되었다.

그러나 여기서 만족해서는 안 된다.
우리에게 필요한 건 __제목__ 이다. 작성 날짜와 제목이 함께 연결되어 있으면, 정보로서의 가치는 떨어진다.

나중에 이렇게 찾은 정보를 엑셀에 저장한다고 하였을 때, 제목과 작성일은 구분해서 저장하는 것이 옳지 않겠는가?

우리는 제목만을 발라내는 작업을 추가로 진행해보자.

먼저 위의 for문을 통째로 지우고 다음으로 수정한다.
```python
for title in post_titles:
    print(title.text.split("-"))
```
post_titles라는 리스트의 요소들을 하나하나 순회하면서 그 요소를 '.text'를 통해 발라내고, '.split("-")'을 통해 리스트에 나눠 저장한다.

무엇인지 모르겠다면 결과를 보자.
실행시켜보면 결과는 아래와 같다.
```
['프론트엔드와 백엔드에 대하여1  ', ' 30 September, 2018']
['MonsterDeal 웹 어플리케이션 구축 청사진  ', ' 26 September, 2018']
['프로그래밍 언어  ', ' 25 September, 2018']
['네트워크와 웹이란 무엇인가 2  ', ' 23 September, 2018']
['네트워크와 웹이란 무엇인가 1  ', ' 21 September, 2018']
['Second Log  ', ' 10 September, 2018']
['First Log  ', ' 10 August, 2017']
```
.split("-")은 문자열을 하이픈("-")을 기준으로 쪼개고 그 쪼갠 결과물을 리스트에 저장하게 하는 메소드(Method)이다.

    프론트엔드와 백엔드에 대하여1  - 30 September, 2018
보다시피 제목과 날짜는 하이픈으로 나뉘어 있다.
우리는 이 하이픈을 기준으로 한 줄이었던 문자열을 두 줄로 나누어 리스트에 넣은 것이다.

리스트는 원하는 요소만 골라 뽑아 출력할 수 있다.

```python
list_a = ['apple', 'mango', 'kiwi']

print(list_a[0])
>>> 'apple'

print(list_a[2])
>>> 'kiwi'
```
위와 같이 list_a의 원소들은 왼쪽부터 순서대로 0, 1, 2의 자리값(index)를 가지고, 각 자리값을 통해 그 위치의 원소만을 출력할 수 있다.

우리는 이 기능을 사용할 것이다.
우리의 코드를 한번 더 수정해보자.

```python
for title in post_titles:
    print(title.text.split("-")[0])
```

자 드디어 완성이다.
자랑스러운 마음을 가지고 프로그램을 실행해서 출력 결과를 확인하자.

> 출력결과
```
프론트엔드와 백엔드에 대하여1
MonsterDeal 웹 어플리케이션 구축 청사진
프로그래밍 언어
네트워크와 웹이란 무엇인가 2
네트워크와 웹이란 무엇인가 1
Second Log
First Log
```
우리는 제목만을 발라내어 제목 하나당 한 줄씩 출력하는 크롤러를 작성하는데 성공한 것이다!
