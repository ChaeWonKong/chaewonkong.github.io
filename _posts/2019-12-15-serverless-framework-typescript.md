---
layout: post
title: "Serverless - TypeScript로 서버리스 프레임워크(Serverless Framework)를 활용한 서버리스 아키텍처 구현"
date: "2019-12-15"
categories:
  - Serverless
excerpt: |
  서버리스 프레임워크(Serverless Framework)와 Node를 이용한 TypeScript 서버 만들기 - AWS Lambda 및 API Gateway 활용
  TypeScript를 활용하고, Node를 사용하는 서버리스 아키텍처를 서버리스 프레임워크(Serverless Framework) 기반으로 구현해 본다.
feature_text: |
  ## TypeScript로 서버리스 프레임워크(Serverless Framework)를 활용한 서버리스 아키텍처 구현
  TypeScript를 활용하고, Node를 사용하는 서버리스 아키텍처를 서버리스 프레임워크(Serverless Framework) 기반으로 구현해 본다.
feature_image: "https://images.unsplash.com/photo-1495592822108-9e6261896da8?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80"
image: "https://images.unsplash.com/photo-1495592822108-9e6261896da8?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2250&q=80"
---


서버리스 프레임워크와 타입스크립트 함께 사용하기

AWS Lambda나 API Gateway는 이미 훌륭한 서버리스 툴이지만, 서비스가 점점 복잡해 질수록, 주기적으로 설정해줘야 하는 번잡함이 커지게 되어 불편함이 있다.

매번 함수를 생성하고 API Gateway를 연동하는 등의 작업을 진행하는 것은 생각보다 반복적인 일이기에, 이를 코드 단에서 일목요연하게 관리하고 한번에 적용시키고 싶다는 생각을 하게 된다.

서버리스 프레임워크(Serverless Framework)는 이러한 불편함을 해결하는 탁월한 해결책이기에, **AWS Lambda나 API Gateway의 사용법을 익힌 사용자라면 서버리스 프레임워크를 사용하기를 강력 추천**한다.

본 튜토리얼에서는 **Node.js 및 TypeScript를 사용하는 법을 위주로 살펴본다.**



## 준비사항

먼저 AWS 콘솔에서 IAM 사용자를 추가해보자.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/serverless/01.png?raw=true" width="100%">

[AWS](https://aws.amazon.com/ko/)에 접속해 위 사진처럼 "보안 자격 증명"으로 이동한다.


<img src="https://github.com/ChaeWonKong/image-resource/blob/master/serverless/02.png?raw=true" width="100%">
이동한 페이지에서 위 사진처럼 사용자 > 사용자 추가를 통해 IAM 사용자를 추가하는 페이지로 이동한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/serverless/03.png?raw=true" width="100%">
사용자 이름을 입력한 후 "프로그래밍 방식 액세스"에 체크하고 "다음:권한" 버튼을 눌러 다음으로 이동한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/serverless/04.png?raw=true" width="100%">
**권한 부분은 중요하다.** 먼저 **"기존 정책 직접 연결"** 탭을 선택한다.
다음으로 **"AdministratorAccess"**항목에 체크한다.

여기서 ```AdministratorAccess```권한은 AWS 권한과 동일한 작업을 모두 수행할 수 있는 **강력한 권한**이다. 따라서 절대로 이후 부여받는 **SecretKey를 노출해선 안 된다.**

"다음:태그" 버튼을 눌러 다음으로 이동한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/serverless/05.png?raw=true" width="100%">
태그는 따로 추가하지 않고 지나간다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/serverless/06.png?raw=true" width="100%">
사용자 만들기를 클릭한다.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/serverless/07.png?raw=true" width="100%">
사용자 생성이 완료되었다.

**액세스 키 ID와 시크릿 액세스 키를 csv 파일로 다운로드**받을 수 있다. 다운 받아 안전한 곳에 저장해두자.
이후 서버리스 프레임워크를 사용할 때 필요하다.

## 설치

먼저 글로벌하게 serverless를 설치한다.

serverless 혹은 sls라는 명령어가 등록되어 서버리스 프레임워크를 제어할 때 사용하게 된다.

```shell
npm install -g serverless
```



다음으로 Node.js로 템플릿을 생성하자.

```shell
serverless create --template aws-nodejs
```


이 디렉토리에는 아직 ```package.json```이 없다. npm 환경을 생성하자.
```shell
npm init
```


다음으로 TypeScript 관련 모듈들을 설치해 준다.

참고로 TypeScript는 개발 환경에서만 사용하고, 배포시에는 commonJS로 컴파일되므로 devDependency에 추가해주면 된다.



```json
"devDependencies": {
    "@types/aws-lambda": "^8.10.37",
    "@types/chai": "^4.2.6",
    "@types/mocha": "^5.2.7",
    "@types/request": "^2.48.3",
    "chai": "^4.2.0",
    "mocha": "^6.2.2",
    "serverless-offline": "^5.12.1",
    "serverless-plugin-typescript": "^1.1.9",
    "tslint": "^5.20.1",
    "tslint-config-airbnb": "^5.11.2",
    "typescript": "^3.7.3"
  }
```

위 모듈들을 추가해줄 것이다.



핵심 모듈들을 먼저 살펴보자.

```@types```로 제공되는 것들은 type definition 모듈로 기존에 존재하던 npm 모듈을 TypeScript에서 사용할 수 있도록 돕는 역할을 한다.



```chai```와 ```mocha```는 테스트를 진행하도록 돕는 라이브러리다. 필수는 아니다.

```serverless-offline```은 우리가 서버리스 프레임워크를 기반으로 작성한 코드를 쉽게 테스트해볼 수 있는 환경을 제공한다.

"http://localhost:3000"과 같은 로컬호스트에서, 작성한 API를 테스트 해볼 수 있고, 저장시 리로딩된다. 



가장 중요한 라이브러리는 ```serverless-plugin-typescript```이다.

서버리스 프레임워크를 TypeScript와 함께 쓸 수 있도록 도와주며, 자동 컴파일을 제공하여 매우 편리한 개발을 가능하게 한다.



```tslint```는 esLint의 TypeScript 버전이라 생각하면 된다. 소스코드에 문제가 있는지 여부를 실시간으로 잡아주는 린팅 기능을 제공해 준다. 



```tslint-config-airbnb```는 AirBnB에서 만든 JavaScript 스타일 가이드의 TypeScript 버전이다. 

#### AirBnB 규칙 예시

- 모든 참조는 `const` 를 사용하고, `var` 를 사용하지 마십시오.
- 오브젝트를 작성할때는, 리터럴 구문을 사용하십시오.
- Additional trailing comma: **Yup.**(끝날 때 콤마 붙이기: 반드시 예쓰!)
- 세미콜론 무조건 씁니다.
- 오브젝트, 함수 그리고 인스턴스에는 camelCase를 사용해 주십시오.

이 밖의 자세한 규칙의 내용은 [Airbnb JavaScript 스타일 가이드](https://github.com/tipjs/javascript-style-guide)를 참고하자.



마지막으로 ```typescript```를 설치해 준다.


## 타입스크립트 설정
다음으로 tsLint와 tsconfig를 설정해보자.

먼저 루트 디렉토리에 ```tslint.json``` 파일을 생성한다.
파일 내용으로 아래를 입력한다.

```json
{
  "defaultSeverity": "error",
  "extends": ["tslint-config-airbnb"],
  "jsRules": {},
  "rules": {
    "import-name": [false],
    "variable-name": [
      true,
      "ban-keywords",
      "check-format",
      "allow-pascal-case"
    ],
    "quotemark": [true, "single", "avoid-escape", "avoid-template"]
  },
  "rulesDirectory": []
}
```

다음으로 ```tsconfig.json```을 만든다.
```json
{
  "compilerOptions": {
    "module": "commonjs",
    "target": "es6",
    "lib": ["es6", "dom"],
    "moduleResolution": "node",
    "rootDir": "./",
    "sourceMap": true,
    "allowJs": true,
    "noImplicitAny": true,
    "noUnusedLocals": true,
    "noImplicitThis": true,
    "strictNullChecks": true,
    "noImplicitReturns": true,
    "preserveConstEnums": true,
    "suppressImplicitAnyIndexErrors": true,
    "forceConsistentCasingInFileNames": true
  },
  "exclude": ["node_modules", "build", "webpack"],
  "types": ["typePatches"]
}
```



## AWS 계정 설정

다음으로 서버리스 프레임워크를 사용하기 위해 AWS 계정 정보를 등록해보자.

등록된 계정은 서버리스 배포시 사용된다.



```shell
serverless config credentials --provider aws --key [YourKeyID] --secret [YourSecretKey]
```



## API 작성

이제 실제로 API를 작성해보자.

우선 루트 디렉토리에 있는 ```handler.js``` 파일을 삭제해준다.
```src/handler.ts``파일을 생성한다.

```typescript
import { Handler, Context } from 'aws-lambda';

interface HelloResponse {
  statusCode: number;
  body: string;
}

const hello: Handler  = async (event: any, context: Context) => {
  const response: HelloResponse = {
    statusCode: 200,
    body: JSON.stringify({
      message: "Hello World!",
    }),
  };
  return response
};

export { hello };
```



## 배포 설정

거의 다 왔다.



배포를 위해서는 ```serverless.yml```파일에서 몇 가지 수정할 부분이 있다.

- 플러그인 등록
  앞서 설치했던 플러그인들을 등록해줘야 작동한다.

  ```yaml
  plugins:
    - serverless-plugin-typescript
    - serverless-offline
  ```

- 프로바이더 설정
  프로바이더란 어떤 클라우드를 사용할지, 어떤 런타임에서 돌릴지, 리전은 어디로 할지 등을 설정하는 부분이다.
  AWS 기준 서울 리전은 ap-northeast-2이다.

  ```yaml
  provider:
    name: aws
    runtime: nodejs10.x
    region: ap-northeast-2
  ```

  

- 함수 등록
  앞서 작성한 함수 모듈로 API를 만들기 위해 등록해준다. 이렇게 등록을 하면 AWS Lambda와 API Gateway를 자동으로 연동해 API를 생성해준다.

  ```yaml
  functions:
    hello:
      handler: src/handler.hello
  
      events:
        - http:
            path: hello
            method: get
  ```

  상상했다시피 API의 엔드포인트는 "https://[URL]/hello"다. [URL]은 서버리스 프레임워크를 배포할 때 확인할 수 있다.







**serverless.yml**

```yaml
service: [YourServiceName]
plugins:
  - serverless-plugin-typescript
  - serverless-offline

provider:
  name: aws
  runtime: nodejs10.x
  stage: dev
  region: ap-northeast-2


functions:
  hello:
    handler: src/handler.hello

    events:
      - http:
          path: hello
          method: get
```

자, 이제 로컬에서 제대로 작동하는지 확인해보자.

```shell
serverless offline
```

터미널에서 위 코드를 입력해 로컬 서버를 구동한다.

"http://localhost:3000/hello" 에 접속해 "Hello World!" 메세지가 제대로 출력되는지 확인한다.

## 배포

다음 명령어로 배포를 진행한다.

```shell
serverless deploy
```

AWS에서 Lambda에 접속해 보면 아래 사진처럼 정상적으로 우리의 함수가 등록된 것을 확인할 수 있다.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/serverless/08.png?raw=true" width="100%">

## 참고자료

- [AirBnB JavaScript 스타일 가이드](https://github.com/tipjs/javascript-style-guide)
- [안경잡이 개발자, Serverless 프레임워크(Framework)의 기본적인 사용법](https://ndb796.tistory.com/311)