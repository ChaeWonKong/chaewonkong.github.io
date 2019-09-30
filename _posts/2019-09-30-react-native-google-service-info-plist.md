---
layout: post
title: "React Native - could not find a valid GoogleService-Info.plist in your project"
date: "2019-09-30"
categories:
  - React-Native
excerpt: |
  react-native-firebase error - could not find a valid GoogleService-Info.plist in your project
feature_text: |
  ## React Native - could not find a valid GoogleService-Info.plist in your project
  react-native-firebase error - could not find a valid GoogleService-Info.plist in your project
feature_image: "https://images.unsplash.com/photo-1560472354-b33ff0c44a43?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2620&q=80"
image: "https://images.unsplash.com/photo-1560472354-b33ff0c44a43?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2620&q=80"
---

I got **"could not find a valid GoogleService-Info.plist in your project."** in xcworkspace file while building app after adding react-native-firebase to my project.

```
could not find a valid GoogleService-Info.plist in your project.
```

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/googleService1.png?raw=true" height="450px" />

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/googleService3.png?raw=true" height="450px" />
Error that occured.

## How To Reproduce

1. Install react-native-firebase

```cmd
$ yarn add react-native-firebase
```

2. Create Firebase iOS App Project
3. Manually Add GoogleService-Info.plist to "ios/[project-name]/" Directory.
4. Add Firebase to Podfile

```cmd
$ pod 'Firebase/Analytics'
```

5. Build and Launch App

## Solution

1. Open up project with "[project_name].xcodproj (Not [project_name].xcworkspace)
   <img src="https://github.com/ChaeWonKong/image-resource/blob/master/googleService2.png?raw=true" height="450px" />

2. Right Click to Your Project Directory in Left Tab.
3. Click "Add Files to [project_name]" and Add "GoogleService-Info.plist"
   <img src="https://github.com/ChaeWonKong/image-resource/blob/master/googleService5.png?raw=true" height="450px" />

4. You Will See "GoogleService-Info.plist" in Build Phases Tab in Copy Bundle Resources.
   <img src="https://github.com/ChaeWonKong/image-resource/blob/master/googleService4.png?raw=true" height="450px" />

5. Clean Up Build and Re-build it.

You will find no error then. Problem solved.

## References

- [Stack-Overflow](https://stackoverflow.com/questions/45317777/could-not-find-a-valid-googleservice-info-plist-in-your-project)
