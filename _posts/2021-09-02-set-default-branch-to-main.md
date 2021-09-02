---
layout: post
title: "시대에 맞게 Git의 default 브랜치명을 main으로 설정하자"
date: "2021-09-02"
categories:
  - DevLog
excerpt: |
  git의 default 브랜치를 master에서 main으로 바꿔보자. 그리고, 세상을 더 나은 곳으로 바꾸는 일에 동참하자. git에서 branch 이름을 변경하는 법을 알아본다. remote도 이에 맞게 설정해주는 법을 알아본다. 더불어 git의 전역 설정을 통해 default branch가 main이 되게 설정해본다.
feature_text: |
  ## cron 표현식 정리
  git의 default 브랜치를 master에서 main으로 바꿔보자. 그리고, 세상을 더 나은 곳으로 바꾸는 일에 동참하자. git에서 branch 이름을 변경하는 법을 알아본다. remote도 이에 맞게 설정해주는 법을 알아본다. 더불어 git의 전역 설정을 통해 default branch가 main이 되게 설정해본다.
feature_image: "https://images.unsplash.com/photo-1606352466047-7cef02b312bb?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1062&q=80"
image: "https://images.unsplash.com/photo-1606352466047-7cef02b312bb?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1062&q=80"
---

BLM(Black Lives matter) 운동과 "Stop Asian Hate" 운동을 통해 **전세계적으로 인종, 성별, 종교, 문화, 소수자 등에 대한 차별은 눈살을 찌푸리는 단계를 넘어 사회에서 적극적으로 배제해 나가야 하는 것으로 자리매김** 하고 있다.

**우리 소프트웨어 엔지니어들은 진보적인 족속**들이다. 누구보다 먼저 스마트폰을 썼고, 새로운 기술에, 그 기술이 만들 변화에 항상 열려있다.

하지만, 우리가 사용하는 많은 어휘들은 과거 정부 주도/산업 주도로 경영정보시스템을 갖추던 시대에 쓰던 낙후된 용어에 머물러 있다. DB에서 Master/Slave라는 표현을 사용하는 것이나, Git에서 master 브랜치를 default 브랜치로 두는 것 등이다.

## 언어, 그 뿌리 깊은 차별의 역사

미국의 급진적인 **흑인 민권운동가 말콤 엑스는 이렇게 얘기했다고 한다.**

그가 인권운동으로 **감옥**에 투옥되었을 때 였다. 어스름한 불빛에 기대 몰래 책을 읽곤 하던 말콤은 어느 날 궁금증으로 사전을 펼쳤다.

그는 "black"과 "white"라는 단어를 찾아봤다고 한다. **"white"라는 단어에, 순결한, 고결한 등의 의미가 담겨있는 반면, "black"에는 더러운, 불결한 등의 의미가 담겨있는 것**을 보고 미국이라는 나라에서 흑인에 대한 차별이 얼마나 뿌리깊은가를 새삼 느꼈다고 한다. (미국에서 흑인은 Black, 백인은 White라고 부르니 말이다)

언어가 우리 사고를 전부 규정하는 것은 아니지만, **우리의 사고는 상당부분 언어에 종속**된다. 우리말보다 이누이트의 언어에 눈과 관련한 형용사가 훨씬 풍부하다고 한다. 당연히 눈을 바라보는 관점 역시 우리보다 이누이트 족이 훨씬 풍부할 것이다.

서론이 길었다.

익숙하다고 가만히 놔두면 안된다. 우린 **기술로 세상을 구원하겠다는 신념으로 뭉친 엔지니어**지 않은가.

그런 의미에서, 가장 쉽게, **우리가 쓰는 Git에서 더는 "master"라는 표현을 쓰지 않도록, 작은 변화를 일으켜 보자.**

## 하나. git에서 브랜치 이름 변경하기

우선 이미 존재하는 리포지토리(repository)의 master 브랜치를 main으로 이름을 변경해본다.

```shell
git branch -m master main
```

짜잔.

쉽다.

변화는 의외로 간단하고 단순하다.

만약 GitHub같이 remote가 있다면?

```shell
git fetch origin
git branch -u origin/main main
```

이렇게 하면, remote 역시 변경된다.

## 둘. 앞으로 생성되는 모든 git repo에서 master 대신 main 사용하기

매번 브랜치 명을 바꿔주는 것은 귀찮다.

이번에는 아예 전역 설정을 변경해 앞으로 생성되는 모든 git 리포지토리(repository)에서 master 대신 main을 default 브랜치 명으로 사용하도록 설정해보자.

```shell
git config --global init.defaultBranch main
```

이제부터는 `git init` 으로 git 리포지토리를 생성할 때, master 대신 main을 default 브랜치로 생성하게 된다.

## 더 나은 세상을 위한 작은 변화

휴.

생각보다 간단하지 않은가?

생각해보면, 왜 default 브랜치는 master라고 불려야 했을까?

이유는 충분하겠지만, 초심자가 git을 배울 때, 한번쯤은 의문을 가질 수밖에 없는, 어찌 보면 직관적이지는 않은 브랜치명이었다.
**main. 훨씬 명확하다. 누가봐도 메인 브랜치 아닌가.**

진작 바꿀걸 그랬다.

이로써 당신은 세상을 위한 작은 변화를 이뤘다.

세상은 결코 한꺼번에 바뀌지 않는다.

누군가 큰 사람이 나서서 모든 것을 바꾸는 식으로 진보하지 않는다.

각자가 자신이 서 있는 자리에서, 딱 한 걸음씩만 앞으로 나아가면,

세상은 한 걸음 성큼 나아간 셈이 된다.

\*\*한 사람이 걸으려면 60억 걸음이지만,

60억 세상 사람들이 한 걸음씩만 걸어도, 60억 걸음이니 말이다.\*\*

비록 우리의 작은 변화가 인종차별을 세상에서 종식시키지는 않겠지만,

**작은 변화들이 모이면 세상이 바뀐다고 그렇게 믿고 스스로를 칭찬해도 좋다.**

감사합니다.
