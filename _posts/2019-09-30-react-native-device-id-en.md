---
layout: post
title: "React Native - Get Device Unique ID Using react-native-device-info"
date: "2019-09-30"
categories:
  - React-Native
excerpt: |
  Get Device Unique ID by Using react-native-device-info in React Native
feature_text: |
  ## React Native - Get Device Unique ID Using react-native-device-info
  Get Device Unique ID by Using react-native-device-info in React Native
feature_image: "https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
image: "https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
---

Sometimes it is necessary to find out device's unique identifier. 
There's a handy library that helps you to do that.

**[react-native-device-info](https://github.com/react-native-community/react-native-device-info)** will do that for you.

## Installation
If you are using React Native >= 0.60:
```
yarn add react-native-device-info
cd ios && pod install && cd ..
```

The code above will automatically links the library to your project.
There's no need to link it, or manually add codes to Podfile.

## Introduction
This library surely gives a lot of handly methods to get infos from devices. Let's see some of it.

- getUniqueId(): It gives you unique identifier of each device both in Android and iOS.
- getBatteryLevel(): It returns battery level of the device.
- getDeviceId(): It returns something like this: "iPhone7, 2" or "goldfish" for Android
- getIpAddress(): It returns IP address.

## Usage

You can use DeviceInfo.getUniqueId() to handle device's unique id.

```react
import DeviceInfo from 'react-native-device-info';

DeviceInfo.getUniqueId().then(uniqueId => {
  console.log(uniqueId)
  // iOS: "FCDBD8EF-62FC-4ECB-B2F5-92C9E79AC7F9"
  // Android: "dd96dec43fb81c97"
});
```

Note that "getUniqueId()" returns a promise.

## Reference
- [react-native-device-info](https://github.com/react-native-community/react-native-device-info)