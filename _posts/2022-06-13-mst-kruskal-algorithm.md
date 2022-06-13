---
layout: post
title: "[Java] MST와 크루스칼 알고리즘을 활용한 백준 1922번 풀이"
date: "2022-06-13"
categories:
  - Algorithm
excerpt: |
  백준 1922번 문제를 MST(최소 스피닝 트리), 크루스칼 알고리즘(Kruskal Algorithm)을 이용해 Java(자바)로 풀어본다.
feature_text: |
  ## [Java] MST와 크루스칼 알고리즘을 활용한 백준 1922번 풀이
  백준 1922번 문제를 MST(최소 스피닝 트리), 크루스칼 알고리즘(Kruskal Algorithm)을 이용해 Java(자바)로 풀어본다.
feature_image: "https://images.unsplash.com/photo-1543966888-7c1dc482a810?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1506&q=80"
image: "https://images.unsplash.com/photo-1543966888-7c1dc482a810?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1506&q=80"
---

## 문제 소스

[1922번: 네트워크 연결](https://www.acmicpc.net/problem/1922)

## 풀이

크루스칼 알고리즘을 이용해서 풀 수 있는 문제다.

크루스칼 알고리즘은 다음과 같은 아이디어에서 착안했다.

### 크루스칼 알고리즘

> 섬이 7개가 있다고 하자. 이 섬들을 가장 적은 비용을 들여 다리를 건설해 모두 연결하는 방법은 무엇일까?

우선, 조금만 생각해보면 총 6개의 다리만 연결하면 무조건 7개의 섬을 연결할 수 있음을 알 수 있다.

만약 연결할 수 없는 섬을 제외하고 임의의 섬 A, B를 연결하는 비용이 주어졌을 때, 최소비용을 구하는 간단한 방법이 바로 크루스칼 알고리즘이다.

_“가장 다리 건설비용이 적게 드는 섬부터 일단 연결해보면 어떨까?”_
이 아이디어는 `사이클을 만들지 않는다면`이라는 조건만 만족하면 실제로 효율적이고 우아하게 잘 동작한다.

A, B, C 섬이 있다고 하자. A와 B, A와 C를 연결했다면, 사실상 B와 C는 연결된 셈이다. A를 통해 이동할 수 있으니 말이다.

여기서 만약 한술 더 떠 B, C를 다리로 연결한다면, 섬 주민들은 편해지겠지만 다리를 한번 덜 건설해도 될 것을 더 건설했으니, 그만큼 비용은 낭비가 된다.

이게 바로 사이클이다. A, B, C를 3개의 다리로 각각 연결하는 순간 사이클이 생기고 낭비가 생긴다.

요약하자면,
크루스칼 알고리즘은

1. 비용이 작은 간선부터 연결하되
2. 낭비가 없도록 사이클을 만들지 않으면서 연결하는

알고리즘이다.

### 정점과 간선

정점과 간선은 `Edge`라는 클래스를 선언해 관리한다.

여기서 `Comparable`을 구현하였는데, 앞서 말했듯 “비용이 작은 간선부터 연결”을 만족하기 위해서는 간선의 비용을 기준으로 정렬해야 하기 때문에 구현하는 것이다.

```java
private static class Edge implements Comparable<Edge> {
    int start;
    int end;
    int weight;

    public Edge(int start, int end, int weight) {
        this.start = start;
        this.end = end;
        this.weight = weight;
    }

    @Override
    public int compareTo(Edge o) {
        return weight - o.weight;
    }
}
```

### union-find

union메서드와 find메서드를 구현한다.
여기서는 편의를 위해 find 메서드를 `getParent`라고 부르고, 두 정점이 하나의 부모를 공유하는지 확인하는 `hasSameParent` 메서드를 하나 더 추가했다.

```java
	  // 자식으로부터 부모를 가져옴
    private static int getParent(int x) {
        if (parent[x] == x) {
            return x;
        }

        parent[x] = getParent(parent[x]);
        return parent[x];
    }

    // 두 부모를 합치는 함수. 더 작은 부모로 합친다.
    private static void union(int x, int y) {
        x = getParent(x);
        y = getParent(y);

        if (x < y) {
            parent[y] = x;
        } else {
            parent[x] = y;
        }
    }

    // 같은 부모를 가지는지 확인
    private static boolean hasSameParent(int x, int y) {
        x = getParent(x);
        y = getParent(y);
        return x == y;
    }
```

### 기타 로직

사실 제일 중요한 부분은 바로 이 부분이다.

1. 최소비용 순으로 정렬된 네트워크 배열을 배열의 시작부터 차례대로 조회하며 (가장 비용이 작은 것부터 순차적으로)
2. 공통의 부모가 없다면 (연결하더라도 사이클을 형성하지 않는다면)
3. 연결하고, 비용을 가산한다.

`ans`는 총 비용을 담는 변수이다.

`hasSameParent` 메서드를 통해 같은 조상을 가지는지 확인할 수 있다.
`union` 메서드를 통해 두 정점을 연결하고 조상을 `parent` 배열에 기록한다.

```java
        for (int i = 0; i < M; i++) {
            Edge edge = network[i];

            if (!hasSameParent( edge.start, edge.end)) { // 사이클이 발생하지 않는다면
                // 간선을 연결하고 비용을 가산
                union(edge.start, edge.end);
                ans += edge.weight;
            }
        }
```

### 전체코드

전체 코드는 다음과 같다.

```java
import java.io.BufferedReader;
import java.io.IOException;
import java.io.InputStreamReader;
import java.util.Arrays;
import java.util.StringTokenizer;

public class ConnectNetwork {
    private static int[] parent;
    public static void main(String[] args) throws IOException {
        BufferedReader br = new BufferedReader(new InputStreamReader(System.in));
        StringTokenizer st;
        int ans = 0;

        int N = Integer.parseInt(br.readLine());
        int M = Integer.parseInt(br.readLine());

        Edge[] network = new Edge[M];
        parent = new int[N + 1];

        // parent 배열을 초기화
        for (int i = 0; i <= N; i++) {
            parent[i] = i;
        }

        // 정점과 간선 입력을 network에 저장
        for (int i = 0; i < M; i++) {
            st = new StringTokenizer(br.readLine());
            int start = Integer.parseInt(st.nextToken());
            int end = Integer.parseInt(st.nextToken());
            int weight = Integer.parseInt(st.nextToken());

            network[i] = new Edge(start, end, weight);
        }

        br.close();

		  // 최소비용 순으로 network 배열을 정렬
        Arrays.sort(network);
        for (int i = 0; i < M; i++) {
            Edge edge = network[i];

            if (!hasSameParent( edge.start, edge.end)) { // 사이클이 발생하지 않는다면
                // 간선을 연결하고 비용을 가산
                union(edge.start, edge.end);
                ans += edge.weight;
            }
        }

        System.out.println(ans);
    }

    private static class Edge implements Comparable<Edge> {
        int start;
        int end;
        int weight;

        public Edge(int start, int end, int weight) {
            this.start = start;
            this.end = end;
            this.weight = weight;
        }

        @Override
        public int compareTo(Edge o) {
            return weight - o.weight;
        }
    }

	  // 자식으로부터 부모를 가져옴
    private static int getParent(int x) {
        if (parent[x] == x) {
            return x;
        }

        parent[x] = getParent(parent[x]);
        return parent[x];
    }

    // 두 부모를 합치는 함수. 더 작은 부모로 합친다.
    private static void union(int x, int y) {
        x = getParent(x);
        y = getParent(y);

        if (x < y) {
            parent[y] = x;
        } else {
            parent[x] = y;
        }
    }

    // 같은 부모를 가지는지 확인
    private static boolean hasSameParent(int x, int y) {
        x = getParent(x);
        y = getParent(y);
        return x == y;
    }
}
```

## 예제 입출력

```
7
9
1 2 29
1 5 75
2 3 35
2 6 34
3 4 7
4 6 23
4 7 13
5 6 53
6 7 25

// out: 159
```

## 참고자료

코드도 읽어보고 유튜브도 봤지만, 이해가 안 되는 사람에게는 유튜브가 제일 이해하기 쉽다. 직접 그래프를 그리고 간선을 연결해 가며 설명해 주셔서 이해가 빨랐다.

- [union-find 알고리즘 - YouTube 동빈나](https://youtu.be/LQ3JHknGy8c)
- [크루스칼 알고리즘 - YouTube 동빈나](https://www.youtube.com/watch?v=AMByrd53PHM)

이외에 단순히 코드만 확인할 목적이라면 아래 자료를 추천한다.

- [느리더라도 꾸준하게 :: 백준 1922번: 네트워크 연결(Java)](https://steady-coding.tistory.com/115)
- [브랜든의 블로그 :: 알고리즘 크루스칼 알고리즘(Kruskal Algorithm)](https://brenden.tistory.com/36)
