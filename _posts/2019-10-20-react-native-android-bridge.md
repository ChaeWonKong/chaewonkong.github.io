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
  public void linkExternalApp(String packageName) {

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
  }

}
```

먼저 Activity, Intent, Uri를 import 해준다.

```java
// Import android modules
import android.app.Activity;
import android.content.Intent;
import android.net.Uri;
```

Activity를 이용해 외부 앱을 실행시킬 것이고, Intent를 이용해 그 외부 앱으로 연결해줄 것이다. Uri를 이용해서는 앱이 설치되어 있지 않을 경우 스토어로 연결해줄 것이다.

다음으로 클래스를 선언하고 그 안에서 reactContext와 getName 메소드를 설정해준다.
getName 메소드에서는 이후 JavaScript 코드에서 우리가 작성하는 모듈을 import 할 때 사용할 이름을 지정해 주면 된다.

```java
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
}
```

이제 우리가 필요로 하는 메소드를 작성할 차례이다.

```java
@ReactMethod
public void linkExternalApp(String packageName) {

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
}
```

먼저 `@ReactMethod`라는 어노테이션을 추가한다. 이렇게 해야지만 JavaScript에서 접근이 가능해진다.

다음으로 return 타입 void인 메소드를 선언한다.
우리는 packageName을 String 인자로 받아 처리하도록 했다. 필요한 경우 콜백 함수를 받을 수도 있다.

context와 activity를 선언하고, Intent 설정을 해준다.
`Intent intent = context.getPackageManager().getLaunchIntentForPackage(packageName);`는 앱이 실행되는 안드로이드 폰에서 packageName에 해당하는 앱을 찾아 intent 설정을 해준다.

마지막으로 분기문을 추가해 만약 앱이 설치되어 있지 않을 경우 구글 플레이 스토어로 이동시켜 준다.

`activity.startActivity(intent);`는 해당 메소드가 실행될 경우 새 앱을 열어주도록 한다.

다음으로는 패키지 파일을 작성하자.
같은 디렉토리 내에 아래와 같이 파일을 생성한다.

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

`modules.add(new LinkExternalAppModule(reactContext));`를 통해서 reactContext를 인자로 넘겨주고 모듈 인스턴스를 등록한다.

## Native Module Link 하기

이제는 **MainApplication.java**에 우리가 작성한 Native Module을 연결할 차례다.

`android/app/src/main/java/com/[프로젝트 명]/MainApplication.java`를 연다.

```java
package com.native_makkcha;

import android.app.Application;
import android.util.Log;

import com.native_makkcha.LinkExternalAppPackage; // 이 부분을 추가한다

import com.facebook.react.PackageList;
import com.facebook.hermes.reactexecutor.HermesExecutorFactory;
import com.facebook.react.bridge.JavaScriptExecutorFactory;
import com.facebook.react.ReactApplication;
import com.facebook.react.ReactNativeHost;
import com.facebook.react.ReactPackage;
import com.facebook.soloader.SoLoader;

import java.util.List;

public class MainApplication extends Application implements ReactApplication {

  private final ReactNativeHost mReactNativeHost = new ReactNativeHost(this) {
    @Override
    public boolean getUseDeveloperSupport() {
      return BuildConfig.DEBUG;
    }

    @Override
    protected List<ReactPackage> getPackages() {
      @SuppressWarnings("UnnecessaryLocalVariable")
      List<ReactPackage> packages = new PackageList(this).getPackages();
      packages.add(new LinkExternalAppPackage()); // 이 부분을 추가한다.
      return packages;
    }

    @Override
    protected String getJSMainModuleName() {
      return "index";
    }
  };

  @Override
  public ReactNativeHost getReactNativeHost() {
    return mReactNativeHost;
  }

  @Override
  public void onCreate() {
    super.onCreate();
    SoLoader.init(this, /* native exopackage */ false);
  }
}
```

주석으로 추가하라고 명시한 부분을 추가해주면 Java로 코드를 작성하는 일은 모두 끝이 난다.

## JavaScript 코드에서 Native Module 사용하기

이제는 JavaScript 코드에서 Native Module을 import해서 사용하면 된다.

```react
import React from "react";
import {
  View,
  Image,
  Platform,
  TouchableOpacity,
  Text,
  NativeModules,
  Linking,
  Alert
} from "react-native";


function openService(service) {
    if (Platform.OS === "android")
      NativeModules.LinkExternalApp.linkExternalApp(packageName);
    else {
      Alert.alert(
        "iOS는 아직 앱 연결이 안되요",
        `${serviceText} 설치 페이지로 이동하실래요?`,
        [
          {
            text: "아뇨",
            onPress: () => console.log("canceled"),
            style: "cancel"
          },
          { text: "네", onPress: () => Linking.openURL(iosLink) }
        ]
      );
    }
  }
}
```

위 코드에 정의된 openService 함수가 Native Module을 사용하는 함수다.

먼저 react-native에서 `import { NativeModules } from "react-native"` 형식으로 NativeModules를 import 한다.

네이티브 모듈은 `NativeModules.LinkExternalApp.linkExternalApp(packageName);` 처럼 사용하면 된다.
NativeModules에 등록된 LinkExternalApp 모듈의 linkExternalApp 함수를 실행하라는 의미이다.

## 마치며

안타깝게도 지금까지 확인한 바로는 iOS에서 Linking과 Custom Scheme을 활용해 앱을 연결하는 방법 외에는 다른 방법이 없다고 한다.
설치된 앱을 실행시키는 방법은 안드로이드에서만 가능한 것이다.

React Native를 이용하다 보면 별별 산전수전을 다 겪게 된다. 그 과정에서 JavaScript를 사용하는 프론트엔드 개발자로서 생각지도 못했던 Java 및 Objective-C 코드 작성도 하게 되는 것 같다.
앞으로 Android 앱 개발을 좀 더 깊이 공부해 보며 네이티브에 대한 이해를 넓혀가려 한다.

## 참고 자료

- [Facebook React Native - Native Modules(Android)](https://facebook.github.io/react-native/docs/native-modules-android)
- [React-Native에서 native module 이용하기](https://medium.com/@kyo504/react-native%EC%97%90%EC%84%9C-native-module-%EC%9D%B4%EC%9A%A9%ED%95%98%EA%B8%B0-d4728337af4e)
