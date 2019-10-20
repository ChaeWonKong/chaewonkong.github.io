---
layout: post
title: "React Native - 브릿지(Bridge)를 이용해 Android에서 Native Module 이용하기"
date: "2019-10-10"
categories:
  - React-Native
excerpt: |
  React Native Android에서 브릿지(Bridge)를 이용해 네이티브 모듈(Native Module)을 사용하는 방법을 알아본다.
feature_text: |
  ## React Native - 브릿지를 이용해 Android에서 Native Module 이용하기
  React Native Android에서 브릿지(Bridge)를 이용해 네이티브 모듈(Native Module)을 사용하는 방법을 알아본다.
feature_image: "https://cdn.pixabay.com/photo/2016/11/29/12/30/android-1869510_1280.jpg"
image: "https://cdn.pixabay.com/photo/2016/11/29/12/30/android-1869510_1280.jpg"
---

오늘은 **Java로 Android용 네이티브 모듈(Native Module)을 작성하고 이것을 브릿지(Bridge)를 이용해 React Native에서 사용하는 방법**을 알아본다.

React Native를 사용하다 보면, 빠르든 늦든 거의 반드시 Native Module을 사용해야 하는 경우가 온다.
슬프지만 사실이다.

React Native는 강력하지만 모든 것이 다 제공되진 않기 때문이다.

우리는 우리가 제공하는 앱에서 다른 앱을 열어주기 위해 어쩔 수 없이 Native Module을 만들어 사용해야만 했다.
React Native는 기본적으로 **Linking**을 제공하기에 해당 앱의 커스텀 스킴(Custom Scheme)을 알면 Linking을 활용해 연결할 수 있다.

```react
import {Linking} from "react-native"

Linking.openURL(url).catch((err) => console.error('An error occurred', err));
```

하지만 커스텀 스킴을 알 수 없는 상황에서 다른 앱을 연결해 줄 수 있는 방법은 없었다.

**iOS의 경우 반드시 커스텀 스킴이 필요하다.** 커스텀 스킴 없이는 사용자의 폰에 설치되어 있는 앱을 실행할 수 있는 방법이 없는 것으로 알고 있다.
하지만 **Android는 다르다. 네이티브 코드로 작성한다면 설치되어 있는 앱을 찾아 열 수 있는 방법이 존재**했고, 우리는 Native Module을 작성하게 되었다.

## Native Module 작성하기

먼저 `android/app/src/main/java/com/[프로젝트 명]/` 디렉토리를 열고 LinkExternalAppModule.java와 LinkExternalAppPackage.java를 만든다.

"LinkExternalApp"은 우리가 만들고자 하는 네이티브 모듈의 특성을 고려해 임의로 설정한 이름이다. 다른 형태의 이름으로 바꿔도 좋지만, **"...Module.java", "...Package.java" 형태로 만들자.**

먼저 **LinkExternalApp.java**를 열고 다음과 같이 코드를 작성한다.

```java
package com.native_makkcha;

// Import android modules
import android.app.Activity;
import android.content.Context;
import android.content.Intent;
import android.net.Uri;

// Import React Native dependencies
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.bridge.ReactContext;
import com.facebook.react.bridge.Callback;
import com.facebook.react.bridge.ReactContextBaseJavaModule;
import com.facebook.react.bridge.ReactMethod;

public class LinkExternalAppModule extends ReactContextBaseJavaModule {
  private static ReactApplicationContext reactContext;

  public LinkExternalAppModule(ReactApplicationContext reactContext) {
    super(reactContext);
    this.reactContext = reactContext;
  }

  @Override
  public String getName() {
    return "LinkExternalApp";
  }

  @ReactMethod
  public void linkExternalApp(String packageName, final Callback callback) {

    // Context from reactContext
    Context context = reactContext;
    Activity activity = getCurrentActivity();

    // Intent set from packageName
    Intent intent = context.getPackageManager().getLaunchIntentForPackage(packageName);
    if (intent == null) {
      // Linking to Google Play Store
      intent = new Intent(Intent.ACTION_VIEW, Uri.parse("market://details?id="+packageName));
    }

    activity.startActivity(intent);
    // callback.invoke();
  }

}
```

**LinkExternalAppPackage.java**

```java
package com.native_makkcha;

import com.facebook.react.ReactPackage;
import com.facebook.react.bridge.NativeModule;
import com.facebook.react.bridge.ReactApplicationContext;
import com.facebook.react.uimanager.ViewManager;

import java.util.ArrayList;
import java.util.Collections;
import java.util.List;

public class LinkExternalAppPackage implements ReactPackage {

    @Override
    public List<ViewManager> createViewManagers(ReactApplicationContext reactContext) {
        return Collections.emptyList();
    }

    @Override
    public List<NativeModule> createNativeModules(ReactApplicationContext reactContext) {
        List<NativeModule> modules = new ArrayList<>();

        modules.add(new LinkExternalAppModule(reactContext));

        return modules;
    }
}
```

<!-- 코드의 의미를 하나하나 살펴봐주자 -->

## Native Module Link 하기

`android/app/src/main/java/com/[프로젝트 명]/MainApplication.java`를 연다.

## JavaScript 코드에서 Native Module 사용하기

## 참고 자료

- [Facebook React Native - Native Modules(Android)](https://facebook.github.io/react-native/docs/native-modules-android)
- [React-Native에서 native module 이용하기](https://medium.com/@kyo504/react-native%EC%97%90%EC%84%9C-native-module-%EC%9D%B4%EC%9A%A9%ED%95%98%EA%B8%B0-d4728337af4e)
