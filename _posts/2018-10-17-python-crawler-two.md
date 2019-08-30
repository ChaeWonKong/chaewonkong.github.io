---
layout: post
title: "파이썬으로 웹 크롤러 만들기 2"
date: "2018-10-17"
categories:
- DevLog
excerpt: |
  파이썬으로 만든 웹 크롤러로 정보를 긁어오고 이를 엑셀 파일로 저장한다.
feature_text: |
  ## 파이썬으로 웹 크롤러 만들기 2
  파이썬으로 긁어온 정보를 엑셀로 저장하자
feature_image: "https://picsum.photos/2560/600?image=733"
image: "https://picsum.photos/2560/600?image=733"
---

오늘은 이전 시간에 이어 파이썬으로 웹 크롤러를 만들어 본다. 기존에 긁어 온 데이터를 마이크로소프트 오피스의 엑셀 파일로 저장하는 법을 배운다. 오늘의 튜토리얼을 따라하게 되면 크롤링한 데이터를 .xlsx 파일 형식으로 저장할 수 있다.

먼저 기존에 완성했던 크롤러의 코드를 살펴보자.

```python
import requests
from bs4 import BeautifulSoup
from openpyxl import Workbook

# Get html text by using requests library
req = requests.get("http://leonkong.surge.sh")
html = req.text

# Parse html text by using Beautiful Soup 4
soup = BeautifulSoup(html, 'html.parser')
post_titles = soup.select('a > div > h3')

# Delete date info from the title and print outcome
for title in post_titles:
    print(title.text.split("-")[0])
```

이전시간에는 가져온 포스트 제목에서 날짜를 발라내 print하게 명령했었다.

이번에는 가져온 데이터를 엑셀로 저장하는 방법을 배운다. 포스트 제목과 날짜를 차곡차곡 엑셀 파일로 정리할 것이다.

### 포스트 제목과 작성날짜를 따로 리스트에 저장하기
먼저 위의 코드에서 마지막 부분을 다음과 같이 수정한다.
titles에는 포스트 제목들을, dates에는 포스트 작성날짜들을 저장할 것이다.

```python
# Delete date info from the title and add it to results
titles = []
dates = []
for title in post_titles:
    raw_title = title.text.split('-')
    titles.append(raw_title[0])
    dates.append(raw_title[1])
```

결과를 한번 확인해 보기로 한다.

방금 고친 코드 밑에 다음의 코드를 추가한다.

```python
print(titles)
print(dates)
```

```python
['개발자를 위한 링크 및 공부법  ', '파이썬으로 웹 크롤러 만들기 1  ', '프론트엔드와 백엔드에 대하여1  ', '프로그래밍 언어  ', '네트워크와 웹이란 무엇인가 2  ', '네트워크와 웹이란 무엇인가 1  ', 'Second Log', 'First Log  ']
[' 08 October, 2018', ' 02 October, 2018', ' 30 September, 2018', ' 25 September, 2018', ' 23 September, 2018', ' 21 September, 2018', ' 10 September, 2018', ' 10 August, 2017']
```

첫번째 리스트에는 포스트 제목이, 두번째 리스트에는 포스트 날짜가 담긴 것을 확인할 수 있다.

이제는 방금 작성한 두 줄의 print문을 지우고 다음 과정으로 넘어간다.

### openpyxl 설치하기
openpyxl은 파이썬으로 엑셀파일을 다룰 수 있는 라이브러리이다. 이 라이브러리를 설치하도록 한다.

```
pip install openpyxl
```

설치했으면, req.py 파일의 제일 윗부분에 다음 import 문을 추가한다.

```python
from openpyxl import Workbook
```

그리고 데이터를 작성할 공간을 만든다. 새 엑셀파일을 연다고 생각하면 된다.

기존에 작성하던 코드의 맨 밑쪽에 다음을 추가한다.

```python
wb = Workbook()
ws = wb.active #grab the active worksheet

# Write table head on the worksheet
ws['A1'] = 'ID'
ws['B1'] = 'TITLE'

# Write actual data on the worksheet
for i in range(len(titles)):
    pos = str(i+2)
    ws['A' + pos] = titles[i]
    ws['B' + pos] = dates[i]
```

for문은 titles에 속한 아이템의 개수만큼 반복된다. 여기서 titles와 dates는 개수가 같다(모든 포스트는 제목 1개와 작성날짜 1개를 가지므로). 따라서 titles의 리스트 아이템 개수만큼 순회하는 것은 dates 리스트의 아이템 개수만큼 순회하는 것과 결과적으로 같다.

pos는 엑셀상 위치를 지정하기 위한 변수이다. i는 0부터 시작한다. 하지만 엑셀의 셀(Cell)은 1이 시작점이다.

한편, A1, B1에는 각각 TITLE, DATE를 입력해두었으므로, 데이터는 2열부터 순차적으로 들어가야 한다. 따라서 i에 2를 더해 pos라는 변수로 설정한 것이다. (i를 2부터 시작하게 만들기 위함)

### 작성한 내용을 엑셀파일로 저장하기
이제 마지막으로 지금까지 작성한 내용이 엑셀파일로 생성/저장되도록 만들어보자.

이어서 코드를 작성한다.

```python
# Save as .xlsx file
wb.save("posts.xlsx")
```

자 이제 req.py 파일이 있는 디렉토리로 이동해 xlsx 파일이 제대로 생성되었는지 확인해보자.

확인했으면 파일을 엑셀로 열어 제대로 작성되었는지도 확인해보자.

이제 크롤링한 데이터를 엑셀에 저장할 수 있게 되었다.

Congratulations!!

전체 코드 보기
```python
import requests
from bs4 import BeautifulSoup
from openpyxl import Workbook

# Get html text by using requests library
req = requests.get("http://leonkong.surge.sh")
html = req.text

# Parse html text by using Beautiful Soup 4
soup = BeautifulSoup(html, 'html.parser')
post_titles = soup.select('a > div > h3')

# Delete date info from the title and add it to results
titles = []
dates = []
for title in post_titles:
    raw_title = title.text.split('-')
    titles.append(raw_title[0])
    dates.append(raw_title[1])

# Save data in a .xlsx excel format
wb = Workbook()
ws = wb.active # Grab the active worksheet

# Write data on worksheet
ws['A1'] = 'Title'
ws['B1'] = 'Date'


for i in range(len(titles)):
    pos = str(i+2)
    ws['A' + pos] = titles[i]
    ws['B' + pos] = dates[i]

# Save the file
wb.save("posts.xlsx")
```
