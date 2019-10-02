---
layout: post
title: "React Native - build commands failed: DoubleConversion-dummy.m normal x86_64 objective-c com.apple.compilers.llvm.clang.1_0.compiler"
date: "2019-09-30"
categories:
  - React-Native
excerpt: |
  React Native - The following build commands failed{:} CompileC /Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/ios/build/Native_Makkcha/Build/Intermediates.noindex/Pods.build/Debug-iphonesimulator/DoubleConversion.build/Objects-normal/x86_64/DoubleConversion-dummy.o /Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/ios/Pods/Target\ Support\ Files/DoubleConversion/DoubleConversion-dummy.m normal x86_64 objective-c com.apple.compilers.llvm.clang.1_0.compiler
feature_text: |
  ## React Native - build commands failed: DoubleConversion-dummy.m normal x86_64 objective-c com.apple.compilers.llvm.clang.1_0.compiler
  React Native - The following build commands failed{:} CompileC /Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/ios/build/Native_Makkcha/Build/Intermediates.noindex/Pods.build/Debug-iphonesimulator/DoubleConversion.build/Objects-normal/x86_64/DoubleConversion-dummy.o /Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/ios/Pods/Target\ Support\ Files/DoubleConversion/DoubleConversion-dummy.m normal x86_64 objective-c com.apple.compilers.llvm.clang.1_0.compiler
feature_image: "https://images.unsplash.com/photo-1532003885409-ed84d334f6cc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
image: "https://images.unsplash.com/photo-1532003885409-ed84d334f6cc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
---

Build failure in debug.
Build Succeeds in Xcode but Fails in terminal when running `react-native run-ios`.

Original Error:
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/e1.png?raw=true" width="100%" />

## Possible Solutions

### Deleting Xcode Cache

```cmd
rm -rf ~/Library/Developer/Xcode/DerivedData/*
```

In my case, it didn't worked.

### Using Legacy Build System

in xcodeproj

1. Navigate to File > Project Settings > Build System.
2. Change this to Legacy Build System.

In my case, it didn't worked either. Some said that this issue isn't about build system.

### Cleaning Node Modules and iOS Build Directory

```cmd
$ rm -rf node_modules
$ rm -f yarn.lock
$ yarn
$ rm -rf ios/build/*
```

This is the only solution that worked for me. After removing all node_modules and yarn.lock file, I removed all files and sub-directories under ios/build.
Then, I ran `react-native run-ios` again, and it finally worked.

## References

- [Facebook React Native Github Issue - After upgrading to react-native 0.59 iOS build fails #23886](https://github.com/facebook/react-native/issues/23886)
