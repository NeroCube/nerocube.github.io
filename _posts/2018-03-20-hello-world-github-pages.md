---
layout:     post
title:      "Hello World, Github Pages"
subtitle:   " \"Build a little blog on Github\""
date:       2018-03-20 10:00:00
author:     "Nero"
header-img: "img/post-bg-2015.jpg"
tags:
    - Github
    - 學習筆記
---

> “學習如何在Github上建立簡單的個人頁面”

## 建立Repository
建立一個新的Repository並將它命名為username.github.io，這邊的username必須是Github 帳號的ID，我的帳號是nerocube，所以這個專案需要命名為nerocube.github.io，如下圖。

<p align="center">
  <img width="70%" height="70%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2018-03-20-hello-world-github-pages/creat-your-blog.png">
</p>


## 複製至本機
將這個專案複製至自己的機器上
```
$ git clone https://github.com/username/username.github.io
```

## Hello World, Github Pages
進入專案資料夾並簡單的加入一個含有Hello World的index.html
```
$ cd username.github.io
$ echo "Hello World" > index.html
```

## 上傳專案
將修改的專案進行上傳
```
$ git add --all
$ git commit -m "Initial commit"
$ git push -u origin master
```

## 完成！！
你可以透過` https://username.github.io.`瀏覽你的Github Pages了

## 參考
[Github Pages官方文件](https://pages.github.com/)
