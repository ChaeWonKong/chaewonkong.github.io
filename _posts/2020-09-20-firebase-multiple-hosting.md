---
layout: post
title: "Separate development/production deployment by using Firebase Multisite hosting"
date: "2020-09-20"
categories:
  - DevLog
excerpt: |
  Using Firebase Hosting's multisite hosting, able to separate development from production. By implementing GitHub Action, deploy process to each namespace could be automated.
feature_text: |
  ## Separate development/production deployment to different URL with same project.
  Using Firebase Hosting's multisite hosting, able to separate development from production. By implementing GitHub Action, deploy process to each namespace could be automated.
feature_image: "https://images.unsplash.com/photo-1532562145520-b8cce2486cd2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1934&q=80"
image: "https://images.unsplash.com/photo-1532562145520-b8cce2486cd2?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1934&q=80"
---

Developing an production-level service requires whole new bunch of things to ensure production-level quality.

Consumers, or users are hot-tempered. They don't tolerate errors, bugs, and waitings.

Quality assurance(QA) becomes is much more important compared to personal toy projects.

Separating development/production deployment offers benefits a lot.

You can **fearlessly deploy to development environment and test your development, without contaminating production environment.**
Plus, **there are some features that could only be tested after deployment**, such as CI/CD configurations and OAuth implementations.

Today, let's find out how to **separate dev/prod deployment by using Firebase hosting**.

## TL;DR
Use [multisite hosting](https://firebase.google.com/docs/hosting/multisites).
If you already have a project deployed to Firebase hosting, just make sure to login again right after implementing multisite configurations.

1. Add another site in Firebase console.
1. Apply sites by using Firebase CLI: `firebase target:apply hosting TARGET_NAME RESOURCE_NAME`
1. Log out and log in again to acquire new token.
1. Deploy to desired environment: `firebase deploy --only hosting:dev` or `firebase deploy --only hosting:prod`

## Add another site.
Pretend we have a site called blablamusic already on firebase hosting.

Just create another site called `blablamusic-dev` or whatever you want, just be sure to include prefix/sufix that indecates develepment environment.

## Setting site targets
Assuming that you have an existing firebase project, let's add tarets.

I guess your local project already been initiated with firebase and has `.firebaserc` and `firebase.json`.

Run command below:
```
$ firebase target:apply hosting prod blablamusic
$ firebase target:apply hosting dev blablamusic-dev
```

The rule is simple:
```
firebase target:apply hosting TARGET_NAME RESOURCE_NAME
```

TARGET_NAME is like an alias that distinguishes site.
RESOURCE_NAME is a name of your site (`blablamusic` or `blablamusic-dev`).

This will add dev/prod targets.

Your firebaserc will look like:
```json
{
  "projects": {
    "default": "blablamusic"
  },
  "targets": {
    "blablamusic": {
      "hosting": {
        "prod": [
          "blablamusic"
        ],
        "dev": [
          "blablamusic-dev"
        ]
      }
    }
  }
}
```

Make sure to log out and log in again to acquire brand new token, unless you'll encounter an error when publishing your site.

````
$ firebase logout
$ firebase login:ci
````

The command in the second line will print out firebase token. Be sure to copy it, for you need to store that token in GitHub to use automated deployment by using GitHub Actions.


## Deployment From local CLI
Let's test our first deployment from terminal.

Build your app and deploy.

```
$ firebase deploy --only hosting:dev
```

This will deploy your site only to `blablamusic-dev`.

## GitHub Integration
Using GitHub actions, the whole deployment process could easily be automated.

### Token Setup
In order to automate the deployment, Firebase token should be registered as GitHub secrets.

1. Go to your GitHub project, settings > secrets.
1. Add new secret called `FIREBASE_TOKEN` and paste your firebase token.

### Deployment Strategy
1. Deploy to development whenever commits has been pushed to origin.
1. Deploy to production when pull request has been merged into master branch.

We will use GitHub actions to automate deployment and `w9jds/firebase-action` library will be in use.

Before begin, create `.github/workflows` directory in the root of your project.

### Development Deployment
Create `deploy.yml` in `.github/workflows`directory.
{% raw %}
```yaml
---
name: Deploy
on:
  - push

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@master
      - name: Install Dependencies
        run: npm ci
      - name: Build
        run: npm run build
      - name: Archive Production Artifact
        uses: actions/upload-artifact@master
        with:
          name: build
          path: build
  deploy:
    name: Deploy Development
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@master
      - name: Download Artifact
        uses: actions/download-artifact@master
        with:
          name: build
          path: build
      - name: Deploy to Firebase
        uses: w9jds/firebase-action@v1.5.0
        with:
          args: deploy --only hosting:dev
        env:
          FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
```
{% endraw %}

### Production Deployment
Create `release.yml` in `.github/workflows`directory.
{% raw %}
```yaml
---
name: Release
on:
  pull_request:
    types: [closed]
    branches:
      - master

jobs:
  build:
    name: Build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@master
      - name: Install Dependencies
        run: npm ci
      - name: Build
        run: npm run build
      - name: Archive Production Artifact
        uses: actions/upload-artifact@master
        with:
          name: build
          path: build
  deploy:
    if: github.event.pull_request.merged == true
    name: Deploy
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Repo
        uses: actions/checkout@master
      - name: Download Artifact
        uses: actions/download-artifact@master
        with:
          name: build
          path: build
      - name: Deploy to Firebase
        uses: w9jds/firebase-action@v1.5.0
        with:
          args: deploy --only hosting:prod
        env:
          FIREBASE_TOKEN: ${{ secrets.FIREBASE_TOKEN }}
```
{% endraw %}

The code below triggers production release(deployment) only when the PR has been merged to branch master:

```yaml
name: Release
on:
  pull_request:
    types: [closed]
    branches:
      - master

# ...
  deploy:
    if: github.event.pull_request.merged == true
```


## Reference
- [Firebase hosting - Share project resources across multiple sites](https://firebase.google.com/docs/hosting/multisites)
- [GitHub Actions for Firebase](https://github.com/w9jds/firebase-action)
- [How do I solve: Error: Failed to list Firebase projects. See firebase-debug.log for more info](https://stackoverflow.com/questions/57941289/how-do-i-solve-error-failed-to-list-firebase-projects-see-firebase-debug-log)
