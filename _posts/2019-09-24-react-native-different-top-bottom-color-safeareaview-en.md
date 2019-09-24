---
layout: post
title: "React Native - iOS Set Different Colors to Top and Bottom of SafeAreaView"
date: "2019-09-24"
categories:
  - React-Native
excerpt: |
  React Native - iOS Set Different Colors to Top and Bottom of SafeAreaView
  Set different colors to top and bottom padding of SafeAreaView(change default padding color of white to else).
feature_text: |
  ## React Native - iOS Set Different Colors to Top and Bottom of SafeAreaView
  Set different colors to top and bottom padding of SafeAreaView(change default padding color of white to else).
feature_image: "https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
image: "https://images.unsplash.com/photo-1512941937669-90a1b58e7e9c?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1350&q=80"
---

{% raw %}
SafeAreaView only works on iOS devices with version 11 or later. It automatically applies paddings to its nested child views. It considers physical limitation of screen differ from devices to devices.

## Set different colors to top and bottom of SafeAreaView

When creating app, sometimes it is needed to apply different colors to top and bottom SafeAreaView padding. Let's find it out how.

### Requirements

```
react >= 16.2.0
```

We are gonna use Fragment imported from React. Fragment is available in React version above 16.2.0.

Default usage of SafeAreaView will like:

```react
import React, { Component } from "react";
import { SafeAreaView, View, Text } from "react-native";

export default class App extends Component {
  render() {
    return (
      <SafeAreaView style={{ flex: 1 }}>
        <View style={{ flex: 1, backgroundColor: "#25365d" }}>
          <Text style={{ fontSize: 30, color: "#fff" }}>Text</Text>
        </View>
      </SafeAreaView>
    );
  }
}
```

The code above will look like:

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/safearea1.png?raw=true" height="450px"/>

In the root component, wrap two SafeAreaView with different colors with Fragment imported from React:

```react
import React, { Component, Fragment } from "react";
import { SafeAreaView, Platform, View, StatusBar, Text } from "react-native";

export default class App extends Component {
  render() {
    if (Platform.OS === "ios")
      return (
        <Fragment>
          <SafeAreaView style={{ flex: 0, backgroundColor: "red" }} />
          <StatusBar barStyle="light-content" />
          <SafeAreaView style={{ flex: 1, backgroundColor: "blue" }}>
            <View style={{ flex: 1, backgroundColor: "#25365d" }}>
              <Text style={{ fontSize: 30, color: "#fff" }}>Text</Text>
            </View>
          </SafeAreaView>
        </Fragment>
      );
    else
      return (
        <View style={{ flex: 1, backgroundColor: "#25365d" }}>
          <Text style={{ fontSize: 30, color: "#fff" }}>Text</Text>
        </View>
      );
  }
}
```

StatusBar component sets text color of top-side status bar containing time, battery and network connection status.
Let's change StatusBar text color to white:

```
barStyle="light-content"
```

Let's see the implementation.

<img src="https://github.com/ChaeWonKong/image-resource/blob/master/safearea2.png?raw=true" height="450px"/>

## Conclusion

You can use Fragment from React to wrap your SafeAreaView component without affecting UI or JavaScript logic yet can change top or bottom color of SafeAreaView's padding.

## Reference

[React Native: Set different colors on Top and Bottom in SafeAreaView component](https://medium.com/reactbrasil/react-native-set-different-colors-on-top-and-bottom-in-safeareaview-component-f008823483f3)

{% endraw %}
