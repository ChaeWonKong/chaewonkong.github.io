---
layout: post
title: "React Native - Android Build Failed for app:transformClassesAndResourcesWithR8ForRelease Error"
date: "2019-10-06"
categories:
  - React-Native
excerpt: |
  React Native - [Android Build Failure] Execution failed for task ':app:transformClassesAndResourcesWithR8ForRelease'. com.android.tools.r8.CompilationFailedException; Compilation failed to complete
feature_text: |
  ## React Native - Android Build Failed for app:transformClassesAndResourcesWithR8ForRelease Error
  React Native - [Android Build Failure] Execution failed for task ':app:transformClassesAndResourcesWithR8ForRelease'. com.android.tools.r8.CompilationFailedException; Compilation failed to complete
feature_image: "https://images.unsplash.com/photo-1543409742-080d01661481?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2850&q=80"
image: "https://images.unsplash.com/photo-1543409742-080d01661481?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2850&q=80"
---

When Building Android App from React Native CLI, I got **error**:

```cmd
> Task :app:transformClassesAndResourcesWithR8ForRelease
R8 is the new Android code shrinker. If you experience any issues, please file a bug at
https://issuetracker.google.com, using 'Shrinker (R8)' as component name. You can
disable R8 by updating gradle.properties with 'android.enableR8=false'.
Current version is: 1.4.94 (build 390954928f0db9c3b888a367f7f128ce3bbfb160 from go/r8bot (luci-r8-ci-archive-0-5g74)).

D8: Attribute Signature requires InnerClasses attribute. Check -keepattributes directive.

> Task :app:transformClassesAndResourcesWithR8ForRelease FAILED

FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:transformClassesAndResourcesWithR8ForRelease'.
> com.android.tools.r8.CompilationFailedException: Compilation failed to complete
```

|     Dependency     | Version |
| :----------------: | :-----: |
|    react-native    | 0.60.4  |
|   google-service   |  4.3.2  |
| firebase-analytics | 17.2.0  |

## Possible Solutions

### Removing build files and caches

```cmd
cd adnroid && ./gradlew clean
./gradlew assembleRelease
cd ..
```

It is the easiest way for the most of build failures, yet in my case it didn't work.

### Sync Project With Gradle File in Android studio

Open the project in Android Studio and click **"Sync Project With Gradle File"** button. Since I deleted node_modules and yarn.lock for previus iOS build failure, I thought I might have to sync my gradle file again.

It didn't work.

### Jetify

Since, I removed and reinstalled dependencies, I ran `yarn jetify`.
It didn't make build successful.

### Disable R8

Build error seems to be related to R8, so I manually added `android.enableR8 = false` to android/gradle.properties file.
Yet I encountered another build error:

```cmd
FAILURE: Build failed with an exception.

* What went wrong:
Execution failed for task ':app:transformClassesWithDexBuilderForRelease'.
> com.android.build.api.transform.TransformException: java.lang.ArrayIndexOutOfBoundsException
```

### Downgrade Google Services Version

The latest dependency I added was things related to firebase-analytics. So I tried to check whether some of versions might not working with react-native project.

|     Dependency     | Version |
| :----------------: | :-----: |
|    react-native    | 0.60.4  |
|  google-services   |  4.3.2  |
| firebase-analytics | 17.2.0  |

So I commented each dependencies related to firebase, and test building. I found that **"google-services"** is the curprit. **Downgrading "google-services version from 4.3.2 to 4.3.1 solved my build error.**
