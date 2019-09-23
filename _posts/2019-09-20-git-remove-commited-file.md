---
layout: post
title: "Git에 커밋 된 커밋하면 안될 파일 제거하기"
date: "2019-09-20"
categories:
  - Git
excerpt: |
  React Native에서 안드로이드를 빌드하는 과정에서 git에 실수로 커밋된 hprof 대용량 파일을 git에서 삭제하는 방법을 알아본다. 커밋하면 안될 파일이 커밋된 경우, 찾아서 삭제하는 방법을 알아본다.
feature_text: |
  ## Git에 커밋 된 커밋하면 안될 파일 제거하기
  React Native에서 안드로이드를 빌드하는 과정에서 git에 실수로 커밋된 hprof 대용량 파일을 git에서 삭제하는 방법을 알아본다. 커밋하면 안될 파일이 커밋된 경우, 찾아서 삭제하는 방법을 알아본다.
feature_image: "https://images.unsplash.com/photo-1556075798-4825dfaaf498?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1355&q=80"
image: "https://images.unsplash.com/photo-1556075798-4825dfaaf498?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1355&q=80"
---

## 발생한 에러

```cmd
$ remote: error: File android/java_pid57988.hprof is 490.48 MB; this exceeds GitHub's file size limit of 100.00 MB
```

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/git.png?raw=true" width="80%" alt="terminal-img" />

React Native에서 안드로이드를 빌드하다 보면 "android/java_pid57988.hprof"와 같은 파일이 생성된다. 지워도 무방한 파일이지만 이 파일이 커밋되는 경우, 400MB 이상의 용량으로 인해 Github 푸시가 실패하는 에러가 발생하기도 한다.

이를 해결하기 위해서는 해당 파일을 깃이 추적하지 않도록 삭제해야 한다.

## 해결방법

```cmd
$ git filter-branch --tree-filter 'rm -rf <path/to/filename>' HEAD
```

사용예:

```cmd
$ git filter-branch --tree-filter 'rm -rf android/java_pid57988.hprof' HEAD
```

모든 커밋들에 대해 해당 파일이 커밋되지 않도록 설정할 수 있다.
