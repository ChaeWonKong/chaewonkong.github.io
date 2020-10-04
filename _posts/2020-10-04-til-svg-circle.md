---
layout: post
title: "[TIL] SVG Circles"
date: "2020-10-04"
categories:
  - TIL
excerpt: |
  Use SVG Circles in React. Find out what exactly height, width, cx, cy, r, stroke, and fill attributes are working on.
feature_text: |
  ## Create SVG Circles Expertly
  Create Color Palette with SVG and React. Find out what exactly height, width, cx, cy, r, stroke, and fill attributes are working on.
feature_image: "https://images.unsplash.com/photo-1583703779168-9b659c61d0b7?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1993&q=80"
image: "https://images.unsplash.com/photo-1583703779168-9b659c61d0b7?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1993&q=80"
---

SVGs are very handy -- you can manipulate icons, images, figures, and graphs by programmatically.

Today, I learned how to create a color palette with several color chips by using SVG and React.

## Basics
```html
<svg height="16" width="16">
  <circle cx="8" cy="8" r="6" stroke="black" stroke-width="2" fill="black" />
</svg>
```

1. `height` and `width` represents size of the canvas; the area that is covered by generated svg.
2. `cx` and `cy` refers center of the circle. By default `cx` and `cy` is set to 0.
3. `r` is an abbreviation of radius.
4. `stroke` and `stroke-width` decides outer border of the circle.
5. `fill` handles inner background color of the circle.

In the example above, I deliberatly set each numbers.

First, I set the size to 16 * 16, so, the center point would be (8, 8).
That's why `cx` and `cy` equals to 8.

Then, I set `stroke-width` to 2, which means, the actual radius of the circle should equal to `r + 2`(Since, border should be counted).

So, the `r` should be 6, in order to present the whole circle in the 16*16 canvas without any cut-offs.

To sum up,
```
cx = cy = width / 2
r = (width / 2) - strokeWidth
```

## Implement in React

Use SVGs in React is easy.
```tsx
import React from 'react';
import styled from 'styled-components';

export const TEXT_COLORS = [
  '#000000',
  '#FFFFFF',
  '#F4511E',
  '#FBC02D',
  '#00C853',
  '#0091EA',
  '#00B8D4',
  '#00695C',
  '#AA00FF',
  '#7B1FA2',
];

export const Wrapper = styled.div`
  display: flex;
  justify-content: center;
  grid-gap: 10px;
  align-items: center;
  width: 100%;
  margin-left: -16px;
  height: 56px;
  position: absolute;
  bottom: 70px;
  padding: 0 20px;
`;

export const ColorSelector = () => {
  const renderColorchips = () => TEXT_COLORS.map((color: string) => (
      <svg
        key={ color }
        width={ 24 }
        height={ 24 }
      >
        <circle
          cx={ 12 }
          cy={ 12 }
          r={ 10 }
          fill={ color }
          stroke='white'
          strokeWidth={ 2 }
        />
      </svg>
    );
  ));

  return (
    <Wrapper>
      { renderColorchips() }
    </Wrapper>
  );
};
```

Since, it's React JSX/TSX not HTML, be sure to make all properties camelCased.
`stroke-width` should be `strokeWidth`.

`width`, `height`, `cx`, `r`, `strokeWidth` could be either string or number type.

The result would be like:
![color palette](https://raw.githubusercontent.com/ChaeWonKong/image-resource/master/palette.png)

The thing is, I added more features, like `onClick`.
But those are beyond of this post's topic.
