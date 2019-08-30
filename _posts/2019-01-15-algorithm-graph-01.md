---
layout: post
title: "그래프를 활용한 알고리즘 01 - PICNIC"
date: "2019-01-15"
categories:
- Algorithm
excerpt: |
  파이썬으로 그래프를 활용하여 알고스팟의 PICNIC 문제를 해결해본다.
feature_text: |
  ## 그래프를 활용한 알고리즘 01 - PICNIC
  파이썬으로 그래프를 활용하여 알고스팟의 PICNIC 문제를 해결해본다.
feature_image: "https://images.unsplash.com/photo-1491895200222-0fc4a4c35e18?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1567&q=80"
image: "https://images.unsplash.com/photo-1491895200222-0fc4a4c35e18?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1567&q=80"
---

문제 출처: [알고스팟](https://algospot.com) <br>
URL: [https://algospot.com/judge/problem/read/PICNIC](https://algospot.com/judge/problem/read/PICNIC)
<br>
ID: PICNIC

---
## 소풍 (PICNIC)
서로 친구 사이인 학생 끼리만 짝을 지어줄 수 있는 모든 방법의 수를 구한다.

### 입력
입력의 첫 줄에는 테스트 케이스의 수 C (C <= 50) 가 주어진다. 각 테스트 케이스의 첫 줄에는 학생의 수 n (2 <= n <= 10) 과 친구 쌍의 수 m (0 <= m <= n*(n-1)/2) 이 주어진다. 그 다음 줄에 m 개의 정수 쌍으로 서로 친구인 두 학생의 번호가 주어진다. 번호는 모두 0 부터 n-1 사이의 정수이고, 같은 쌍은 입력에 두 번 주어지지 않는다. 학생들의 수는 짝수이다.

### 출력
각 테스트 케이스마다 한 줄에 모든 학생을 친구끼리만 짝지어줄 수 있는 방법의 수를 출력한다.

### 전체 코드
```python
def numbers_of_cases(are_friends):
    N = len(are_friends)
    taken = [0] * N

    def find(left):
        count = 0
        if left == 0:
            return 1
        for i in range(N):
            if not taken[i]:
                nxt = i
                break
        for j in range(nxt+1, N):
            if not taken[j] and are_friends[nxt][j]:
                taken[j] = taken[nxt] = 1
                count += find(left-2)
                taken[j] = taken[nxt] = 0
        return count
    
    return find(N)


if __name__ == "__main__":
    # Three Example Test Cases
    T = int(input())
    results = []
    for _ in range(T):
        n, m = [int(i) for i in input().split()]
        temp = [int(i) for i in input().split()]
        are_friends = [[0]*(n) for _ in range(n)]
        idx = 0
        while idx < len(temp):
            are_friends[temp[idx]][temp[idx+1]] = are_friends[temp[idx+1]][temp[idx]] = 1
            idx += 2
        results.append(numbers_of_cases(are_friends))
    
    for ret in results:
        print(ret)
```

### 풀이
학생을 Vertex, 친구 사이인지 여부를 Edge로 보면 이 문제는 그래프를 활용해 풀 수 있는 문제임을 알 수 있다.

먼저 입력으로 주어지는 정보를 처리하는 코드를 작성한다.
```python
if __name__ == "__main__":
    # Three Example Test Cases
    T = int(input())
    results = []
    for _ in range(T):
        n, m = map(int, input().split())
        temp = [int(i) for i in input().split()]
        are_friends = [[0]*(n) for _ in range(n)]
        idx = 0
        while idx < len(temp):
            are_friends[temp[idx]][temp[idx+1]] = are_friends[temp[idx+1]][temp[idx]] = 1
            idx += 2
        results.append(numbers_of_cases(are_friends))
    
    for ret in results:
        print(ret)
```
테스트 케이스의 수 T에 대해 각 테스트마다 n, m을 입력받는다. 이후 친구들끼리 짝지어진 입력을 리스트로 변환하여 temp에 저장한다. are_friends는 n * n으로 구성된 2차원 배열로 두 정점이 서로 친구인지 여부를 기록할 인접행렬이다(adjacent matrix).

idx를 0으로 선언한 후 , idx가 temp의 길이보다 작은 경우 while문을 실행시켜 인접행렬 are_friends를 채워나간다. while문을 실행하면 idx는 2씩 커지며 temp의 원소를 하나 걸러 하나씩 순회하게 되고, are_friends에 친구 여부가 표시되게 된다.

이후 results라는 결과를 담을 리스트에 추후 작성할 함수를 호출하고 그 결과를 담는다. 해당 함수 numbers_of_cases에 are_friends를 매개변수로 준다.

실행이 완료된 경우 결과값을 하나씩 출력해주는 작업을 진행하면 된다.

다음으로 numbers_of_cases 함수를 구현하는 코드를 살펴본다.
```python
def numbers_of_cases(are_friends):
    N = len(are_friends)
    taken = [0] * N

    def find(left):
        count = 0
        if left == 0:
            return 1
        for i in range(N):
            if not taken[i]:
                nxt = i
                break
        for j in range(nxt+1, N):
            if not taken[j] and are_friends[nxt][j]:
                taken[j] = taken[nxt] = 1
                count += find(left-2)
                taken[j] = taken[nxt] = 0
        return count
    
    return find(N)
```

numbers_of_cases 함수는 인접행렬인 are_friends를 매개변수로 받는다. 먼저 함수 내에서 are_friends의 길이를 N에 저장하고, taken이라는 N개의 0으로 이루어진 배열(리스트)를 생성한다. taken은 짝을 지어진 번호들을 tracking하기 위해 생성하는 리스트로, 짝이 지어진 원소에 해당하는 인덱스는 1로 바꿔줄 것이다.

각 정점(vertex)를 순회하며(정점을 선택하며) 짝인 경우만 뽑는 경우의 수를 찾아야 한다. 따라서 이 문제는 DFS를 활용하며, 재귀호출을 사용해 풀기로 한다. 참고로 재귀호출은 Stack 개념과 관련이 깊다.

find 함수를 보자. 이 함수는 경우의 수를 세는 함수이다. 매개변수로는 짝이 지어지고 남은 학생의 수가 들어간다. 만약 남은 학생의 수가 0이라면 짝이 모두 지어졌으므로 1을 반환하고 종료한다. 
```python
if left == 0:
    return 1
```

전체 학생 수에 대해 만약 i가 뽑히지 않았다면 nxt에 i를 할당한다.
```python
for i in range(N):
    if not taken[i]:
        nxt = i
        break
```
nxt를 선택한 상황에서 나머지 하나를 뽑는 방법이므로 nxt+1부터 N 사이에서 j를 선택한다. 이 중 j가 뽑히지 않았고, j와 nxt가 친구라면 짝을 지어준다.
짝은 2명씩 지어지므로 짝을 하나 완성하였으면 left-2를 한 값을 find함수로 재귀호출 진행한다. 재귀호출이 종료된 후에는 taken을 초기화 해준다.
```python
for j in range(nxt+1, N):
    if not taken[j] and are_friends[nxt][j]:
        taken[j] = taken[nxt] = 1
        count += find(left-2)
        taken[j] = taken[nxt] = 0
```
한편 find(left-2)를 재귀호출로 진행한 결과값은 count에 더해나가도록 설정한다.

모든 작업이 끝났으면 count를 반환하고 종료한다.