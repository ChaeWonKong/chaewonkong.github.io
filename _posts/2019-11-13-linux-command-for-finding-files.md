---
layout: post
title: "Linux - Linux Command for finding files"
date: "2019-11-12"
categories:
  - Linux
excerpt: |
  Basic Linux commands for finding files in terminal - "find", "grep", "locate"
feature_text: |
  ## Linux - Linux Command for finding files
  Basic Linux commands for finding files in terminal - "find", "grep", "locate"
feature_image: "https://images.unsplash.com/photo-1518432031352-d6fc5c10da5a?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1567&q=80"
image: "https://images.unsplash.com/photo-1518432031352-d6fc5c10da5a?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1567&q=80"
---

Working with Linux commands is not easy for front-end developers. For me, it always been pain in the ass, since I haven't learned Linux before.

But sometimes, you really need to know some commands, for they will significantly reduce your time and effort.

Today, **We will gonna find some files through our terminal using Linux commands.**



### Find

```shell
find / -iname "FILENAME"
```

In order to ignore case use "-iname", for case-sensitive search, use '-name' instead.





### grep

```shell
sudo grep -r -i 'FILENAME' /
```

-r: find recursively.

-i: not case-sensitive.

/: within the directory given.



### locate

First, you need to install locate functionality.



#### Ubuntu

```shell
sudo apt-get install mlocate
```



Update `locate` database.

```shell
sudo updatedb
```



Find your file.

```shell
locate -i "FILENAME"
```