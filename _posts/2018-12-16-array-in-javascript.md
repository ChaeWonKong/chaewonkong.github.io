---
layout: post
title: "자바스크립트의 배열(Array in JavaScript)"
date: "2018-12-16"
categories:
- JavaScript
excerpt: |
  자바스크립트(JavaScript)의 배열에 대해 알아보고 그 배열을 조작하는 방법을 살펴본다.
feature_text: |
  ## 자바스크립트의 배열(Array in JS)
  자바스크립트의 배열은 어떤 특징을 가지고 있는지, 배열을 조작하는 방법에는 어떤 것들이 있는지 알아본다.
feature_image: "https://images.unsplash.com/photo-1518600654093-2a24cddafa38?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2017&q=80"
image: "https://images.unsplash.com/photo-1518600654093-2a24cddafa38?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2017&q=800"
---

## 자바스크립트의 배열
프로그래밍을 하다보면 데이터를 집합적으로 모아 관리할 필요성이 많다. 자바스크립트에서는 이럴 경우 객체나 배열을 사용한다.

자바스크립트의 배열은 파이썬의 리스트(List)와 거의 같다. 배열은 순서가 있는 데이터 집합으로 배열의 첫 요소의 인덱스는 0부터 시작한다.

배열은 대괄호로 표현되며 각 요소는 쉼표로 구분된다. 배열의 길이는 배열 내 요소들의 개수이며 length 프로퍼티로 확인 가능하다.

```javascript
const arr = [1, 2, 3, 4, 5];
arr[0] = 1;
arr.length = 5;
```

배열의 요소는 숫자나 문자열, 객체나 심지어 배열일 수 있다. 배열의 요소들은 서로 다른 타입이어도 상관 없다.

```javascript
const arr = [1, 'a', [2, 3], {a:15}]
```

자바스크립트에서는 배열의 길이보다 큰 인덱스를 사용해 새로운 요소를 추가할 수 있다. 만약 빈 자리가 있다면 그 빈 자리는 undefined로 채워진다.

```javascript
let arr = [1, 2, 3, 4, 5];
arr.length = 5;
arr[5] = 6;
console.log(arr);
// [1, 2, 3, 4, 5, 6]

arr[7] = 8;
console.log(arr);
// [1, 2, 3, 4, 5, 6, undefined, 8]
```

이는 파이썬(Python)과는 다르다. 파이썬에서는 객체의 길이보다 큰 인덱스로 배열의 새 요소를 추가하는 것이 불가능하다.

```python
arr = [1, 2, 3, 4, 5]
arr[5] = 6
# IndexError: list assignment index out of range
```

이처럼 파이썬에서는 인덱스 에러가 발생하게 된다.

자주 사용되지는 않지만 Array 생성자를 통해서도 배열을 생성할 수 있다.

```javascript
const arr1 = new Array();
// []

const arr2 = new Array(3);
// [undefined, undefined, undefined]

const arr3 = new Array("2");
// ["2"]

const arr4 = new Array(1, 2, 3);
// [1, 2, 3]
```
Array 생성자의 매개변수가 없으면 빈 배열이, 매개변수가 정수 1개이면 해당 정수 만큼의 undefined를 포함한 배열이 생성된다. 매개변수가 2개 이상이거나 정수가 아닌 경우에는 해당 매개변수가 포함된 배열이 생성된다.

## 배열의 요소 조작
배열의 메소드(Method)를 이용하면 배열 자체를 수정하거나 새 배열을 생성할 수 있다. 일부 매소드는 배열 자체를 수정하고, 일부 메소드는 새 배열을 반환한다. 매소드 이름에서 이런 차이를 확인하는 것은 불가능하므로, 직접 전부 외워야 한다.

### 배열의 처음에 요소 하나를 추가하거나 제거하기
unshift를 이용하면 배열의 처음에 요소 한개를 추가할 수 있고, shift를 이용하면 배열의 처음에서 하나의 요소를 제거할 수 있다.

```javascript
const arr = [1, 2, 3];
arr.unshift(0);
// 4
console.log(arr);
// [0, 1, 2, 3]

arr.shift();
// 0
console.log(arr);
// [1, 2, 3]
```

unshift와 뒤에서 배울 push 처럼 배열에 요소를 추가할 경우 괄호 안에 추가할 요소를 포함한다. 한편, 결과로는 늘어난 상태에서의 길이를 반환한다.

반면 shift와 뒤에서 배울 pop처럼 배열의 요소를 제거할경우 괄호에 따로 요소를 명시하지 않으며, 제거한 요소가 반환된다.

### 배열의 끝에 요소 하나를 추가하거나 제거하기
push를 이용하면 배열의 끝에 새 요소를 추가할 수 있고, pop을 이용하면 배열의 끝 요소를 제거할 수 있다.

```javascript
const arr = [1, 2, 3];
arr.push(0);
// 4
console.log(arr);
// [1, 2, 3, 0]

arr.pop();
// 0
console.log(arr);
// [1, 2, 3]
```

### 배열의 끝에 여러 요소 추가하기
concat을 이용하면 배열의 끝에 여러 요소를 추가하거나 두 개의 배열을 합칠 수 있다. 다만 원래 배열은 바뀌지 않으며 바뀐 사본이 반환된다. concat은 배열을 한 번 까지만 분해하므로 인자로 배열을 포함한 배열을 주게 되면 내부의 배열은 분해 후 결합되는 것이 아니라 배열인 상태로 결합된다.

```javascript
const arr = [1, 2, 3];
arr.concat(4, 5, 6);
// [1, 2, 3, 4, 5, 6]
console.log(arr);
// [1, 2, 3]

arr.concat([4, 5, 6]);
// [1, 2, 3, 4, 5, 6]
console.log(arr);
// [1, 2, 3]

arr.concat([[4, 5, 6]]);
// [1, 2, 3, [4, 5, 6]]
console.log(arr);
// [1, 2, 3]
```

### 배열 일부 가져오기
배열의 일부만을 가져올 때는 slice를 사용한다. slice는 최대 2개의 매개변수를 받는데 첫번째 매개변수는 가져올 시작점을, 두번째 매개변수는 가져올 끝점(바로 직전 인덱스까지만 가져옴)을 지정한다. 두번째 매개변수는 생략가능하며, 생략시 배열의 마지막까지를 반환한다. slice의 매개변수에는 음수 인덱스를 쓸 수 있다. -1은 배열의 끝, -2는 배열의 끝에서 직전을 의미한다. 이런식으로 -를 붙이면 배열의 뒤에서부터 위치를 셀 수 있다.

```javascript
const arr = [1, 2, 3, 4, 5];
arr.slice(3);
// [4, 5]
console.log(arr);
// [1, 2, 3, 4, 5]

arr.slice(2:4);
// [3, 4]
console.log(arr);
// [1, 2, 3, 4, 5]

arr.slice(-2, -1);
// [4]
console.log(arr);
// [1, 2, 3, 4, 5]
```

### 임의의 위치에 요소 추가/삭제하기
splice를 활용하면 배열의 임의의 위치에 대해 요소를 추가하거나 삭제할 수 있다.

```javascript
const arr = [1, 2, 3];
arr.splice(1, 0, 4, 5);
// arr = [1, 4, 5, 2, 3]
```

splice의 첫번째 매개변수는 수정을 시작할 인덱스, 두번째는 삭제할 요소의 숫자, 세번째부터는 배열에 추가될 요소를 의미한다.
splice 매소드는 실행 후 삭제한 요소를 배열로 반환한다. 삭제한 요소가 없으면 빈 배열이 반환된다.

즉 splice(1, 2)는 1번째부터 2개의 요소를 배열에서 삭제하고 반환한다.

### 배열 안에서 요소 교체하기
copyWith은 배열 요소를 복사해 다른 위치에 붙여넣고, 기존의 요소를 덮어쓴다. 첫번째 매개변수는 복사한 요소를 붙여넣을 위치이고 두번째는 복사를 시작할 위치, 세번째는 복사를 끝낼 위치이다.

```javascript
const arr = [1, 2, 3, 4];
arr.copyWith(1, 3);
// arr = [1, 4, 3, 4]
```

### 특정 값으로 배열 채우기
fill이라는 매소드를 사용하면 정해진 값으로 배열을 채울 수 있다. Array 생성자와 함께 사용하면 다음과 같은 효과를 낼 수 있다.

```javascript
const arr = new Array(5).fill(1);
// 1로 채워진 길이가 5인 배열 [1, 1, 1, 1, 1]이 탄생
```

fill의 첫번째 매개변수는 채워넣을 값을, 두번째는 채워넣기 시작할 위치를, 세번째는 채워넣기를 끝낼 위치를 의미한다.

두번째, 세번째 매개변수를 주지 않으면 모든 값을 첫번째 매개변수로 할당된 값으로 채워넣게 된다.

### 배열의 정렬과 역순
sort는 배열요소의 순서를 정렬한다. reverse는 주어진 배열을 역순으로 바꾼다. reverse는 정렬은 하지 않고 순서만 역순으로 한다.

```javascript
const arr = [1, 4, 5, 3, 2];
arr.sort();
// arr = [1, 2, 3, 4, 5]

arr.reverse();
// arr = [5, 4, 3, 2, 1]
```

sort는 정렬함수를 받을 수 있다. 
```javascript
const arr = [{name: 'b'}, {name: 'a'}]

arr.sort((a, b) => a.name > b.name);
// name 프로퍼티의 알파벳 순으로 정렬
// [{name: 'a'}, {name: 'b'}]
```
함수 (a, b) => a.name > b.name은 불리언을 반환한다. 함수가 true를 반환하면 정렬을 수행하고, false를 반환하면 정렬을 종료한다. 따라서 숫자 0을 반환하게 해도 정렬은 종료된다.


### 임의의 위치에 요소를 추가하거나 제거하기