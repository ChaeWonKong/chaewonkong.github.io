---
layout: post
title: "파이썬(Python) 데코레이터(Decorator)"
date: "2019-02-02"
categories:
- Algorithm
excerpt: |
  파이썬(Python)과 데코레이터(Decorator)에 대해 알아보고, 데코레이터를 사용해 n 번째 피보나치(Fibonacci) 수를 찾는 알고리즘을 구현해본다.
feature_text: |
  ## 파이썬(Python) 데코레이터(Decorator)
  파이썬에서 데코레이터를 사용해 피보나치 알고리즘을 구현해본다
feature_image: "https://images.unsplash.com/photo-1472289065668-ce650ac443d2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2850&q=80"
image: "https://images.unsplash.com/photo-1472289065668-ce650ac443d2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2850&q=80"
---
## 1. 파이썬 데코레이터
### 1.1 데코레이터란 무엇인가?
파이썬에서 함수는 First Class Object 이다. 즉, 함수는 다른 객체처럼 여기 저기 할당되어 사용될 수 있다는 것이다. 또 파이썬에서는 함수 안에 함수를 선언할 수 있다. 즉 클로저를 사용할 수 있다.

이러한 성격을 활용한 것이 바로 데코레이터다. 데코레이터는 메타프로그래밍(metaprogramming) 이라고도 불리는 데, 한 프로그램이 컴파일 될 때, 다른 프로그램이 그 프로그램에 영향을 미치기 때문이다.

함수 A, B가 있다고 하자. 우리는 함수 B가 실행될 때 마다, 함수 A로 감싸진 내부함수로 실행되길 원한다. 이 경우 우리는 데코레이터를 사용해 구현할 수 있다.

### 1.2 데코레이터, 어떻게 쓰는가?

```python
import time

def get_time(func):
    def wrapper(*args, **kargs):
        stime = time.time()
        v = func(*args, **kwargs)
        etime = time.time()
        return v, etime - stime

@get_time
def gauss(n):
    return n * (n+1) // 2
```

get_time 함수는 함수의 실행 시간을 측정하는 함수다. 우리가 시간을 측정하고 싶은 함수는 gauss 함수로 1부터 n까지를 모두 더하는 함수다.

데코레이터를 사용하면 마치 gauss함수를 get_time의 wrapper로 감싼 것과 같은 효과를 가질 수 있다.

### 1.3 데코레이터, 왜 쓰는가?
그렇다면 데코레이터를 왜 써야 하는가? 프로그래밍에는 결합도와 응집도라는 개념이 있다. 결합도는 낮을수록, 응집도는 높을 수록 좋은 코드이다. 결합도란 모듈 간의 상호 의존성을, 응집도란 모듈 내의 기능적 집중 정도를 말한다. 

우리는 gauss 함수 안에서 실행 시간을 측정할 수도 있다. 그러나 이는 효과적인 방법이 아니다. 왜냐면 가우스 함수의 본질은 1부터 n까지 더하는 것이지 시간을 측정하는 것이 아니기 때문이다.

따라서 아래 코드는 응집도가 낮은 코드가 된다.
```python
import time

def gauss(n):
    stime = time.time()
    ret = n * (n+1) // 2
    etime = time.time()
    return ret, etime - stime
```
한 함수 안에서 두 가지 기능이 작동하므로 함수의 본 의미가 희석되고 응집도가 낮아졌다.

한편, 다음과 같은 코드는 결합도가 높은 코드가 된다.
```python
import time

def gauss(n):
    return n * (n+1) // 2

def get_time(n):
    stime = time.time()
    ret = gauss(n)
    etime = time.time()
    return ret, etime - stime
```
get_time 함수는 gauss 함수에 의존적이다. gauss 함수가 존재하지 않으면 결코 실행될 수 없다. gauss 함수가 무엇인지 알아야만 get_time의 목적을 이해할 수 있다.

즉 결합도가 높다.

그러나 데코레이터를 사용하게 되면 응집도는 높이고 결합도는 낮출 수 있다. 다시 데코레이터를 사용한 코드를 살펴보자.

```python
import time

def get_time(func):
    def wrapper(*args, **kargs):
        stime = time.time()
        v = func(*args, **kwargs)
        etime = time.time()
        return v, etime - stime

@get_time
def gauss(n):
    return n * (n+1) // 2
```
get_time은 어떤 함수든 실행 시간을 측정해 실행된 함수의 결과값과 함께 반환한다. 즉 시간을 측정하는 함수이다.

gauss는 1부터 n까지 더한 값을 결과값으로 반환하는 함수이다. 두 함수 모두 하나의 특수한 기능만을 수행한다. gauss 함수가 없어도 get_time 함수는 독립적으로 존재하며 다른 함수와도 결합할 수 있다.

## 2. 피보나치를 데코레이터를 활용해 구현하기
### 2.1 기존 피보나치 알고리즘의 한계
n번째 피보나치 수를 찾는 함수는 다음과 같다.
```
f(n) = f(n-1) + f(n-2)
```

이것을 재귀호출 알고리즘으로 구현하면 다음과 같아진다.
```python

def fibonacci(n):
    if n in [0, 1]:
        return n
    return fibonacci(n-1) + fibonacci(n-2)
```
이 알고리즘의 문제는 트리 재귀호출 형태로 실행되면서 이미 찾은 값을 반복적으로 찾게 된다는 것이다. fibonacci(n-1)은 fibonacci(n-2) + fibonacci(n-3)이다. fibonacci(n-2)는 fibonacci(n-3) + fibonacci(n-4)이다. 계속 겹치는 값들이 등장함을 알 수 있다.

이로 인해 이 알고리즘의 시간복잡도는 O(2^n)이다. n이 커질수록 지수함수로 시간복잡도가 증가하는 것이다.

아래 함수도 크게 다르지 않다.
```python
def fibonacci(n):
    a, b = 1, 1
    i = 1
    while i < n:
        a, b = b, a+b
        i += 1
    return a
```
이 함수는 O(n)으로 실행된다. 계속 값을 더해나가는 방식으로 계산하기 때문이다.

다만 연속적으로 함수가 실행될 경우 O(n^2)이 되는 문제가 발생한다. 즉 위 함수가 n이 1부터 100만까지 변화하면서 100만번 실행된다면, 결국 O(n^2)이 된다는 의미다.

```python
for n in range(1, 10**6 +1):
    print(fibonacci(n))
```
100만번에 대해 O(n)이므로 결과는 O(n^2)이나 마찬가지다.

이를 더 효율적으로 바꿀 수 있는 방법은 cache를 이용해 실행 결과들을 저장하는 방법이다.

### 2.2 데코레이터와 캐시를 이용해 피보나치 알고리즘 개선하기
Codewars Algorithm URL: [https://www.codewars.com/kata/529adbf7533b761c560004e5](https://www.codewars.com/kata/529adbf7533b761c560004e5)

이 알고리즘은 위의 문제를 참고해 작성된 것이다.

```python
def memoized(f):
    cache = {}
    def wrapper(k):
        v = cache.get(k)
        if v is None:
            v = cache[k] = f(k)
        return v
    return wrapper

@memoized
def fibonacci(n):
    if n in [0, 1]:
        return n
    return fibonacci(n - 1) + fibonacci(n - 2)
```

이 알고리즘의 구조는 이렇다. fibonacci 수가 계산되면 memoized 내의 cache에 n과 fibonacci(n)이 key:value 형태로 저장된다. 그리도 새 fibonacci 함수가 실행될 때 만약 n이 cache에 있으면 계산을 하지 않고 cache[n]을 가져다 쓴다.

이렇게 되면 O(n^2)이 되고 마는 연속적인 fibonacci 함수 호출이 O(n)으로 줄어들게 된다.

```python
for n in range(1, 10**6):
    print(fibonacci(n))
```
이 경우 시간 복잡도는 10^12에서 10^6으로 낮아지게 된다.
