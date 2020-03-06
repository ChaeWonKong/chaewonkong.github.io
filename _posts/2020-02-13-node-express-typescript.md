---
layout: post
title: "How to Set up Node/Express server with TypeScript"
date: "2020-02-13"
categories:
  - NodeJS
excerpt: |
  Setting Up server that uses Node/Express.js with TypeScript.
feature_text: |
  ## How to Set up Node/Express server with TypeScript
  Setting Up server that uses Node/Express.js with TypeScript.
feature_image: "https://images.unsplash.com/photo-1504192010706-dd7f569ee2be?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1951&q=80"
image: "https://images.unsplash.com/photo-1504192010706-dd7f569ee2be?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1951&q=80"
---

## Installation

You must install TypeScript globally.

```shell
$ npm install -g typescript
```

If you got an permission error, try `sudo` prefix.

## Project Initialization

Now, let's create an npm project.

```shell
$ mkdir ts-node-server
$ cd ts-node-server
$ npm init -y
```

This will create `package.json` and other setting files.

Then run,

```shell
$tsc --init
```

to create TypeScript environment.
`tsconfig.json` file will be initialized then, and you can configure settings for TypeScript.

## Express

Now is the time to create express app.

First, we need to install `@types/express`.

```shell
$ npm i @types/express
```

Create `index.ts` file to store code for your server, and type the code below.

```typescript
import express from "express";

declare global {
  namespace NodeJS {
    interface ProcessEnv {
      NODE_ENV: string;
      PORT?: string;
    }
  }
}

class App {
  public application: express.Application;
  constructor() {
    this.application = express();
  }
}

const app = new App().application;
const PORT = process.env.PORT || 4000;

// show "Successfully started" on browser when you visit localhost:4000
app.get("/", (req: express.Request, res: express.Response) => {
  res.send("Successfully started");
});

// show "Server is Running at >>> localhost:4000" on your terminal.
app.listen(PORT, () =>
  console.log(`Server is Running at >>> localhost:${PORT}`)
);
```

you may import express like `import * as express from "express"`, if you got an error from module imports.

We use `process.env.PORT` as a port if it exists, else use 4000.
In order to use this method, we need to declare namespace and interface globally by using `declare global` code block.

Then we created `App` class to generate express app.

## Run

In order to watch updates in your code while running the server, we need to use `nodemon`.

```shell
$ npm i -D nodemon ts-node
```

The `-D` flag implies the followed modules would be added to `devDependencies`.

Almost done. Add `start` script to `package.json`.

Your `package.json` would like:

```json
{
  "name": "server",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "start": "nodemon index.ts"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "nodemon": "^2.0.2",
    "ts-node": "^8.6.2"
  },
  "dependencies": {
    "@types/express": "^4.17.2"
  }
}
```

Finally, it's time to run your server.

Type `npm run start`, and go to "http://localhost:4000".

You will see "Successfully started", then you made it!

## Reference

- [StackOverflow, "using process.env in TypeScript"](https://stackoverflow.com/questions/45194598/using-process-env-in-typescript/45195359)
- [Ali GOREN, "Developing an Express Application Using TypeScrip"](https://dev.to/aligoren/developing-an-express-application-using-typescript-3b1)
