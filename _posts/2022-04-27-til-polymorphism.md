---
layout: post
title: "[TIL] 다형성(Polymorphism)"
date: "2022-04-27"
categories:
  - TIL
excerpt: |
  다형성(Polymorphism)

feature_text: |
  ##  다형성(Polymorphism)
  다형성은 같은 타입이지만 실행 결과가 다양한 객체를 이용할 수 있는 성질을 의미한다.
feature_image: "https://images.unsplash.com/photo-1510172951991-856a654063f9?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=987&q=80"
image: "https://images.unsplash.com/photo-1510172951991-856a654063f9?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=987&q=80"
---

## 다형성은 같은 타입이지만 실행 결과가 다양한 객체를 이용할 수 있는 성질을 의미한다

```java
class Person{
  public void introduce() {
    System.out.println("안녕하세요! 사람입니다.");
  }
}

class Student extends Person{
  @Override
  public void introduce() {
    System.out.println("안녕하세요! 학생입니다.");
  }
}
/*
  ...
*/
Person james = new Student();
james.introduce(); // "안녕하세요! 학생입니다."
```

동일한 타입을 사용하지만 다양한 결과가 나오게 된다.

프로그램은 수많은 객체들이 서로 연결되고 각자의 역할을 한다.
각 객체들은 다른 객체로 교체될 수 있어야 한다. 특정 맥락에서 보다 상황에 적합한 객체가 있을 수 있기 때문이다.

자식 클래스는 부모의 메소드를 오버라이딩해서 메소드의 실행내용을 변경하게 되고, 더 적합한 결과를 만들 수도 있는 것이다.

요약하자면

> 한 행동을 여러 방법으로 구현하고 상황에 맞는 적합한 구현을 선택해서 사용할 수 있는 성질을 "다형성"이라고 한다.

## 자동 타입 변환과 강제 타입 변환

자식 타입은 언제든 부모 타입으로 자동 타입 변환이 가능하다.
단, 다음과 같은 규칙이 따른다.

1. 부모 타입으로 변환되면, 부모 클래스에 선언된 필드와 메소드만 접근 가능하다.
2. 만약 자식클래스에서 특정 메소드를 오버라이딩 했다면, 해당 메서드에 한해 자식 클래스의 메소드가 대신 실행된다.

한편 강제 타입 변환은 다음의 경우에만 가능하다.

- 자식 타입을 부모 타입으로 자동변환한 상태인데, 이를 다시 자식 타입으로 변환한다 (자식 타입 -> 부모 타입 -> 자식타입)

보통 자식 타입이 부모 타입으로 변환되면, 부모 타입에 선언된 필드와 메서드만 접근 가능하기 때문에, 자식 타입에 선언된 필드와 메소드를 사용하기 위해 강제 타입 변환을 한다.

강제 타입 변환은 언제나 가능하지는 않다.
따라서 객체의 타입을 꼭 확인하는 습관을 가져야 한다.

```java
/* ... */
if (person instanceof Student) {
  Student student = (Student) person;
}
```
