---
layout: post
title: "[TIL] Node로 JavaScript 객체 File 작성하는 법"
date: "2021-09-19"
categories:
- TIL
excerpt: |
Node로 JavaScript 객체 File 작성하는 법을 배웠다.
feature_text: |
## Node로 JavaScript 객체 File 작성하는 법
Node로 JavaScript 객체 File 작성하는 법을 배웠다.
feature_image: "https://images.unsplash.com/photo-1601445638532-3c6f6c3aa1d6?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=933&q=80"
image: "https://images.unsplash.com/photo-1601445638532-3c6f6c3aa1d6?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=933&q=80"
---

Node로 JavaScript 객체 File 작성하는 법을 배웠다.

`fs.writeFile` 과 `util.inspect` 를 적절히 사용하면 가능하다.

`fs.writeFile` 은 비동기적으로 파일을 쓰는 API다.

`util.inspect`는 디버깅 목적으로 사용될 수 있도록 객체의 string representation을 반환하는 메서드다.

나는 JavaScript 객체 파일을 작성할 때 stringify된 형태가 아닌 plain object로 작성되도록 하기 위해 이 메서드를 사용했다.

아래와 같은 예시가 있다고 하자.

```typescript
interface ICat {
 name: string;
  age: number
}

const cats: ICat[] = [...] // 매우 긴 배열
```

HTTP 요청에 대한 응답으로 돌아온 값인 `cats` 배열을 별도에 TypeScript 파일 `cats.ts` 에 작성하고 이를 export 하고 싶었다.

콘솔을 찍어보고, 복사해서 붙여넣어 파일을 만들 수도 있었지만, 조금은 개발자스럽게 스크립트 파일을 작성해 자동으로 파일을 write하도록 만들어 보았다.

Nest.js를 이용한 예시이다.

대략 아래와 유사한 데이터를 API 요청으로 가져온다고 가정한다.

```typescript
interface ICats {
  breed: string;
  description: string;
}

const cats: ICats[] = [
  {
    breed: "Persian",
    description:
      "long-haired breed of cat characterized by its round face and short muzzle",
  },
  {
    breed: "Siamese",
    description:
      "blue almond-shaped eyes; a triangular head shape; large ears; an elongated, slender, and muscular body",
  },
];
```

`ICats` 인터페이스가 배열로 제공되는 응답이다.

여기서 `breed` 만 따로 빼서 별도 파일로 추가한 후 상수로 관리하고 싶었다.

이제 이 데이터를 별도의 JavaScript 파일로 작성할 수 있도록 해본다.

```typescript
import { appendFileSync, writeFileSync } from "fs";
import { inspect } from "util";

interface ICats {
  breed: string;
  description: string;
}

interface IBreed {
  [key: string]: string;
}

const newFilePath = join("./src/constants", "cats.constant.ts");

// ICats[] => IBreed로 변환하는 reduce 함수
function createBreedObjFromCatsArr(cats: ICat[]) {
  return cats.reduce((acc, cur) => {
    return Object.assign(acc, {
      [cur.breed.toUpperCase()]: cur.breed,
    });
  }, {});
}

function createBreedObjFile() {
  // 파일을 생성
  writeFileSync(this.newFilePath, "export const BREEDS = ");

  // 생성된 파일에 Object를 추가
  appendFileSync(
    this.newFilePath,
    inspect(this.createBreedObjFromCatsArr(cats), {
      // 만약 response로 받은 배열의 길이가 길 경우, truncate되지 않게 하기 위해 maxArrayLength를 null로 설정;
      maxArrayLength: null,
    })
  );
}
```

여기서 `writeFileSync`, `appendFileSync` 를 쓴 것은 writeFile과 appendFile의 로직이 synchronous하게 동작해야 하기 때문이다. writeFile이 끝나기 전에 appendFile이 진행되어서는 안 되므로 sync로 동작하게 하였다.

사실 나는 상수형으로 관리할 때는 enum을 즐겨쓰지만, 아쉬운대로 node를 이용해 데이터를 받고, 이를 자동으로 상수형으로 변환해볼 수 있었다.
