---
layout: post
title: "타입스크립트(TypeScript)의 타입들"
date: "2018-12-18"
categories:
- TypeScript
excerpt: |
  타입스크립트의 기본적인 타입들에 대해서 알아보고 자바스크립트와의 차이점을 살펴본다.
feature_text: |
  ## 타입스크립트와 타입 (Types of TypeScript)
  타입스크립트에는 어떤 타입이 있는가?
feature_image: "https://images.unsplash.com/photo-1514228742587-6b1558fcca3d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
image: "https://images.unsplash.com/photo-1514228742587-6b1558fcca3d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
---

> ## 목차 <br>
>  **1. 타입스크립트(TypeScript)에 대하여** <br>
>  **2. 타입스크립트의 타입** <br>
>  2.1 number와 string <br>
>  2.2 boolean <br>
>  2.3 array <br>
>  2.4tuple <br>
>  2.5 enum <br>
>  2.6 any <br>
>  **3. 함수와 타입** <br>
>  3.1 반환되는 결과에 타입 지정하기 <br>
>  3.2 void 타입 <br>
>  3.3 매개변수(Parameter)에 타입 지정하기 <br>
>  3.4 함수 타입 지정하기 <br>
>  **4. 객체와 타입** <br>
>  **5. 연습** <br>
> **마치며**


## 1. 타입스크립트(TypeScript)에 대하여
타입스크립트는 자바스크립트의 확장판(Superset)이다. 자바스크립트의 기능을 포함하고, 추가적인 기능을 제공한다.

타입스크립트는 자바스크립트에 비해 보다 강한 Type 관리를 제공한다. 즉, 타입에 대해 엄격하다.

타입스크립트는 자바스크립트랑 다르기 때문에 자바스크립트로 컴파일해야 이용 가능하다.

## 2. 타입스크립트의 타입
타입스크립트의 타입으로는 number, string, boolean, array, tuple, enum, any가 있다.

### 2.1 number와 string
자바스크립트의 Number과 거의 유사하다. 다만 몇가지 차이가 있다.

```javascript
let num = 13;
num += '23';

// JavaScript >>> '1323'
// TypeScript >>> Type 'string' is not assignable to type 'number'

let anotherNum = 23;
anotherNum = 'abc';
// Type '"ab"' is not assignable to type 'number'
```

자바스크립트에서는 Number와 String을 서로 더할 수 있었다. 이럴 경우 String이 반환되었다. 타입스크립트에서는 이런 것은 불가능하다.

타입스크립트는 변수를 선언할 때 자동으로 타입을 판단한다. 변수에 number가 할당되면 해당 변수의 타입은 number이다. 변수에 string이 할당되면 해당 변수의 타입은 string이다. 

변수이므로 다른 값을 할당할 수는 있다. 다만 해당 타입 이외의 값은 할당이 불가능하다. 즉, string에는 string만을, number에는 number만을 할당해야 한다.

```javascript
let num: number;
num = 23;
num = '23';
// Type '"ab"' is not assignable to type 'number'

let a;
a = 23;
a = '23';
```

타입스크립트에서는 명시적으로 변수의 타입을 선언할 수 있다. 위 코드에서처럼 타입을 명시해주면 해당 타입의 값만 할당할 수 있다. 

한편 변수를 선언만 하고 값을 할당하지 않는 경우, 타입을 명시적으로 함께 선언하지 않으면 'any'라는 타입으로 선언된다.

즉, 어떤 타입의 값이든 할당이 가능하다.

### 2.2 boolean
기본적으로 JavaScript의 boolean과 같다. 다만 다음과 같은 차이가 있다.

```javascript
let isTrue = false;
isTrue = 1;
// Type '1' is not assignable to type 'boolean'
```

타입스크립트에서 boolean으로 선언된 변수에는 0이나 1을 할당할 수 없다.오직 true와 false만 할당할 수 있다.

### 2.3 array
array의 선언은 조금 특이하다.

```javascript
let a: number[] = [1, 2, 3];

let b: string[] = ['a'];

let c: any[] = ['a', 1, 'b'];
```

배열의 경우 배열 내의 타입 관리는 엄격한 편이다. 
다음을 보자.

```javascript
let arr = [1, 2, 3];
arr.push("a");
// Argument of type '"a"' is not assignable to parameter of type 'number'

let arrAny = [1, 'a', 3];
arrAny.push('s');
arrAny.push(33);
console.log(arrAny);
// [1, 'a', 3, 's', 33]
```

타입스크립트는 타입을 명시하지 않았을 경우 할당된 값을 통해 해당 변수에 할당된 타입을 확인한다. number로만 이루어진 배열은 number형 배열로 인식되며 여러 타입이 혼합되어 있을 경우엔 any형 배열로 인식된다. 당연히 boolean형, string형 등 여타 타입별로 배열이 다 존재한다.

### 2.4tuple
파이썬의 튜플과는 약간 다르다. 파이썬의 튜플은 불변성을 특징으로 한다. 배열 내에 값을 추가하거나 삭제할 수 없고 변경할 수도 없다.

타입스크립트의 튜플은 파이썬에 비해서는 덜 엄격한 편이다.

```javascript
let address: [string, number] = ['Baker st.', 32]

address.push(33);
address.pop();

address[0] = 23;
// Type '23' is not assignable to type 'string'
```

튜플로 선언되었지만 push도 pop도 가능하다. 하지만 명시된 타입만큼은 반드시 지켜져야 한다. 첫번째 요소는 string, 두번째 요소는 number이므로 이 순서와 타입은 반드시 지켜져야 한다.

### 2.5 enum
enum은 number의 표현력을 강화한 형태이다.

switch 문을 사용하다 보면 string 형태로 각 케이스를 설정하는 것은 타이포 발생으로 인한 오류의 가능성을 높이고, string이 number에 비해 메모리를 더 차지하므로 불리함을 알 수 있다.

enum은 내부적으로는 숫자로 취급되지만 프로그래머는 숫자가 아닌 string으로 볼 수 있다.

```javascript
enum Color {Gray, Green, Blue};
let myColor = Color.Green;

console.log(myColor);
// 1
```

enum 키워드를 사용하면 내부적으로 array처럼 index가 부여된다. array와의 차이점이 있다면 요소에 그 index가 직접 할당된다는 것이다.

### 2.6 any
마지막으로 any라는 타입이 있다. any는 무엇이든 할당가능하다. 즉 자바스크립트의 일반적인 변수와 같아진다.

따라서 any는 조심해 사용해야 한다. 타입스크립트의 타입과 관련된 수많은 장점들은 any라는 타입을 사용함으로써 전부 사라지게 된다. 자바스크립트와 같아지기 때문이다.

## 3. 함수와 타입
위에서 언급한 타입들은 함수에도 지정하거나 사용할 수 있다.

### 3.1 반환되는 결과에 타입 지정하기
함수가 특정 값을 반환한다면 그 반환되는 값의 타입을 함수를 선언하면서 지정할 수 있다.

```javascript
const myName = 'Leon';

function returnString(s): string {
  return s
};

console.log(returnString(myName));
```

### 3.2 void 타입
함수에서는 특별히 반환 값의 타입으로 void를 지정할 수 있다.

void는 값이 없음, 즉 리턴값이 없어야 함을 지정하는 것이다.

```javascript

const myName = "Leon"

function callName(name: string): void {
  console.log(name);
  return name;
};

callName(myName);
// Type 'string' is not assignable to type 'void'
```

### 3.3 매개변수(Parameter)에 타입 지정하기
타입스크립트에서는 매개변수가 있을 경우 타입을 지정해야 한다.

```javascript
const myName = "Leon"

function callName(name): void {
  console.log(name);
  return name;
};

callName(myName);
// Type 'string' is not assignable to type 'void'
```

타입을 지정하지 않아서 오류가 발생했다. 타입 지정은 다음처럼 하면 된다.

```javascript
function multiply(value1: number, value2: number): number {
  return value1 * value2
}
```

### 3.4 함수 타입 지정하기
타입스크립트에서는 변수에 함수타입을 지정할 수 있다. ES6 화살표함수 문법을 사용하면 된다. 이를 통해 매개변수의 타입과 반환값의 타입을 지정해둘 수 있다. 

```javascript
function multiply(value1: number, value2: number): number {
  return value1 * value2
}

let myFunc: (val1: number, val2: number) => number;

myFunc = multiply
console.log(myFunc(2, 4));
// 8
```

## 4. 객체와 타입

```javascript
let data = {
  name: "Leon",
  age: 27
}

data = {}
//Type '{}' is missing the following properties from type '{ name: string; age: number; }': name,age

data = {a: "a", b: 2}
// Type '{ a: string; b: number; }' is not assignable to type '{ name: string; age: number; }'.
// Object literal may only specify known properties, and 'a' does not exist in type '{ name: string; age: number; }'
```
객체에서도 타입스크립트는 자바스크립트와 약간 다르다. 타입을 지정하지 않고 data라는 객체를 만들었다. name과 age라는 프로퍼티가 담겨있는데, name에는 string이, age에는 number가 할당되어 있다. 타입스크립트는 이 타입들을 기억한다. 나아가 key값 역시 기억한다. 따라서 다른 키 값을 할당 하는 것은 에러를 발생시키며, value 값의 타입이 달라져도 에러를 발생시킨다.

## 5. 연습
```typescript
let bankAccount = {
    money: 2000,
    deposit(value) {
        this.money += value;
    }
};
 
let myself = {
    name: "Max",
    bankAccount: bankAccount,
    hobbies: ["Sports", "Cooking"]
};
 
myself.bankAccount.deposit(3000);
 
console.log(myself);
```
위 코드를 타입스크립트적으로 리팩토링 해보자. 최대한 타입을 활용해야 한다.

```typescript
let bankAccount: {money: number, deposit: (val: number) => void} = {
  money: 2000,
  deposit(value: number) {
    this.money += value;
  }
}
```
먼저 bankAccount라는 변수를 타입스크립트로 변환해 보았다. 객체의 각 프로퍼티에 대해 타입을 지정했다. deposit 함수의 경우 매개변수는 number 타입을, return값으로는 void 타입을 주었다. return이 없기 때문이다.

다음으로 myself라는 변수를 살펴보자.
```typescript
let myself: {name: string, bankAccount: {money: number, deposit: (val: number) => void}, hobbies: string[]} = {
    name: "Max",
    bankAccount: bankAccount,
    hobbies: ["Sports", "Cooking"]
}
```
조금 지저분하다. 아무래도 bankAccount를 새 타입으로 선언하는 것이 전체적인 코드를 깔끔하고 정돈되게 만들 것 같다.

전체 코드를 다음과 같이 바꾼다.
```typescript
type bankAccount = {money: number, deposit: (val: number) => void}
let bankAccount: bankAccount= {
    money: 2000,
    deposit(value: number) {
        this.money += value;
    }
}

let myself: {name: string, bankAccount: bankAccount, hobbies: string[]} = {
    name: "Max",
    bankAccount: bankAccount,
    hobbies: ["Sports", "Cooking"]
}

myself.bankAccount.deposit(3000)
console.log(myself)
```

## 마치며
타입스크립트는 강한 타입을 제공한다. 이를 통해 좀 더 탄탄하고 안정적인 개발을 가능하게 한다. 자바스크립트와 타입스크립트는 유사하다. 따라서 배우기 어렵지 않다. 다만, 타입과 관련해서는 약간의 규칙이 추가되는 정도다.

배우기 쉬우면서도 자바스크립트의 단점들을 개선한 타입스크립트. 과연 주목받을만 하다.





