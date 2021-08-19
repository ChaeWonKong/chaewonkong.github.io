---
layout: post
title: "[TIL] Mac - Unity 설치부터 VS Code를 이용한 개발환경 세팅까지
date: "2021-08-19"
categories:
  - TIL
excerpt: |
  Mac에 Unity Hub를 이용해 Unity를 설치하고, Visual Studio Code를 이용해 개발하도록 환경 세팅을 진행해본다.
feature_text: |
  ## Mac - Unity 설치부터 VS Code를 이용한 개발환경 세팅까지
  Mac에 Unity Hub를 이용해 Unity를 설치하고, Visual Studio Code를 이용해 개발하도록 환경 세팅을 진행해본다.
feature_image: "https://images.unsplash.com/photo-1524351543168-8e38787614e9?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=967&q=80"
image: "https://images.unsplash.com/photo-1524351543168-8e38787614e9?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=967&q=80"
---

Unity는 Unreal Engine과 함께 가장 흔히 사용되는 게임 엔진이다. 나는 Unity를 선택했는데, Unity는 모바일 게임 개발에서 보다 흔하게 사용되기 때문이다.

더 현실감 있고 사실적인 묘사가 필요한 고사양 게임에는 Unreal Engine이 조금 더 적합하다는 얘기가 많은 것 같다.

결정 하는 데에는 [<유니티(Unity) VS 언리얼 엔진(Unreal Engine)>](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=cdw0424&logNo=221525769932) 블로그를 많이 참고했다.

## 개발환경 세팅

각자에 맞는 방법이 있겠지만, 나는 이렇게 개발환경을 세팅했다.

### Unity 설치

Unity는 Unity Hub를 이용해 설치했다. Unity Hub를 이용하면 보다 편리하게 Unity 버전을 관리할 수 있고, 디펜던시들을 설치할 수 있다.

[Unity Download 페이지](https://unity3d.com/kr/get-unity/download)에서 **"Unity Hub 다운로드"** 버튼을 클릭하면 된다.

참고로, 개인 및 소규모 목적의 Unity 이용은 무료 라이센스로도 충분하다. 유료 라이센스는 프리미엄 기술 지원 서비스와 관련 있는 것으로 알고 있다.

Unity Hub에서 **Installs** 에 들어가 필요한 버전을 설치하면 된다. 최신 버전 중에서 LTS 버전을 선택해 설치했다. LTS는 Long Term Support의 약자로, 공식 버전 중에서도 가장 오랜 기간 동안 유지보수 되는 버전이므로 가장 안정성이 높다. 최신 버전일수록 최신 기능들이 많으므로, **최신 버전 중에서 LTS 버전을 선택할 것** 을 추천한다.

### IDE/Code Editor 선택

보통 Unity 개발은 Visual Studio로 하는 것이 국룰인 것 같다. 하지만, 코드 에디터는 취향에 따르는 법. 개인적으로 느끼기에 Visual Studio는 조금 무겁다는 느낌이 있다. 빠르게 실행해서 코드를 수정하는 데 불편함이 있다고 느끼기에, 요즘 갈수록 강력해지는 **VS Code를 이용해 개발환경을 세팅**해 보기로 했다.

Unity의 경우 Android Studio를 이용해야만 하는 Android나 Xcode를 이용해야 하는iOS 개발과는 다르게 빌드 툴과 코드 에디터가 분리되어 있다. **Unity 프로젝트는 Unity Editor로 빌드하며, 코드 작성은 Visual Studio와 같은 별도의 코드 에디터를 이용하는 구조다.** 따라서, 본인이 사용하고 싶은 코드 에디터를 사용하면 된다.

일반적으로는

1. MonoDevelop
2. Visual Studio
3. VS Code

가 흔히 사용된다고 한다.

참고로 내가 생각하는 VS Code의 장점은 다음과 같다.

1. 강력한 에디터 성능 (IntelliSense 등)
2. C# 뿐만 아니라 JavaScript, Python 등 모든 언어를 지원하는 범용 에디터다
3. 로딩 없이 바로 실행되는 가볍다
4. 익스텐션을 통해 얼마든지 본인의 필요에 맞게 기능을 추가할 수 있고, 커스터마이징이 용이하다

이러한 이유에서 VS Code를 선택했다.

### VS Code 개발환경 세팅

이미 익숙하게 사용하고 있는 코드 에디터지만, Unity 개발을 위해서는 약간의 세팅이 필요하다.

우선 [Mono를 설치](https://www.mono-project.com/download/stable/) 해준다. "Visual Studio channel"로 설치하면 된다. Mono가 설치되어 있어야 코드 에디터에서 Unity 관련 API의 참조를 찾지 못하는 에러가 발생하지 않는다고 한다.

다음으로 [.NET Core SDK](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp)를 설치한다. .NET Framework를 이용하는 데 필요하다.

설치 후 터미널에서 설치 완료 여부를 확인해본다.

```shell
dotnet
```

다음으로 [C# 익스텐션](https://marketplace.visualstudio.com/items?itemName=ms-dotnettools.csharp) 을 설치한다. VS Code에서 다른 익스텐션을 설치하듯 확장 탭에서 "C#"을 검색해 설치하면 된다.

### Unity Editor에서 external script editor 설정

마지막으로 VS Code를 기본 코드 에디터로 사용하기 위해 Unity Editor에서 간단한 설정을 진행해 준다.

Preferences > External Tools > External Script Editor에서 드롭다운을 클릭하고 Visual Studio Code를 선택해 준다.

### Troubleshooting

만약 `[ERROR] Error: Unable to find Mono. Ensure that Mono's '/bin' folder is added to your environment's PATH variable.`와 같은 에러가 발생한다면 `bashrc`, `bashprofile`, `zshrc` 등 자신이 사용하는 것을 열어서 아래처럼 추가해주자.

```shell
export PATH=$PATH:/Library/Frameworks/Mono.framework/Versions/Current/bin/


```

만약 `Omnisharp`이 Mono와 관련해 다른 에러를 일으킨다면, VS Code에서 Mono를 Global에서 찾을 수 있게도 해주자.

```json
{
  "omnisharp.useGlobalMono": "always"
}
```

#### 참고

VS Code를 항상 최신으로 업데이트 하는 것은 위험할 수 있다. VS Code 업데이트는 보수적으로 하자.

### References

- [Cannot start OmniSharp because Mono version >=6.4.0 is required](https://stackoverflow.com/questions/66507869/cannot-start-omnisharp-because-mono-version-6-4-0-is-required)
- [VS Code로 유니티 개발하기 (macOS 사용자를 위한 팁 포함)](https://www.androidhuman.com/2020-09-14-unity_with_vscode)
- [[Tip] 유니티에서 VSCode : Visual Studio Code 사용하기 / 필수 Extensions 소개 / 테마 변경하는 법 / 여러 오류 해결법](https://m.blog.naver.com/hana100494/222075610297)
