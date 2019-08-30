---
layout: post
title: "분할정복을 이용한 자바스크립트 병합정렬 알고리즘"
date: "2019-01-04"
categories:
- Algorithm
excerpt: |
  자바스크립트에서 분할정복을 이용해 병합정렬 알고리즘을 구현해본다.
feature_text: |
  ## 분할정복을 이용한 자바스크립트 병합정렬 알고리즘
  자바스크립트에서 분할정복을 이용해 병합정렬 알고리즘을 구현해본다.
feature_image: "https://images.unsplash.com/photo-1491895200222-0fc4a4c35e18?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1567&q=80"
image: "https://images.unsplash.com/photo-1491895200222-0fc4a4c35e18?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1567&q=80"
---

## 1. 병합정렬과 분할정복
병합정렬은 대표적인 분할정복 알고리즘의 사례이다. 분할정복은 주어진 문제를 둘 이상의 부분 문제로 나눈 뒤 각 문제에 대한 답을 재귀 호출을 이용해 계산하고, 각 부분 문제의 답으로부터 전체 문제의 답을 계산해 낸다.

분할정복을 이용해 문제를 풀기 위해서는 둘 이상의 부분 문제로 나누는 자연스러운 방법이 있어야 한다. 또, 부분 문제의 답을 조합해 원래 문제의 답을 계산하는 효율적인 방법이 있어야 한다.<br><br>
### 1.1 분할정복의 구성요소
- 문제를 더 작은 문제로 분할하는 과정(divide)
- 각 문제에 대해 구한 답을 원래 문제에 대한 답으로 병합하는 과정(merge)
- 더 이상 답을 분할하지 않고 곧바로 풀 수 있는 Base Case.
<br><br>
### 1.2 분할정복의 장점
분할정복은 많은 경우 같은 작업을 더 빠르게 처리해준다. 병합정렬의 경우 시간복잡도가 O(logn)으로 O(n^2)까지 걸릴 수 있는 선택정렬 등의 정렬법에 비해 빠른 결과를 보여준다.
<br><br>
## 2. 구현
```javascript
function mergeSort(arr) {
  function divide(lo, hi) {
    if (lo === hi) return;
    let mid = parseInt((lo + hi) / 2);
    divide(lo, mid);
    divide(mid + 1, hi);
    merge(lo, mid, hi);
  }
  function merge(lo, mid, hi) {
    let left = lo;
    let right = mid + 1;
    const temp = [];
    while (left <= mid || right <= hi) {
      if (left <= mid && (right > hi || arr[left] < arr[right])) {
        temp.push(arr[left]);
        left += 1;
      } else {
        temp.push(arr[right]);
        right += 1;
      }
    }

    arr.splice(lo, hi - lo + 1, ...temp);
  }
  divide(0, arr.length - 1);
  return arr;
}
```

분할과정과 병합과정을 각각 함수로 나누어 병합정렬을 구현하였다.
먼저 분할 과정을 살펴보자.

```javascript
function divide(lo, hi) {
    if (lo === hi) return;
    let mid = parseInt((lo + hi) / 2);
    divide(lo, mid);
    divide(mid + 1, hi);
    merge(lo, mid, hi);
    }
```
분할 함수 divide()는 배열의 가장 작은 인덱스 lo와 가장 큰 인덱스 hi를 매기변수로 받는다. 이 인덱스들을 기준으로 분할작업을 실행하는데 lo===hi라면, 배열의 인자가 1개이므로 분할이 완료된 것으로 본다. 즉, 탈출조건(종료조건)은 lo===hi인 경우이다.

이외의 경우에 대해서는 lo ~ mid, mid+1 ~ hi 두 구간으로 나누어 분할 과정을 이어간다. 배열의 인자가 1개가 될 때까지 배열을 둘로 쪼개는 작업은 반복된다.

분할이 모두 완료되면 병합 함수 merge()가 호출된다. merge()는 lo, mid, hi를 매개변수로 받는다.

```javascript
  function merge(lo, mid, hi) {
    let left = lo;
    let right = mid + 1;
    const temp = [];
    while (left <= mid || right <= hi) {
      if (left <= mid && (right > hi || arr[left] < arr[right])) {
        temp.push(arr[left]);
        left += 1;
      } else {
        temp.push(arr[right]);
        right += 1;
      }
    }

    arr.splice(lo, hi - lo + 1, ...temp);
  }
```
병합 함수를 살펴보자, 먼저 left에는 lo를, right에는 mid+1을 할당한다. left와 right에서부터 한칸씩 우측으로 이동하며 값을 비교하고, 작은 값을 왼쪽에, 큰 값을 오른쪽에 삽입하는 방식으로 정렬 작업을 진행할 것이다.

병합정렬에서 실제 정렬이 이뤄지는 것은 바로 이 병합 함수이다. temp라는 임시 배열에는 정렬된 값을 추가해 보관한다.

먼저, left가 mid보다 같거나 작고 right가 hi보다 같거나 작은 경우에 한해 병합 작업을 진행하는데, left가 mid보다 크고, right가 hi 보다 크면 더 이상 원소가 없게 되어 병합 과정이 끝난 것이기 때문이다.

left가 mid보다 같거나 작은 경우 중 right가 hi보다 크거나(right 부분의 정렬이 끝났거나) arr[left]가 arr[right]보다 작은 경우, temp에 arr[left]를 추가한다. 이후 left의 값을 1 증가시킨다. 이외의 경우엔 arr[right]를 temp에 추가하고 right의 값을 1 증가시킨다.

```javascript
arr.splice(lo, hi - lo + 1, ...temp);
```
마지막으로 arr에서 lo부터 hi까지를 temp로 대체하는 작업을 해준다. splice를 이용해 lo부터 hi까지를 temp의 인자들로 바꿔준다.

```javascript
  divide(0, arr.length - 1);
  return arr;
```
divide() 함수를 호출하는데, 0과 arr.length-1을 매기변수에 넣어준다. 즉, 리스트 전체에 대해 분할을 진행하라는 의미이다.

이후 정렬이 완료된 arr을 반환하며 mergeSort() 함수를 종료한다.
<br><br>
## 출처
- 프로그래밍 대회에서 배우는 알고리즘 문제해결전략(구종만, 인사이트)