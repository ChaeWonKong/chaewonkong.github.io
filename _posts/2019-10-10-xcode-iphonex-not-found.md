---
layout: post
title: "React Native - Could Not Find 'iPhone X' Error"
date: "2019-10-10"
categories:
  - React-Native
excerpt: |
  Xcode Update Bug on React Native - error Could not find "iPhone X" simulator. Run CLI with --verbose flag for more details.
feature_text: |
  ## React Native - Could Not Find 'iPhone X' Erro
  Xcode Update Bug on React Native - error Could not find "iPhone X" simulator. Run CLI with --verbose flag for more details.
feature_image: "https://images.unsplash.com/photo-1508921234172-b68ed335b3e6?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
image: "https://images.unsplash.com/photo-1508921234172-b68ed335b3e6?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1950&q=80"
---

| Environments | Version |
| :----------: | :-----: |
| react-native | 0.60.4  |
|    Xcode     |  11.1   |

After updating Xcode to Version 11.1 (11A1027) I encountered 'iPhone X' not found error when running ```react-native run-ios```.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/iphonex1.png?raw=true" width="100%" />

```cmd
$ react-native run-ios

> error Could not find "iPhone X" simulator. Run CLI with --verbose flag for more details.
```

It's mainly because **new Xcode version does not have 'iPhone X' anymore.**
Actually iPhone model name has been slightly changed. 'iPhone Xs' Became 'iPhone 11 Pro'. Some of other recent models has simmilar changes.

## Possible Solutions
### Specifying Device
When running ```react-native run-ios``` command, you can specify device that exists.

```cmd
react-native run-ios --simulator="iPhone 11 Pro"
```

It is the most simpleast solution, yet it's quite annoying, **since you have to type longer command.**

### Change Default Virtual Device
You can change default running virtual device in node_modules.
Open up "/node_modules/@react-native-community/cli-platform-ios/build/commands/runIOS/index.js" in your text editor.

```js
{
    ...
options: [{
    name: '--simulator [string]',
    description: 'Explicitly set simulator to use. Optionally include iOS version between' + 'parenthesis at the end to match an exact version: "iPhone 6 (10.0)"',
    default: 'iPhone X'
  },
          ...
          ]
}
```
See "default"? Just change it to something like "iPhone 11 Pro" or else.

The problem is, **whenever you install node_modules again, it goes back to "iPhone X".**

## Recommanded Solution
It is kind a tricky solution but easy and certain. Just add "iPhone X" to your Xcode.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/iphonex2.png?raw=true" width="100%" />
Press Button marked as in image above.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/iphonex3.png?raw=true" width="100%" />
Then, press plus button.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/iphonex4.png?raw=true" width="100%" />
Add "iPhone X".

## References
- [StackOverflow](https://stackoverflow.com/questions/54504076/react-native-run-ios-returns-error-could-not-find-iphone-x-simulator)