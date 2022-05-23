---
layout: post
title: "[Java] 2019 카카오 겨울 인턴쉽 코딩테스트 - 튜플"
date: "2022-05-22"
categories:
  - Algorithm
excerpt: |
  문자열 파싱, 커스텀 comparator

feature_text: |
  ## [Java] 2019 카카오 겨울 인턴쉽 코딩테스트 - 튜플
  문자열 파싱, 커스텀 comparator
feature_image: "https://images.unsplash.com/photo-1543966888-7c1dc482a810?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1506&q=80"
image: "https://images.unsplash.com/photo-1543966888-7c1dc482a810?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1506&q=80"
---

[문제링크: https://programmers.co.kr/learn/courses/30/lessons/64065](https://programmers.co.kr/learn/courses/30/lessons/64065)

## [문제내용]

셀수있는 수량의 순서있는 열거 또는 어떤 순서를 따르는 요소들의 모음을 튜플(tuple)이라고 합니다. n개의 요소를 가진 튜플을 n-튜플(n-tuple)이라고 하며, 다음과 같이 표현할 수 있습니다.

(a1, a2, a3, ..., an)
튜플은 다음과 같은 성질을 가지고 있습니다.

중복된 원소가 있을 수 있습니다. ex : (2, 3, 1, 2)
원소에 정해진 순서가 있으며, 원소의 순서가 다르면 서로 다른 튜플입니다. ex : (1, 2, 3) ≠ (1, 3, 2)
튜플의 원소 개수는 유한합니다.
원소의 개수가 n개이고, 중복되는 원소가 없는 튜플 (a1, a2, a3, ..., an)이 주어질 때(단, a1, a2, ..., an은 자연수), 이는 다음과 같이 집합 기호 '{', '}'를 이용해 표현할 수 있습니다.

{% raw %}
`{{a1}, {a1, a2}, {a1, a2, a3}, {a1, a2, a3, a4}, ... {a1, a2, a3, a4, ..., an}}`
예를 들어 튜플이 (2, 1, 3, 4)인 경우 이는

`{{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}`
와 같이 표현할 수 있습니다. 이때, 집합은 원소의 순서가 바뀌어도 상관없으므로

`{{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}}`
`{{2, 1, 3, 4}, {2}, {2, 1, 3}, {2, 1}}`
`{{1, 2, 3}, {2, 1}, {1, 2, 4, 3}, {2}}`
{% endraw %}
는 모두 같은 튜플 (2, 1, 3, 4)를 나타냅니다.

특정 튜플을 표현하는 집합이 담긴 문자열 s가 매개변수로 주어질 때, s가 표현하는 튜플을 배열에 담아 return 하도록 solution 함수를 완성해주세요.

## [제한사항]

s의 길이는 5 이상 1,000,000 이하입니다.
s는 숫자와 '{', '}', ',' 로만 이루어져 있습니다.
숫자가 0으로 시작하는 경우는 없습니다.
s는 항상 중복되는 원소가 없는 튜플을 올바르게 표현하고 있습니다.
s가 표현하는 튜플의 원소는 1 이상 100,000 이하인 자연수입니다.
return 하는 배열의 길이가 1 이상 500 이하인 경우만 입력으로 주어집니다.

{% raw %}

## [입출력 예]

| s                               | result       |
| :------------------------------ | :----------- |
| "{{2},{2,1},{2,1,3},{2,1,3,4}}" | [2, 1, 3, 4] |
| "{{1,2,3},{2,1},{1,2,4,3},{2}}" | [2, 1, 3, 4] |
| "{{20,111},{111}}"              | [111, 20]    |
| "{{123}}"                       | [123]        |
| "{{4,2,3},{3},{2,3,4,1},{2,3}}" | [3, 2, 4, 1] |

## 풀이

문자열 파싱과 정렬을 커스터마이징 하는 능력을 보는 문제였다.

"{2}, {2, 1}, {2, 1, 3}, {2, 1, 3, 4}"가 주어지면 "{2, 1, 3, 4}"를 리턴하면 된다.
즉, 부분집합을 통해 전체집합을 유추하는 것인데,
단, 순서가 있어야 한다. 즉, {1, 2, 3, 4}를 리턴하면 안되고, 꼭 {2, 1, 3, 4}를 리턴해야 한다.

"{4,2,3},{3},{2,3,4,1},{2,3}" 사례를 보자.
[3, 2, 4, 1]이 정답이다. 이 순서의 규칙이 뭘까?

우선 가장 작은 배열부터 다시 나열해보면 다음과 같다.
"{3}, {2, 3}, {4, 2, 3}, {2, 3, 4, 1}"

위 순서 그대로 나열하되, 중복되지 않은 숫자만 볼드 처리하면 다음과 같다.
"{**3**}, {**2**, 3}, {**4**, 2, 3}, {2, 3, 4, **1**}"

이 볼드처리된 숫자들만 뽑아보면 {3, 2, 4, 1}로 답이 된다.
{% endraw %}

즉, 정리하면 이렇다.

1. 가장 작은 배열부터 순서대로 정렬하고, (이 문제의 경우 String의 길이로 정렬할 예정이다)
2. 작은 배열 순서대로 그 배열 내부의 원소를 결과배열에 담는데,
3. 결과배열에 담으려는 원소가 있다면 패스하고 없다면 담는다.
4. 결과배열은 담는 순서가 보장된다.

이 조건들을 만족하도록 알고리즘을 짜면 된다.

### 자료구조

튜플이라는 문제 이름에서 알 수 있듯 중복되는 원소는 없다. Java에서 보통 중복되는 원소가 없는 것이 보장되는 자료구조로는 `Set`을 떠올릴 수 있다.
하지만, 가장 일반적으로 사용되는 `HashSet`의 경우 모든 원소가 중복되지 않는 것이 보장되나, 원소의 순서가 일정할 것은 보장되지 않는다.

결국 `LinkedHashSet`을 사용하면 1) 원소가 중복되지 않고, 2) 순서가 고정적으로 보장되는데, 이 문제에 가장 적합하다고 할 수 있다.

### 문자열 파싱

다음으로 문자열 파싱이다.
먼저 정렬하기 직전에 부분집합 단위로 쪼갠 문자열을 담은 배열을 만든다.
{% raw %}

```java
String[] arr = s.substring(2, s.length() - 2).split("\\}\\,\\{");
```

`s.substring(2, s.length() - 2)`
먼저 앞의 "{{" 부분과 뒤의 "}}" 부분은 필요 없으므로 잘라낸다.

`.split("\\}\\,\\{")`
다음으로 중간에 있는 "},{"를 기준으로 문자열 배열로 쪼개준다.
{% endraw %}

### 정렬

이제 정렬을 하면 된다. `Arrays.sort`를 이용하되 Comparator를 만들어서 짧은 문자열부터(짧은 문자열이면 원소수가 적은 집합이다) 정렬한다.

```java
Arrays.sort(arr, new Comparator<String>() {
   @Override
   public int compare(String o1, String o2) {
       return o1.length() - o2.length();
   }
});
```

new 연산자로 `Comparator<String>`을 만들어주고 `compare` 메서드를 오버라이딩 한다. 길이 비교를 통해 정렬되게 한다.

### 집합에 숫자 담기

```java
for (String subSet : arr) {
   String[] subSetArr = subSet.split(",");
   for (String numString: subSetArr) {
       int val = Integer.parseInt(numString);
       set.add(val);
   }
}
```

`String[] subSetArr = subSet.split(",");`
String 배열을 순회하며 각 String에 대해 다시 `,`(콤마)를 기준으로 배열로 쪼갠다.
문자열로 된 부분집합의 각 원소를 순회하며 이를 int형으로 바꿔 `LinkedHashSet`에 넣어줄 것이다.

```java
for (String numString: subSetArr) {
    int val = Integer.parseInt(numString);
    set.add(val);
}
```

각 원소를 int로 바꿔 `LinkedHashSet`에 추가한다.

## 전체 풀이 코드

{% raw %}

```java
import java.util.*;

/** 다시 보면 좋을 문제
 * 문자열 파싱과 커스터마이징된 정렬 알고리즘
 */

public class Tuple {
    public int[] solution(String s) {
        String[] arr = s.substring(2, s.length() - 2).split("\\}\\,\\{");
        LinkedHashSet<Integer> set = new LinkedHashSet<>();

        Arrays.sort(arr, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o1.length() - o2.length();
            }
        });

        for (String subSet : arr) {
            String[] subSetArr = subSet.split(",");
            for (String numString: subSetArr) {
                int val = Integer.parseInt(numString);
                set.add(val);
            }
        }

        return set.stream().mapToInt(Integer::intValue).toArray();
    }
}
```

{% endraw %}

## 마치며

오랜만에 알고리즘을 다시 잡았더니 많은 것들이 리셋된 느낌이다.
문제풀이라는게 사실 풀다보면 느는 것 같다.
꾸준히 차근차근 해 나가자.
