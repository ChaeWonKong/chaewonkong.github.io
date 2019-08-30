---
layout: post
title: "카운팅 정렬(계수정렬, Counting Sort) - 백준 알고리즘 10989번"
date: "2019-01-23"
categories:
- Algorithm
excerpt: |
  파이썬으로 카운팅 정렬(Counting Sort)를 구현해 본다. 또, 백준 알고리즘 10989번 "수 정렬하기 3"을 카운팅 정렬로 구현해 본다.
feature_text: |
  ## 파이썬 카운팅 정렬(계수정렬, Counting Sort) / 백준 알고리즘 10989번
  파이썬으로 카운팅 정렬(Counting Sort)를 구현해 본다. 또, 백준 알고리즘 10989번 "수 정렬하기 3"을 카운팅 정렬로 구현해 본다.
feature_image: "https://images.unsplash.com/photo-1509228468518-180dd4864904?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80"
image: "https://images.unsplash.com/photo-1509228468518-180dd4864904?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1500&q=80"
---
## 1. 카운팅 정렬(계수정렬, Counting Sort)
원소 간의 비교 없이 정렬을 진행할 수 있는 계수 정렬에 대해 알아본다. 
카운팅정렬(계수정렬)은 원소간 비교하지 않고 각 원소가 몇개 등장하는지 갯수를 세서 정렬하는 방법이다. 여기서 주어진 배열의 모든 원소는 양의 정수여야 한다. 시간복잡도는 O(n+k) 로 퀵정렬, 병합정렬에 비해 일반적으로 빠르다. k는 배열에서 가장 큰 정수이다(maximum). 정렬을 위한 길이 n의 배열 하나, 계수를 위한 길이 k의 배열 하나. O(n+k) 의 공간복잡도를 가진다.
<br><br>
### 1.1 카운팅 정렬이란
두 값을 반복적으로 비교해 정렬하는 기법인 comparison sort는 아무리 알고리즘을 잘 짜도 계산복잡성이 O(nlogn)보다 크다.

카운팅 정렬(counting sort)는 non-comparison sort 기법으로 정렬에 드는 계산복잡성을 O(n)으로 낮추려는 알고리즘 이다.<br><br>

### 1.2 카운팅 정렬의 작동 예
```python
A = [3, 0, 1, 3, 2, 1, 0, 2]
```
위와 같은 배열을 정렬한다고 하자. 카운팅 정렬을 위해서 먼저 A의 모든 요소값의 빈도를 추적할 배열 C를 선언한다.

```python
C = [0, 0, 0, 0]
```
C는 A의 각 요소들을 인덱스에 대응시키고 빈도를 그 인덱스의 요소로 저장한다. 0, 1, 2, 3이 각각 2번씩 등장하므로 C는 다음과 같아진다.

```python
C = [2, 2, 2, 2]
```

다음으로 C의 각 요소를 자신까지 포함하는 누적합으로 전환한다.
```python 
C = [2, 4, 6, 8]
```

배열 A의 역순으로 요소마다 위치를 찾고 이를 결과 값을 담을 배열 B에 담는다. 해당 자리가 채워지면 C에서 빈도를 -1 해준다. 여기서 배열의 인덱스는 0부터 시작하므로 C의 값을 대응시켜 B에 위치를 찾을 때는 -1을 해준다.

배열 A의 마지막 원소 2의 B에서의 위치 => C[2] - 1
```python
B[C[2]-1] = 2
```
참고로 배열 B는 배열 A와 길이가 같고 각 원소는 0으로 구성된 배열로 미리 선언해둔다.

2를 하나 배치했으므로, C에서 빈도 값을 하나 빼준다.
```python
C[2] -= 1
```

다음으로 배열 A의 마지막에서 두번째 원소인 0에 대해서 위의 작업을 반복한다.
```python
B[C[0]-1] = 0
C[0] -= 1
```

이런식으로 A의 배열을 끝에서부터 역순으로 하나하나 탐색하며 배열 B를 채워나가면 된다.
배열 B는 자연스럽게 A의 정렬된 모습으로 변경된다.<br><br>

### 1.3 전체 코드
```python

def counting_sort(A, k):
    """Return sorted array by using counting sort. k is maximum number in A"""

    N = len(A)
    C = [0] * (k+1)
    B = [0] * N

    for n in A:
        C[n] += 1
    for i in range(1, len(C)):
        C[i] += C[i-1]
    i = N -1
    while i >= 0:
        num = A[i]
        B[C[num]-1] = num
        C[num] -= 1
        i -= 1
    return B
```
<br><br>

## 2. 백준 알고리즘 10989번 - 수 정렬하기 3 풀이
### 2.1 문제 요약
> URL: https://www.acmicpc.net/problem/10989 <br>
> 시간 제한: 3초<br>
> 메모리 제한: 8 MB

N개의 수가 주어졌을 때, 이를 오름차순으로 정렬하는 프로그램을 작성하시오.

[입력]
첫째 줄에 수의 개수 N(1 ≤ N ≤ 10,000,000)이 주어진다. 둘째 줄부터 N개의 줄에는 숫자가 주어진다. 
이 수는 10,000보다 작거나 같은 자연수이다.

[출력]
첫째 줄부터 N개의 줄에 오름차순으로 정렬한 결과를 한 줄에 하나씩 출력한다.
<br><br>
## 2.2 풀이
먼저 알고리즘의 성능을 위해 sys 모듈을 불러온다. 처리해야할 입력의 줄 수가 최대 10,000,000까지 이므로 input()을 사용하지 않고 sys 모듈의 표준입출력을 사용할 것이다. 즉, sys.stdin.readline().rstrip()을 이용한다는 이야기다.

각 숫자는 10,000보다 작은 수이다.
따라서 상수 MAX를 10000으로 선언한다. MAX는 위에서 살펴본 알고리즘에서의 k와 같은 것으로 정렬한 리스트 내 요소 중 최댓값을 의미한다. 물론 실제로 주어지는 요소의 최댓값은 1만보다 훨씬 작을 수도 있다. 그러나 최댓값이 얼마인지를 알 수 있는 방법은 없다. 따라서 가능한 최댓값 중 가장 큰 값인 1만을 최댓값으로 산정하고 카운팅 정렬을 사용해 정렬을 진행하는 것이다.

이러한 점들을 참고해 입력을 받고 출력을 진행하는 부분의 코드를 작성해보면 다음과 같다.
```python
import sys
MAX = 10000

def counting_sort(N):
    # 구현할 코드
    return cache

if __name__ == "__main__":
    N = int(sys.stdin.readline().rstrip())
    cache = counting_sort(N)
    for i in range(MAX):
        for _ in range(cache[i]):
            sys.stdout.write(str(i) + '\n')
```
먼저 총 테스트 케이스의 개수를 T에 저장한다. 카운팅 정렬을 통해 빈도를 저장한 배열이 cache이다. 원래 카운팅 정렬이라면 정렬된 배열을 반환해야 하지만, 여기서는 정렬된 순서대로 원소를 한 줄에 하나씩 출력하기만 하면 되므로 새 배열에 정렬을 진행하지는 않는다.

여기에는 이유가 있다. cache의 길이는 10,0001이지만, 정렬할 배열과 정렬된 결과 배열의 길이는 최대 10,000,000이다. 문제에 주어진 시간제한과 메모리제한을 준수하기 위해서는 이렇게 큰 배열을 생성하는 것은 무리다. 따라서 배열을 생성하지 않고 해결하고자 하는 것이다.

중첩된 for문의 내용은 다음과 같다. cache의 인덱스는 정렬할 배열의 요소를 의미하며 cache의 요소는 해당 인덱스와 같은 숫자가 정렬할 배열에서 등장하는 빈도수를 의미한다. 즉 cache는 일반적인 카운팅 정렬과는 다르게 누적합 형태의 빈도수를 담는 것이 아니라, 각 인덱스의 단순 빈도수를 담고 있다. 정렬할 배열에 1이 3개 2가 6개라면 cache[1] = 3, cache[2] = 6이라는 말이다. 이를 바탕으로 중첩된 for문은 0부터 max까지 cache에 저장된 빈도의 횟수만큼 출력을 진행해준다.

다음으로 counting_sort()를 구현해본다. counting_sort는 테스트 케이스의 숫자를 받아 필요한 만큼의 입력을 받고 이를 처리해 cache를 반환하는 함수다.

```python
def counting_sort(N):
    cache = [0] * (MAX+1)
    for _ in range(N):
        num = int(sys.stdin.readline().rstrip())
        cache[num] += 1   
    return cache
```
먼저 MAX+1개의 0으로 이루어진 배열을 선언하고 이를 cache에 할당한다. 여기서 MAX개가 아니라 MAX+1개인 이유는 배열의 인덱스가 정렬할 배열의 각 요소에 대응되게 하기 위함이다. 최댓값이 MAX이므로 MAX에 해당하는 인덱스를 가지기 위해서는 길이가 MAX+1이어야 하기 때문이다.

다음으로 N번의 입력을 sys 모듈의 입력을 이용해 받고 각 입력에 대해 cache에서 해당 입력 요소의 인덱스에 해당하는 값을 1씩 올려준다. 즉, 입력으로 3이 들어왔다면 cache의 3번째에 1을 더하는 식이다. 이렇게 cache에는 인덱스의 빈도가 저장되게 된다.
<br><br>
## 2.3 전체코드
```python
import sys
MAX = 10000

def counting_sort(N):
    cache = [0] * (MAX+1)
    for _ in range(N):
        num = int(sys.stdin.readline().rstrip())
        cache[num] += 1   
    return cache

if __name__ == "__main__":
    N = int(sys.stdin.readline().rstrip())
    cache = counting_sort(N)
    for i in range(MAX):
        for _ in range(cache[i]):
            sys.stdout.write(str(i) + '\n')
```
<br><br>
## 참고자료 및 출처
[ratsgo's blog - 카운팅 정렬, 래딕스 정렬](https://ratsgo.github.io/data%20structure&algorithm/2017/10/16/countingsort/)

[YABOONG - Counting Sort - 계수정렬](https://yaboong.github.io/algorithms/2018/03/20/counting-sort/)