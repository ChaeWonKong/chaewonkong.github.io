---
layout: post
title: "Python - 데크(deque) 언제, 왜 사용해야 하는가?"
date: "2019-10-28"
categories:
  - python
excerpt: |
  Python의 데크(deque)에 대해 알아보고 언제, 왜 써야 하는지 살펴본다
feature_text: |
  ## Python - 데크(deque) 언제, 왜 사용해야 하는가?
  Python의 데크(deque)에 대해 알아보고 언제, 왜 써야 하는지 살펴본다
feature_image: "https://images.unsplash.com/photo-1500672860114-9e913f298978?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1949&q=80"
image: "https://images.unsplash.com/photo-1500672860114-9e913f298978?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1949&q=80"
---

오늘은 Python의 데크(deque)라는 자료구조에 대해 알아보고, 어떤 상황에서 사용해야 하는지에 대해 살펴본다.

## 데크(deque)의 개념

보통 큐(queue)는 선입선출(FIFO) 방식으로 작동한다. 반면, **양방향 큐**가 있는데 그것이 바로 **데크(deque)**다.

즉, **앞, 뒤 양쪽 방향에서 엘리먼트(element)를 추가하거나 제거할 수 있다.**

데크는 양 끝 엘리먼트의 append와 pop이 압도적으로 빠르다.

컨테이너(container)의 양끝 엘리먼트(element)에 접근하여 삽입 또는 제거를 할 경우, **일반적인 리스트(list)가 이러한 연산에 O(n)이 소요되는 데 반해, 데크(deque)는 O(1)**로 접근 가능하다.

## 데크(deque) 사용법

데크는 다음처럼 임포트(import)해 사용한다.

```python
from collections import deque

deq = deque()

# Add element to the start
deq.appendleft(10)

# Add element to the end
deq.append(0)

# Pop element from the start
deq.popleft()

# Pop element from the end
deq.pop()
```

데크(deque)에 존재하는 메서드(method)는 대략 다음과 같다.

- `deque.append(item)`: item을 데크의 오른쪽 끝에 삽입한다.
- `deque.appendleft(item)`: item을 데크의 왼쪽 끝에 삽입한다.
- `deque.pop()`: 데크의 오른쪽 끝 엘리먼트를 가져오는 동시에 데크에서 삭제한다.
- `deque.popleft()`: 데크의 왼쪽 끝 엘리먼트를 가져오는 동시에 데크에서 삭제한다.
- `deque.extend(array)`: 주어진 배열(array)을 순환하면서 데크의 오른쪽에 추가한다.
- `deque.extendleft(array)`: 주어진 배열(array)을 순환하면서 데크의 왼쪽에 추가한다.
- `deque.remove(item)`: item을 데크에서 찾아 삭제한다.
- `deque.rotate(num)`: 데크를 num만큼 회전한다(양수면 오른쪽, 음수면 왼쪽).

여기서 rotate() 메서드(method)가 특히 재밌는데, 설명이 부족하니 코드를 추가해 보겠다.

```python

# Contain 1, 2, 3, 4, 5 in deq
deq = deque([1, 2, 3, 4, 5])

deq.rotate(1)
print(deq)
# deque([5, 1, 2, 3, 4])

deq.rotate(-1)
print(deq)
# deque([1, 2, 3, 4, 5])
```

이렇게 양수 값 또는 음수 값을 파라미터(parameter)로 제공하여 데크(deque)를 좌, 우로 회전할 수 있다.

## 데크(deque), 언제, 왜 써야 하는가?

요약하자면, 데크(deque)는 스택처럼 사용할 수도 있고, 큐 처럼 사용할 수도 있다.

시작점의 값을 넣고 빼거나, 끝 점의 값을 넣고 빼는 데 최적화된 연산 속도를 제공한다.

즉, 대부분의 경우에 데크(deque)는 리스트(list)보다 월등한 옵션이다.

데크는 특히 push/pop 연산이 빈번한 알고리즘에서 리스트보다 월등한 속도를 자랑한다.

일례로 백준 7576번 "토마토" 문제에서 익은 토마토를 리스트에 담도록 코드를 짜면 시간초과로 통과에 실패하지만, 데크를 사용하면 무난히 통과한다.

마지막으로 `백준 토마토(7576) 문제의 Python 코드`를 공유하며 데크 사용법을 마친다.

```python
from collections import deque


def solution(m, n, tomatoes):
    count = 0  # Count number of days
    deq = deque()
    # deq = list()
    D = [(0, 1), (1, 0), (0, -1), (-1, 0)]

    def search(row, col):
        searched_list = []

        for i, j in D:
            if (row + i < N and col + j < M) and (row + i >= 0 and col + j >= 0):
                if tomatoes[row + i][col + j] == 0:
                    tomatoes[row + i][col + j] = 1
                    searched_list.append((row + i, col + j))

        return searched_list

    # Add all riped tomatoes
    for r in range(N):
        for c in range(M):
            if tomatoes[r][c] == 1:
                deq.append((r, c))

    # Search begin
    while deq:
        for _ in range(len(deq)):
            r, c = deq.popleft()
            for tomato in search(r, c):
                deq.append(tomato)
        count += 1

    # Check unriped tomato(es) after search
    for r in range(N):
        for c in range(M):
            if tomatoes[r][c] == 0:
                return -1

    return count - 1


if __name__ == "__main__":
    M, N = map(int, input().split(" "))
    tomatoes = [[int(n) for n in input().split(" ")] for _ in range(N)]
    print(solution(M, N, tomatoes))

```

## 참고자료

- [GeeksforGeeks - Deque in Python](https://www.geeksforgeeks.org/deque-in-python/)
- [GOD.OF.TYPING - [python]데크(deque)](https://godoftyping.wordpress.com/2015/04/24/python-%EB%8D%B0%ED%81%ACdeque/)
- [백준 7576번 - 토마토](https://www.acmicpc.net/problem/7576)
