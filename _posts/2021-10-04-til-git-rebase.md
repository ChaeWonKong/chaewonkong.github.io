---
layout: post
title: "6 Steps to Use Git Rebase in Your Projects"
date: "2021-10-04"
categories:
- GIT
excerpt: |
Use git rebase change your base commits. Programmatically(CLI) learn how to rebase. Using rebase, collaboration with other developer will be much easier and simpler than before.
feature_text: |
  ## 6 Steps to Use Git Rebase in Your Projects
  Use git rebase change your base commits. Programmatically(CLI) learn how to rebase. Using rebase, collaboration with other developer will be much easier and simpler than before.
feature_image: "https://images.unsplash.com/photo-1556075798-4825dfaaf498?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=2352&q=80"
image: "https://images.unsplash.com/photo-1556075798-4825dfaaf498?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=2352&q=80"
---

Consider, there is two branches `main` and `develop`.

I need to add authentication api.
Then I will create new branch from `develop`.

### 1. Create Feature Branch

```bash
# First, change base branch to develop
$ git checkout develop

# Next, create new feature branch from develop
$ git checkout -b feature/auth-api
```

### 2. Make Changes And Commit the Changes

Then, I might commit some changes and updates.
Add new api, add configurations.
Do whatever is needed.

```bash
git add [CHANGED FILENAME]
git commit -m "[YOUR COMMIT MESSAGE]"
```

### 3. Rebase

After committing all changes,
now is the time to rebase.

```bash
git pull --rebase origin develop
```

which rebases your base commit, conforming to origin.

### 4. Push the Changes

Then, push your changes to the remote.

```bash
git push origin feature/auth-api
```

Create a pull request,
get some code reviews,
and merge your pull requrest.

### 5. Pull Origin

Next, checkout `develop` and pull the origin.

```bash
git checkout develop
git pull origin develop
```

### 6. Remove Feature Branch

Finally, remove the feature branch.

```bash
$ git branch -D feature/auth-api

# Tell remote about deleting the feature branch.
$ git push origin --delete feature/auth-api
```
