---
layout: post
title: "React에서 contenteditable을 이용해 글자수에 따라 width가 바뀌는 textfield 만들기"
date: "2020-11-18"
categories:
  - React
excerpt: |
  React에서 input 텍스트가 입력될 때, 텍스트의 길이만큼 점진적으로 width가 늘어나도록 구현해 봅니다.
  contenteditable 속성을 활용하며, 일반 input이나 textarea처럼 placeholder를 넘겨 사용합니다.
  contenteditable로 입력할 수 있는 최대 글자수를 제한하는 방법도 알아봅니다.
feature_text: |
  ## React에서 contenteditable을 이용해 글자수에 따라 width가 바뀌는 textfield 만들기 (글자수 제한 / 너비 자동)
  React에서 input 텍스트가 입력될 때, 텍스트의 길이만큼 점진적으로 width가 늘어나도록 구현해 봅니다.
  contenteditable 속성을 활용하며, 일반 input이나 textarea처럼 placeholder를 넘겨 사용합니다.
  contenteditable로 입력할 수 있는 최대 글자수를 제한하는 방법도 알아봅니다.
feature_image: "https://images.unsplash.com/photo-1489533119213-66a5cd877091?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1951&q=80"
image: "https://images.unsplash.com/photo-1489533119213-66a5cd877091?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1951&q=80"
---

경우에 따라서 텍스트가 입력되는 만큼 텍스트필드의 넓이가 넓어지도록 구현해야할 수가 있습니다.
바로 아래 사진 처럼 말이죠.

![content-editable](https://firebasestorage.googleapis.com/v0/b/codechats.appspot.com/o/blog%2Fcontent-editable.gif?alt=media)

오늘은 `contenteditable` 속성을 이용해,

1. 7글자까지만 입력 가능하고(maxLength),
1. placeholder를 가지며,
1. 작성하는 텍스트의 너비만큼 input 영역의 너비가 확장되는
  
그런 컴포넌트를 구현해 보겠습니다.

## 차례

1. [contenteditable 속성 알아보기](#info)
1. [글자수에 따라 늘어나는 contenteditable 구현](#resize)
1. [contenteditable이 적용된 엘리먼트에 placeholder 추가하기](#placeholder)
1. [contenteditable에 글자수 제한 도입하기](#length-limit)
1. [완성 코드](#complete-code)
1. [참고자료](#ref)

## Contenteditable 속성 알아보기

<span id='info'></span>

HTML5의 모든 엘리먼트는 `contenteditable='true'`로 설정함으로써 해당 엘리먼트 내부에 텍스트를 작성할 수 있게 됩니다.

contenteditable 속성은 열거형(enum) 속성인데, `true`, `false` 이외에도 `inherit`을 가질 수 있습니다.
기본(default)은 부모요소에서 편집가능 여부를 상속받는 `inherit`입니다.

참고로, `HTMLElement.isContentEditable`와 같이 접근하여 HTMLElement의 contenteditable 속성을 불리언(boolean) 값으로도 받아볼 수 있습니다.

즉, `input`, `textarea`와 같은 종래에 텍스트필드로 사용되던 엘리먼트 이외에 `div`, `span` 등의 엘리먼트도 텍스트 필드로 변화시킬 수 있는 것이죠.
> 참고: [MDN Docs](https://developer.mozilla.org/ko/docs/Web/Guide/HTML/Content_Editable)

`contenteditable` 속성은 거의 모든 브라우저에서 폭넓게 지원됩니다.

<a href='https://developer.mozilla.org/ko/docs/Web/HTML/Global_attributes/contenteditable'>
  <img alt='MDN content editable browser support' src='https://firebasestorage.googleapis.com/v0/b/codechats.appspot.com/o/blog%2Fcontenteditable-browser-support.png?alt=media'>
</a>
> 출처: [MDN HTML 웹/HTML/전역 요소/contenteditable](https://developer.mozilla.org/ko/docs/Web/HTML/Global_attributes/contenteditable)

<span id='resize'></span>

## 글자수에 따라 늘어나는 contenteditable 구현

다음으로 입력되는 글자수에 비례해 너비가 늘어나는 contenteditable 엘리먼트를 구현해 보겠습니다.  
편의상 글 최상단에서 보여드린 gif처럼 밑줄로 텍스트 입력 영역을 표현하겠습니다.

```tsx
import React, { Dispatch, useRef, useState } from 'react';
import styled from 'styled-components';

const Input = styled.span`
  display: inline-block;
  font-weight: lighter;
  border-bottom: 1px solid #232323;
  font-size: 34px;
  max-width: calc(100% - 32px);
  min-width: 50px;
`

interface Props{
  value: string;
  onChange: Dispatch<React.SetStateAction<string>>;
}

export const AutoWidthInput = ({
  value,
  onChange,
}: Props) => {

  return (
    <Input
      spellCheck={ false }
      contentEditable
      onInput={
          (e: React.ChangeEvent<HTMLSpanElement>) =>  onChange(e.target.innerText);
        }
    />
  );

  return (
    <div>
      <Input
        placeholder='이름 또는 애칭'
        spellCheck={ false }
        contentEditable
        onInput={
            (e: React.ChangeEvent<HTMLSpanElement>) =>  onChange(e.target.innerText);
          }
      />
    </ div>
  )
};
```
  
우선 다른 컴포넌트에서 input의 change와 value를 control하도록 만들어 보았습니다.

<details><summary>`div`로 요소를 감싼 이유</summary>
  <pre><code>
  `div`로 요소를 감싼 이유는, 해당 컴포넌트가 `display: flex`가 적용된 부모요소의 자식요소로 사용되더라도 `flex`가 적용되지 않도록 하기 위함입니다.
  `div`로 감싸지 않고 사용할 경우, 부모요소가 `display: flex`라면, 이에 영향을 받아 width가 full로 차게 되어, 입력 컨텐츠에 비례해 커지지 않습니다.
  </code></pre>
</details>
<br />

즉, `value`와 `onChange`를 props로 받는 것이죠. contenteditable로 설정된 엘리먼트의 `innerText` 값이 변경될 경우,
`props.onChange`가 동작해 해당 컴포넌트의 부모 컴포넌트가 변경사실을 관리하게 됩니다.

React에서 `contenteditable={ true }`의 경우 `contenteditable`로만 놓아도 기본값(default value)으로 `true`가 들어가므로,
속성값에는 `contenteditable`만 작성하면 됩니다.

`spellCheck` 속성은 오타가 있을 경우 붉은 물결 밑줄을 표시할 것인지를 결정합니다. 기본값(default value)은 `true`입니다.

제 경우, 사용자의 닉네임을 입력받는 텍스트 필드이기 때문에, 오타를 검증하지 않도록 `spellCheck={ false }`로 설정했습니다.

**`onInput` 속성으로 입력값을 관리합니다. contenteditable이 설정된 요소(element)의 innerText(혹은 innerHTML)에 사용자가 "쓰기"를 진행할 경우 발생되는 이벤트입니다.**

TypeScript를 사용하고 있으므로 onInput에도 타입을 적절히 지정해 줍니다.
span 엘리먼트를 이용하고 있으므로, `React.ChangeEvent<HTMLSpanElement>`가 Event의 타입이 됩니다.

<span id='placeholder'></span>
  
## contenteditable이 적용된 엘리먼트에 placeholder 추가하기

다음으로 placeholder를 사용하는 방법을 알아보겠습니다.
  
input이나 textarea와는 달리 **contenteditable 속성을 `true`로 주어도 placeholder 속성이 바로 적용되지는 않습니다.**
  
별도로 설정해줘야만 하는 것이죠.

해당 엘리먼트의 CSS에 다음 속성을 추가해 주어야 합니다.

```css
// placeholder
&:empty:before {
  content: attr(placeholder);
  color: grey;
  display: inline-block;
};
```

또, contenteditable이 설정된 엘리먼트에 placeholder도 추가해 줘야 하죠.
  
`placeholder='이름 또는 애칭'`을 JSX 요소의 속성으로 추가해 줍니다.

즉, 우리의 코드는 아래처럼 바뀌게 됩니다.

```tsx
import React, { Dispatch, useRef, useState } from 'react';
import styled from 'styled-components';

const Input = styled.span`
  display: inline-block;
  font-weight: lighter;
  border-bottom: 1px solid #232323;
  font-size: 34px;
  max-width: calc(100% - 32px);
  min-width: 50px;

  // placeholder
  &:empty:before {
    content: attr(placeholder);
    color: grey;
    display: inline-block;
  };
`

interface Props{
  value: string;
  onChange: Dispatch<React.SetStateAction<string>>;
}

export const AutoWidthInput = ({
  value,
  onChange,
}: Props) => {

  return (
    <div>
      <Input
        placeholder='이름 또는 애칭'
        spellCheck={ false }
        contentEditable
        onInput={
            (e: React.ChangeEvent<HTMLSpanElement>) =>  onChange(e.target.innerText);
          }
      />
    </ div>
  )
};
```

<span id='length-limit'></span>
  
## contenteditable에 글자수 제한 도입하기

마지막으로 입력 가능한 글자수에 제한을 두어 보겠습니다.

이 부분은 살짝 hacky한 해결책을 사용했습니다.
ref를 이용해 contenteditable이 적용된 요소를 다루고, value를 넘겨주는 것은 조금 까다롭습니다.

제 스스로는 아직 best practice를 찾지 못한 상황이고, **미리 지정한 글자수를 초과할 경우, 입력 자체를 비활성화 하는 방법으로 대응**해 사용하고 있습니다.

먼저 ref를 사용해야겠죠. `useRef`를 import해주고, 아래 코드를 추가해 줍니다.  
`const ref = useRef<HTMLSpanElement>(null);`

또, `editable: boolean`을 state에서 관리하도록 `useState`도 가져옵니다.

```tsx
import React, { Dispatch, useRef, useState } from 'react';

export const AutoWidthInput = ({
  value,
  onChange,
}: Props) => {
// ...
  const [editable, setEditable] = useState(false);
  const ref = useRef<HTMLSpanElement>(null);

// ...
}
```

`editable` 속성이 `true`일 경우에만 contenteditable이 활성화되어 텍스트 입력이 가능하도록 설정할 것입니다.

다음으로, 조건부로 입력값을 다뤄줄 event-handler를 작성해 보겠습니다.

```tsx
// 최대 입력가능한 글자수
const MAX_LENGTH = 7

// onInput handler
const handleInputEvent = (e: React.ChangeEvent<HTMLSpanElement>) => {
  const newValue = e.target.innerText;
  if (newValue.length >= MAX_LENGTH) {
    setEditable(false);
  }
  onChange(newValue);
};
```
  
length가 `MAX_LENGTH` 보다 커질 경우, `setEditable(false)`를 호출합니다.
  
다음으로 contenteditable이 설정된 엘리먼트의 contenteditable 속성의 값을 `contenteditable={ true }`에서 `contenteditable={ editable }`로 변경합니다.

더불어 `onInput={ handleInputEvent }`으로 변경도 진행해 줍니다.

```tsx
export const AutoWidthInput = ({
  value,
  onChange,
}: Props) => {
  const [editable, setEditable] = useState(false);
  const ref = useRef<HTMLSpanElement>(null);
  
  // 최대 입력가능한 글자수
  const MAX_LENGTH = 7

  // onInput handler
  const handleInputEvent = (e: React.ChangeEvent<HTMLSpanElement>) => {
    const newValue = e.target.innerText;
    if (newValue.length >= MAX_LENGTH) {
      setEditable(false);
    }
    onChange(newValue);
  };

  return (
    <div>
      <Input
        ref={ ref } // 아까 추가한 ref를 여기에 추가해 줍니다.
        placeholder='이름 또는 애칭'
        spellCheck={ false }
        contentEditable={ editable}
        onInput={ handleInputEvent }
      />
    </ div>
  );
}
```
  
여기서 문제가 있죠. `contenteditable={ true }`로 설정해주는 방법이 필요하겠죠. 결국 우리가 원하는 것은 사용자로부터 텍스트 입력을 받는 것이니까요.

엘리먼트를 클릭하면, `contenteditable={ true }`가 되도록 `onClick` 이벤트를 추가하겠습니다.

`onClick={ () => setEditable(true) }`를 wrapper로 사용된 div 엘리먼트에 추가해 줍니다.
  
```tsx
export const AutoWidthInput = ({
  value,
  onChange,
}: Props) => {
  // ...

  return (
    <div onClick={ () => setEditable(true) }>
      <Input
        ref={ ref }
        placeholder='이름 또는 애칭'
        spellCheck={ false }
        contentEditable={ editable}
        onInput={ handleInputEvent }
      />
    </ div>
  );
}
```
  
<span id='complete-code'></span>
  
### 완성 코드

최종적인 코드는 아래와 같습니다.
  
```tsx
import React, { Dispatch, useRef, useState } from 'react';
import styled from 'styled-components';

const Input = styled.span`
  display: inline-block;
  font-weight: lighter;
  border-bottom: 1px solid #232323;
  font-size: 34px;
  max-width: calc(100% - 32px);
  min-width: 50px;

  // placeholder
  &:empty:before {
    content: attr(placeholder);
    color: grey;
    display: inline-block;
  };
`

interface Props{
  value: string;
  onChange: Dispatch<React.SetStateAction<string>>;
}

export const AutoWidthInput = ({
  value,
  onChange,
}: Props) => {
  const [editable, setEditable] = useState(false);
  const ref = useRef<HTMLSpanElement>(null);
  
  // 최대 입력가능한 글자수
  const MAX_LENGTH = 7

  // onInput handler
  const handleInputEvent = (e: React.ChangeEvent<HTMLSpanElement>) => {
    const newValue = e.target.innerText;
    if (newValue.length >= MAX_LENGTH) {
      setEditable(false);
    }
    onChange(newValue);
  };

  return (
    <div onClick={ () => setEditable(true) }>
      <Input
        ref={ ref }
        placeholder='이름 또는 애칭'
        spellCheck={ false }
        contentEditable={ editable}
        onInput={ handleInputEvent }
      />
    </ div>
  );
};
```

## 참고자료

<span id='ref'></span>

- [MDN Docs](https://developer.mozilla.org/ko/docs/Web/Guide/HTML/Content_Editable)
- [MDN HTML 웹/HTML/전역 요소/contenteditable](https://developer.mozilla.org/ko/docs/Web/HTML/Global_attributes/contenteditable)
- [StackOverflow - contenteditable change events](https://stackoverflow.com/questions/1391278/contenteditable-change-events/6263537)
- [StackOverflow - React contentEditable and cursor position](https://stackoverflow.com/questions/45306325/react-contenteditable-and-cursor-position)
