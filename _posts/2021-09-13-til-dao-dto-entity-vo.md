---
layout: post
title: "[TIL] DAO, DTO, Entity, VO 정리해보기"
date: "2021-09-13"
categories:
  - TIL
excerpt: |
  DAO, DTO, Entity, VO는 Spring에서 일반적으로 사용되지만, Nest.js에서도 일부 사용되는 개념이다. 이들 개념을 정리해본다.
feature_text: |
  ## DAO, DTO, Entity, VO 정리해보기
  DAO, DTO, Entity, VO는 Spring에서 일반적으로 사용되지만, Nest.js에서도 일부 사용되는 개념이다. 이들 개념을 정리해본다.
feature_image: "https://images.unsplash.com/photo-1545987796-200677ee1011?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3300&q=80"
image: "https://images.unsplash.com/photo-1545987796-200677ee1011?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3300&q=80"
---

DAO, DTO, Entity, VO 정리해보기

DAO, DTO, Entity, VO는 Spring에서 일반적으로 사용되지만, Nest.js에서도 일부 사용되는 개념이다. 이들 개념을 정리해본다.

Nest.js는 Spring MVC와 유사한 면이 많다.

사용하는 용어들도 어느 정도 그러한 것 같다.

실제로 Nest.js를 사용하려다 보니, 데이터의 스키마나 인터페이스를 어떤 형태로 관리해야 할지 궁금함이 생겼다.

즉, Spring에서 사용되는 DAO, DTO, Entity, VO와 같은 개념이 궁금해졌다.

## DAO

Data Access Object를 줄인 말이다. DB의 데이터에 접근하고 이를 다루는 객체다. SQL을 사용하여 DB에 접근한 뒤, CRUD API를 제공한다.

## Entity

Entity는 영속화될 수 있고 식별가능한 (예를들어 ID혹은 유니크한 복합키가 존재하는) 객체를 의미합니다.

Entity는 영속화될 수 있고 식별가능한 객체로 DB의 테이블과 매칭될 클래스이다(1:1 대응은 현실적으로 되지 않는다. DB를 객체지향스럽게 표현하는 것에는 한계가 있기 때문이다).

## DTO

계층 간 데이터 교환을 위한 객체다. 데이터의 전송과 계층의 분리를 돕는다.

getter/setter를 갖는다.

DB에서 service로, controller로, client로 데이터를 주고받을 때, 데이터를 담는 컨테이너(객체)다.

```typescript
class UserDto {
  name: string;
  id: string;
  age: number;
}
```

## VO

Read Only. 특정한 비즈니스 값을 담는 객체다. 담긴 데이터 그 자체에 의미를 둔다(DTO는 계층 간 전송/교환이 핵심).

entity나 domain의 특정 개념(일부)을 표현하고, 식별성이 존재하지 않는다.

> VO는 특정 개념을 나타내기 위함입니다. 예를들자면 상품 entity안에 할인률, 판매가, 할인가격 등등 price에 대한 필드들이 있고 그 안에 고유한 개념이 담겨져 있다면 그것을 Price VO로 분리함으로써 좀더 명확하게 도메인에 대한 개념을 표현하고 다른 코드에서도 Price라는 개념을 통해 객체를 캡슐화하여 사용할 수 있습니다. price.할인가계산() 같은 용도로 사용할 수 있겠네요. 이렇게 되면 계산할때마다 상품 외부의 객체가 상품 내부의 필드를 가져와서 계산하지 않아도 되어 캡슐화를 높이고 다른 곳에서 price라는 개념을 사용할때에도 정해진 price VO를 사용함으로서 로직이 흩어지는 일없이 한곳으로 모아 응집도를 높일 수 있습니다.
>
> @[devcrema](https://okky.kr/user/info/82323) 님 답글 발췌.

## References

- [OKKY - DAO, DTO, VO 개념이 정확히 뭔가요?](https://okky.kr/article/786760)
  어찌보면 가장 비공식적인 소스 OKKY. 그렇지만 다른 블로그들이 학생 레벨에서 정리한 것인 경우가 많아, 이 글이 가장 신뢰성 있었다.
- [DeCoding - VO vs DTO, DAO vs Repository, 그리고 Domain과 프로젝트 구조](https://xengom.com/spring/vo-dto-dao-repository-domain/)
  새로 서버 개발을 공부하는 사람 입장에서 가장 말미에 있는 "프로젝트 구조"가 서비스의 규모에 맞게 진화해야 하며, 어떤 방향으로 진화하는가는 나름 공감가고 끄덕여지는 부분이 있었다.
- [[GitHub Issues] MagicalLas - DAO, DTO, Entity Class 차이 및 package 구조에 대한 오류 #3](https://github.com/siwony/TIL/issues/3)
  실무 경험이 있는 개발자가 이슈를 통해 소통한 것을 구글링으로 찾았다. 무조건적으로 신뢰할 수 있는지는, 좀 더 공부해 봐야할 것 같다.
- [ohzzi.log - Entity, DAO, DTO가 무엇이며 왜 사용할까?](https://velog.io/@ohzzi/Entity-DAO-DTO%EA%B0%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%99%9C-%EC%82%AC%EC%9A%A9%ED%95%A0%EA%B9%8C)
- [heejung Kwon - [DAO] DAO, DTO, Entity Class의 차이](https://gmlwjd9405.github.io/2018/12/25/difference-dao-dto-entity.html)
  가장 처음에 개념을 잡기 위해 읽은 글이었다.
- [@JUN0126 - [고오급 개발자가 되는 길] DAO, DTO, VO, Entity](https://jun0126.tistory.com/49)
  bullet point 요약을 제공하고 있어서 wrap up 하기 좋다.
