---
layout:     post
title:      "Hello World, Github Pages"
subtitle:   " \"Build a little blog on Github\""
URL: "/2018/03/20/hello_world_github_pages/"
date:       2018-03-20 10:00:00
image:      "img/post-bg-2015.jpg"
author:     "Nero"
showonlyimage: false
published:  true 
tags:
    - Github
    - 學習筆記

# description: "除来自用户的访问请求以外，微服务应用中的各个微服务相互之间还有大量的访问,根据应用系统数据敏感程度不同，对于系统内微服务的访问也需要进行相应的安全控制。"
# excerpt: "除来自用户的访问请求以外，微服务应用中的各个微服务相互之间还有大量的访问,根据应用系统数据敏感程度不同，对于系统内微服务的访问也需要进行相应的安全控制。"
#categories: [ Tech ]    
---

{{< myblockquote "colorquote info" "“學習如何在Github上建立簡單的個人頁面”" >}}


## 建立Repository
建立一個新的Repository並將它命名為username.github.io，這邊的username必須是Github 帳號的ID，我的帳號是nerocube，所以這個專案需要命名為nerocube.github.io，如下圖。

![](/img/in-post/2018-03-20-hello-world-github-pages/creat-your-blog.png)

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
