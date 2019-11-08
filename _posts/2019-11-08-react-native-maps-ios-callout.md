---
layout: post
title: "React Native - react-native-maps text not visible in default info window(callout)"
date: "2019-11-08"
categories:
  - React-Native
excerpt: |
  When using react-native-maps with google maps in iOS, the white background info window(callout) shows no text in dark mode. Here's solution.
feature_text: |
  ## React Native - react-native-maps text not visible in default info window(callout)
  When using react-native-maps with google maps in iOS, the white background info window(callout) shows no text in dark mode. Here's solution.
feature_image: "https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/typescript.png"
image: "https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/typescript.png"
---

Recently I faced an issue that iOS callouts(info windows) that not showing any text.
White background callout with white text.

<div style="display: flex">
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/ios_callout.png?raw=true" width="32%" />
<img src="https://github.com/ChaeWonKong/image-resource/blob/master/android_callout.png?raw=true" width="32%" />
</div>

Just like left side pic shows.

Here's the code:
```jsx
  <MapView>
    <Marker
      ref={ref => {
        this.marker = ref;
      }}
      coordinate={coords}
      title={title}
      description={description}
    ></Marker>
  </MapView>
  ```

In general, Marker with title and description props automatically shows callout with white background and black text.
In android and iOS below version 13, it is true.
But, **in iOS >= v.13 especially in dark mode, the white background callout seems to show white text.**

## Initial Solution: Adding Custom Markers
My initial solution was adding a whole-customed markers inside the <MapView />.

```jsx

<Marker
          ref={ref => {
            this.marker = ref;}}
          coordinate={coords}
      >
          <View style={styles.container}>
            <View
              style={styles.callout}
            >
              <Text style={style.titleText}>
                {title}
              </Text>
            </View>
            <Text>
              <Icon name="ios-pin" color="red" size={36}></Icon>
            </Text>
          </View>
        </Marker>
```

I added custom icon as well.
Then, the main problem has solved. Callout finally shows black text.
Yet, another problem occured.

**The position of the marker pin was unstable when zooming in or out. The pin moves a lot when the user zoom in or zoom out the map.** The movement of marker pin was so obvious that any user can tell it.

So, **this solution is not recommanded.**

## Final Solution: Using Custom Callout
react-native-maps offers <Callout /> API, yet the documentation is not that friendly.

So, I had to open up the node_modules/react-native-maps and study through the example codes.

Here's final, final solution:

```jsx
<Marker
                pinColor="red"
                ref={ref => {
                  this.marker = ref;
                }}
                coordinate={coords}
                title={title}
                description={description}
              >
                {Platform.OS === "ios" ? (
                  <Callout
                    tooltip={true}
                    style={styles.callout}
                  >
                    <Text style={styles.title}>
											{title}
                    </Text>
                    <Text style={styles.description}>
                      {description}
                    </Text>
                  </Callout>
                ): null }
</Marker>
```

Here's my style:

```jsx
import { fonts } from "../Styles/_mixin";


const styles = StyleSheet.create({
callout: {
                  backgroundColor: "white",
                  borderRadius: 4,
                  alignItems: "center",
                  justifyContent: "center",
                  padding: 4
                },
title: {
                    color: "black",
                    fontSize: 14,
                    lineHeight: 18,
                    flex: 1,
                    fontFamily: fonts.spoqaHanSansRegular
                  },
description: {
                    color: "#707070",
                    fontSize: 12,
                    lineHeight: 16,
                    flex: 1,
                    fontFamily: fonts.spoqaHanSansRegular
                  }
})
```

Until react-native-maps library officially supports dark mode in iOS, this solution might help.