---
layout:     post
title:      "Put Everything in Docker"
subtitle:   " \"Into the world of docker container\""
date:       2018-03-21 10:34:00
author:     "Nero"
header-img: "img/post-bg-unix-linux.jpg"
tags:
    - Docker
    - 學習筆記
---

> “學習如何建立一個DockerFile並運行在Container中”

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

## Hello World - Docker
以下為一個 nodejs 的 Dockerfile範例
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
- EXPOSE: 指定所有發布的 port
- CMD: 指定 image 啟動後所要執行的指令
- ENTRYPOINT: 指令 image 啟動後，程式的進入點
## 參考資料

[Docker學習筆記](https://peihsinsu.gitbooks.io/docker-note-book/content/)

[Docker — 從入門到實踐](https://yeasy.gitbooks.io/docker_practice/)

[Windows 上的 Dockerfile](https://docs.microsoft.com/zh-tw/virtualization/windowscontainers/manage-docker/manage-windows-dockerfile)

[Best practices for writing Dockerfiles](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/#run)
