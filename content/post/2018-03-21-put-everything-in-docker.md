---
layout:     post
title:      "Put Everything in Docker"
subtitle:   " \"Into the world of docker container\""
date:       2018-03-21 10:34:00
author:     "Nero"
image: "img/post-bg-unix-linux.jpg"
tags:
    - Docker
    - 學習筆記
---

{{< myblockquote "colorquote info" "“學習如何建立一個DockerFile並運行在Container中”" >}}


## 什麼是 Docker?
Docker是一套使用Google推出的Go語言實作的一項技術，在Linux內核架構上，使用作業系統層面的虛擬化技術對進程進行封裝隔離。

## 使用 Docker 的好處

### 提高利用資源的效率
不需要進行硬體虛擬以及運行完整操作系統，這使 Docker 比其他虛擬化技術系統資源的利用率更高，由於直接運行在Linux內核，無需啟動完整的作業系統，因此可以在幾秒或毫秒內啟動，大幅節省了開發、測試、部署的時間，以下為與傳統虛擬機比較。

特性 | 容器 | 虛擬機
---- | ---- | ----
啟動 | 秒級 | 分鐘級
硬盤使用 | 一般為 MB | 一般為 GB
性能 | 接近原生 | 弱於
系統支持量 | 單機支持上千個容器 | 一般幾十個

### 一致的運行環境
一致的開發環境、測試環境，避免了系統在不同環境下不可預期的bug產生，也不會有「在我的環境上跑沒問題啊」的情況。

### 部署更加容易
Docker 可以運行在許多平台 Mac、Linux 甚至 Windows，透過簡單的指令便能快速地將服務部署到機器上，且不用擔心運行環境變化導致無法正常運行。

## 前置作業
在開始前你必須在自己的自己的機器上安裝[Docker](https://docs.docker.com/docker-for-mac/install/#where-to-go-next)

## Dockerfile
在你的專案目錄底下執行以下指令，創建 Dockerfile。
```
$ touch Dockerfile
```
並在 Dockerfile 中編寫以下內容，以下為一個 nodejs 的 Dockerfile範例。
```
FROM google/nodejs

# Define working directory.
WORKDIR /data

# Define default command.
EXPOSE 3000
CMD []
ENTRYPOINT ["/nodejs/bin/npm", "start"]
```
- FROM: 指定 base image
- WORKDIR: 指定 docker 執行起來時候的預設目錄位置
- EXPOSE: 指定 image運作在 Docker 中的哪一個 port
- CMD: 指定 image 啟動後所要執行的指令
- ENTRYPOINT: 指令 image 啟動後，程式的進入點

## 建立並啟動
在 Dockerfile 所在的目錄底下，透過以下指令將你撰寫好的 Dockerfile 編譯成 image ，指令中的`-t`為設定 image 的 tag ，在 tag 欄位可以輸入你希望設定的 image tag。
```
docker build -t {image tag}
```
有了編譯好的 image 檔後我們只要輸入以下指令，便可以將 Docker 的服務跑起來，設定`-p`可以將在 Docker 中的 port 映射到主機上的 port。
```
docker run -p {localhost port}:{docker port} {image tag}
```

## 參考資料

[Docker學習筆記](https://peihsinsu.gitbooks.io/docker-note-book/content/)

[Docker — 從入門到實踐](https://yeasy.gitbooks.io/docker_practice/)

[Windows 上的 Dockerfile](https://docs.microsoft.com/zh-tw/virtualization/windowscontainers/manage-docker/manage-windows-dockerfile)

[Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#run)
