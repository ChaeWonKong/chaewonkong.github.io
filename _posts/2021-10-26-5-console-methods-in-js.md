---
layout: post
title: "5 Console Methods That Makes Your JavaScript Debbuging Life Easier"
date: "2021-10-26"
categories:
  - JavaScript
excerpt: |
  There are other useful console methods that will make your debugging life easier than console.log. Let's learn about console.log with styling, console.table, console.trace, console.count, and console.time

feature_text: |
  ## 5 console methods that makes your JavaScript debbuging life easier
  There are other useful console methods that will make your debugging life easier than console.log. Let's learn about console.log with styling, console.table, console.trace, console.count, and console.time
feature_image: "https://images.unsplash.com/photo-1543966888-7c1dc482a810?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1506&q=80"
image: "https://images.unsplash.com/photo-1543966888-7c1dc482a810?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1506&q=80"
---

`console.log()` is a magical wand when debugging JavaScript codes.

It's easy to use, customizable.

Yet, there are other useful methods that will definitely make your debugging life easier.

Let's find things out.

### 1. console.log() with style

You can add css style to console.log in browser.

```javascript
const user = { name: "Leon", age: 29 };

console.log("%c User", "color: orange; font-weight: bold;");
console.log(user);
```

### 2. console.table()

Sometimes it's useful to present array of data as table.

It's simple like this:

```javascript
const users = [
  { name: "Leon", age: 29 },
  { name: "Jane", age: 27 },
];
console.table(users);
```

### 3. console.trace()

Wanna trace the console log?

Do this:

```javascript
// Coded in Node.js REPL (in terminal)
const doThings = () => {
  // do stuff
  console.trace("doThings doing");
};

doThings();
/*
Trace: doThings doing
    at doThings (REPL33:3:11)
    at REPL34:1:1
    at Script.runInThisContext (vm.js:134:12)
    at REPLServer.defaultEval (repl.js:488:29)
    at bound (domain.js:416:15)
    at REPLServer.runBound [as eval] (domain.js:427:12)
    at REPLServer.onLine (repl.js:821:10)
    at REPLServer.emit (events.js:412:35)
    at REPLServer.emit (domain.js:470:12)
    at REPLServer.Interface._onLine (readline.js:364:10)
*/
```

You can print and trace where the function was defined, how was it called.

### 4. console.count()

Sometimes it is important to know how many times, a function or method has been called.

In that case, `console.count()` will come in handy:

```javascript
const doSomething = () => {
  // do something
  console.count();
};

// Execution
doSomething();
/*
default: 1
undefined
*/

doSomething();
/*
default: 2
undefined
*/
```

It automatically increments `default` everytime the function has been called.

### 5. console.time(), console.timeEnd()

Somethimes, it is useful to know the execution of certain function or method.

`console.time` and `console.timeEnd` can be helpful in that case.

Provide any string that distinguishes the timer.

In the example `'timer'` was provided as param.

It can be 'abcd', 'defg' or what ever else.

Just need to provide the same string to both `console.time` and `console.timeEnd`.

```javascript
const getTime = () => {
  console.time("timer");
  let i = 0;
  while (i < 1000000000) i++;
  console.timeEnd("timer");
};

// Execution
getTime();
// timer: 759.641ms
```
