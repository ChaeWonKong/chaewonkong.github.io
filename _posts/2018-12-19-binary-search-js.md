---
layout: post
title: "자바스크립트 이진탐색 알고리즘"
date: "2018-12-19"
categories:
- Algorithm
excerpt: |
  자바스크립트의 재귀호출을 활용해 정렬된 리스트에서 target 요소의 인덱스를 구하는 알고리즘을 구현해본다.
feature_text: |
  ## 자바스크립트 이진탐색 알고리즘
  자바스크립트에서 재귀호출을 이용해 이진탐색 함수를 구현한다.
feature_image: "https://images.unsplash.com/photo-1509228468518-180dd4864904?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80"
image: "https://images.unsplash.com/photo-1509228468518-180dd4864904?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80"
---

## 이진탐색
이진탐색에 대한 내용은 이미 파이썬 이진탐색 알고리즘에서 한 번 살펴본 바 있다. 아래 링크를 참조하길 바란다.

[파이썬 이진탐색 알고리즘](https://chaewonkong.github.io/algorithm/2018/12/11/binary-search/)

## 자바스크립트에서의 이진탐색
자바스크립트에서 이진탐색을 구현하는 것 또한 파이썬과 별반 다르지 않다. 함수 안에 함수를 두고, 재귀호출을 이용해 리스트의 길이가 1이 될 때까지 리스트를 반씩 쪼개나가면서 탐색을 진행하면 된다.

## 코드
완성된 코드는 다음과 같다.

```javascript
function binarySearch(arr, target) {
  function search(lo, hi) {
    const mid = parseInt((lo + hi) / 2);
    if (lo === hi) {
      if (arr[lo] != target) return -1;
      return lo;
    } else if (target > arr[mid]) {
      return search(mid + 1, hi);
    } else {
      return search(lo, mid);
    }
  }

  return search(0, arr.length - 1);
}
```

코드를 단위별로 뜯어보며 살펴보자.
```javascript
function binarySearch(arr, target)
```
실제로 호출될 함수이며, 정렬된 배열과 찾고자 하는 target을 매개변수로 받는다.
```javascript
function search(lo, hi)
```
binarySearch 함수 내에 존재하는 함수로 재귀호출시 호출되며 타겟 값의 인덱스를 찾아나갈 함수이다.
```javascript
const mid = parseInt((lo + hi) / 2);
```
mid는 배열의 중앙 인덱스를 저장한다. parseInt를 이용해 항상 음이 아닌 정수가 되게 한다. mid를 기준으로 좌우를 두개의 배열로 나누고 target이 속하는 배열을 선택해 다시 mid를 찾고 또 나눈다.

이렇게 target쪽으로 포커스를 좁혀가며 배열을 반씩 나누다 보면 배열의 길이가 어느덧 1이 된다.
```javascript
if (lo === hi) {
      if (arr[lo] != target) return -1;
      return lo;
    }
```
배열의 길이가 1이라는 것은 lo === hi를 의미하기도 한다. 이것이 탈출조건이다. 

그러나 주의할 것이 있다. target이 주어진 배열 arr 내에 없는 경우이다. 이런 경우에는 -1을 반환하는 것이 관례이다.

따라서, 배열의 길이가 1이 될 때까지 좁혀나가며 탐색을 했는데, 그 배열의 유일한 요소가 target과 다르다면 -1을 반환한다.

만약 그 배열의 요소가 target과 같다면, 지금까지 찾은 인덱스를 반환한다.

여기서 lo === hi이므로 lo나 hi 중 아무 것이든 반환하면 된다.

```javascript
else if (target > arr[mid]) {
      return search(mid + 1, hi);
    } else {
      return search(lo, mid);
    }
```
만약 배열의 길이가 1이 아니라면 탐색을 계속 진행한다. target이 mid를 인덱스로 가지는 배열의 요소보다 크다면 mid+1 ~ hi까지만 탐색하면 된다.

target이 mid를 인덱스로 가지는 배열의 요소보다 작다면, lo ~ mid까지만 탐색하면 된다.

이 과정에서 처음 받은 리스트를 반씩 둘로 쪼개고, target이 왼쪽, 오른쪽 부분리스트 중 어느 리스트에 들어가는지 확인한 후, target이 포함되어 있을 리스트를 대상으로 재귀호출을 진행해 쪼개기 작업을 게속하면 된다.

## 테스트
```javascript
const arr = [1, 2, 3, 4, 5];
for (let n = 0; n < 6; n++) {
    console.log(binarySearch(arr, n))
};
```
위 블록을 코드의 밑에 추가해 간단하게 잘 작동하는지 테스트해 본다.

```javascript
-1
0
1
2
3
4
```
0은 arr 내에 없기 때문에 -1을 반환했고, 나머지 요소들은 전부 발견되어 인덱스가 정상적으로 반환되었다.