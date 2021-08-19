---
layout: post
title: "[TIL] Unity를 위한 C# - 변수(variables)"
date: "2021-08-20"
categories:
  - TIL
excerpt: |
  Unity를 사용하기 위해 기초적으로 알아야할 C#을 공부해본다. 오늘은 변수의 타입에 대해 알아본다.
feature_text: |
  ## Unity를 위한 C# - 변수(variables)
  Unity를 사용하기 위해 기초적으로 알아야할 C#을 공부해본다. 오늘은 변수의 타입에 대해 알아본다.
feature_image: "https://images.unsplash.com/photo-1531525645387-7f14be1bdbbd?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1400&q=80"
image: "https://images.unsplash.com/photo-1531525645387-7f14be1bdbbd?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1400&q=80"
---

오늘은 Unity 개발을 위한 C# 문법을 조금 익혔다. 큰 틀에서 다른 프로그래밍 언어들과 크게 다르지 않았다.

```c#
// C#의 다양한 변수들
int hitPoints = 20;
float speed = 3.8f; // float일 경우 f를 끝에 붙여준다.
bool isAlive = true;
string name = "Leon";

// 다음은 가능하다.
int max = 99999;
Debug.Log("The highest number is: " + max);
// 정수타입의 변수 max에 담긴 값이 string으로 변환되어 하나의 합쳐진 string으로 console에 로깅되게 된다.
```

기초적인 Unity course다 보니 C#을 깊이 있게 다루지는 않는다. 나 역시 Unity가 무엇인지, 게임 개발은 어떤 즐거움이 있을지 알아본다는 개념으로 공부하고 있다 보니, 깊이 있게 파보지는 않았다.
