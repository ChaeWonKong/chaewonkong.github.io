---
layout: post
title: "자바스크립트 알고리즘 - n개의 원소 중 m개를 뽑는 모든 조합"
date: "2018-12-24"
categories:
- Algorithm
excerpt: |
  자바스크립트에서 n개의 원소 중 m개를 뽑는 모든 조합을 찾는 알고리즘을 구현해본다.
feature_text: |
  ## JS Algorithm - n개의 원소 중 m개를 뽑는 모든 조합
  자바스크립트에서 n개의 원소 중 m개를 뽑는 모든 조합을 찾는 알고리즘을 구현해본다.
feature_image: "https://images.unsplash.com/photo-1491895200222-0fc4a4c35e18?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1567&q=80"
image: "https://images.unsplash.com/photo-1491895200222-0fc4a4c35e18?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1567&q=80"
---
## 문제
n개의 원소로 이루어진 정렬된 배열 arr에 대하여 m개를 뽑는 모든 조합을 배열로 출력하기.

## 전체 코드
```javascript
function getAllCombinations(arr, m) {
  const combinations = [];
  const picked = [];
  const used = [];
  for (item of arr) used.push(0);

  function find(picked) {
    if (picked.length === m) {
      const rst = [];
      for (let i of picked) {
        rst.push(arr[i]);
      }
      combinations.push(rst);
      return;
    } else {
      let start = picked.length ? picked[picked.length-1] + 1 : 0;
      for (let i = start; i < arr.length; i++) {
        if (i === 0 || arr[i] !== arr[i-1] || used[i-1]) {
          picked.push(i);
          used[i] = 1;
          find(picked);
          picked.pop();
          used[i] = 0;
        }
      }
    }
  }
  find(picked);
  return combinations;
}
```

## 예제 확인
```javascript
const arr = [1, 2, 2, 3];
const m = 3;
console.log(getAllCombinations(arr, m));
// [[1, 2, 2], [1, 2, 3], [2, 2, 3]]
```

## 풀이
getAllCombination 함수는 배열 arr과 양의정수 m을 매개변수로 받는다. 이를 활용해 arr에서 m개의 조합을 선택하는 모든 경우를 배열로 반환한다.

```javascript
const combinations = [];
const picked = [];
const used = [];
for (item of arr) used.push(0);
```
먼저 각 조합들을 담을 배열 combinations를 빈 배열로 선언한다. 

이후 picked를 빈 배열로 선언하는데, 이 배열에는 선택한 요소의 인덱스가 담길 것이다.

다음으로 used라는 빈 배열을 선언하고, arr의 길이만큼 used에 0을 요소로 추가한다. used는 arr의 요소 중 같은 요소가 2개 이상일 경우, 중복되는 조합이 거듭 combinations에 추가되는 것을 막기 위해 사용한다.

```javascript
function find(picked) {}
```
다음으로는 find라는 함수를 선언하며 매개변수로는 picked를 준다.

```javascript
if (picked.length === m) {
    const rst = [];
    for (let i of picked) {
    rst.push(arr[i]);
    }
    combinations.push(rst);
    return;
}
```

탈출조건(종료조건)을 추가한다. 만약 m개를 모두 뽑아 picked의 길이가 m과 같아진다면 종료한다.

종료할 때는 rst라는 배열에 picked에 속한 인덱스를 바탕으로 arr의 실제 요소를 담는다. rst가 완성되면 combinations에 담고 return을 통해 함수를 종료한다.

```javascript
else {
    let start = picked.length ? picked[picked.length-1] + 1 : 0;
    for (let i = start; i < arr.length; i++) {
    if (i === 0 || arr[i] !== arr[i-1] || used[i-1]) {
        picked.push(i);
        used[i] = 1;
        find(picked);
        picked.pop();
        used[i] = 0;
    }
    }
}
```
시작 인덱스를 담기 위해 변수 start를 선언하는데, 만약 picked에 이미 인덱스가 있다면 그 인덱스보다 1 큰 수를, 없다면 0을 start로 선언한다.

for문을 통해 start부터 arr.length-1까지 수 i에 대해 뽑기를 진행한다. 단, if문 조건을 만족하는 경우에 한해 뽑는다.

만약 i가 0, 즉 첫 원소이면 무조건 뽑는다. 혹은 arr[i]와 arr[i-1]이 서로 같지 않다면, 역시 뽑는다. 마지막으로 used[i-1]이 이미 뽑혀서 1인 경우, i를 뽑는다.

뽑게 된 후엔 picked에 i를 담고, used[i]는 1로 바꿔준다.
이후 find(picked)를 통해 재귀호출을 진행한다.
재귀 이후엔 picked.pop()을 통해 가장 마지막에 추가한 요소를 제거해 다음 요소를 탐색할 수 있게 한다. used[i]도 0으로 초기화 해준다.

```javascript
find(picked);
return combinations;
```

마지막으로 find함수를 실행시켜 주고, 재귀가 끝나면 combinations를 반환한다.