---
layout: post
title: "React Native - iOS Error: Could not parse the simulator list output."
date: "2019-11-12"
categories:
  - React-Native
excerpt: |
  iOS debug Build Error - Could not parse the simulator list output.
feature_text: |
  ## React Native - iOS Error: Could not parse the simulator list output.
  iOS debug Build Error - Could not parse the simulator list output.
feature_image: "https://images.unsplash.com/photo-1534802046520-4f27db7f3ae5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2001&q=80"
image: "https://images.unsplash.com/photo-1534802046520-4f27db7f3ae5?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=2001&q=80"
---

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/xcode1.png?raw=true" width="100%" />

| Environments | Version |
| :----------: | :-----: |
| react-native | 0.60.4  |
|    Xcode     |  11.2   |

Recently, I updated Xcode from 11.1 to 11.2 and faced error like:

```shell
info Found Xcode workspace "Native_Makkcha.xcworkspace"
2019-11-12 11:55:22.239 simctl[12139:74526] CoreSimulator detected version change.  Framework version (681.15) does not match existing job version (681.5.4).  Attempting to remove the stale service in order to add the expected version.
error Could not parse the simulator list output. Run CLI with --verbose flag for more details.
SyntaxError: Unexpected token I in JSON at position 0
    at JSON.parse (<anonymous>)
    at runOnSimulator (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/@react-native-community/cli-platform-ios/build/commands/runIOS/index.js:128:23)
    at Object.runIOS [as func] (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/@react-native-community/cli-platform-ios/build/commands/runIOS/index.js:97:12)
    at Command.handleAction (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/@react-native-community/cli/build/cliEntry.js:160:21)
    at Command.listener (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/commander/index.js:315:8)
    at Command.emit (events.js:198:13)
    at Command.parseArgs (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/commander/index.js:651:12)
    at Command.parse (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/commander/index.js:474:21)
    at setupAndRun (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/@react-native-community/cli/build/cliEntry.js:210:24)
    at Object.run (/Users/leonkong/Desktop/dev/CodingHippo/Native_Makkcha/node_modules/@react-native-community/cli/build/cliEntry.js:173:11)
```

## Solution
Solution is simple.
Just **open up [YourProjectName].xcworkspace file, then Xcode will automatically install some components.**

My error disappeared automatically.

## Possible Error
After opening up my .xcworkspace file, ```Could not parse the simulator list output``` error finally disappeared.

Yet, I encountered another error:

```shell
info Found Xcode workspace "Native_Makkcha.xcworkspace"
error Could not find "iPhone X" simulator. Run CLI with --verbose flag for more details.
```

This error has occured since Xcode ver. 11.1 because of **mis-match of React Native's target build device and Xcode's device name.**

You can see detail here: [React Native - Could Not Find 'iPhone X' Error
](https://chaewonkong.github.io/posts/xcode-iphonex-not-found.html)

So, I manually added "iPhone X" in Xcode.
Here's how:

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/xcode2.png?raw=true" width="100%" />
See "+" button in left bottom corner? Press the button and just add "iPhone X" like picture before.
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/xcode3.png?raw=true" width="100%" />

## Reference
- [StackOverFlow, "Hello, World! program for React Native throws “Could not parse the simulator list output” on Mac"](https://stackoverflow.com/questions/39810470/hello-world-program-for-react-native-throws-could-not-parse-the-simulator-lis)