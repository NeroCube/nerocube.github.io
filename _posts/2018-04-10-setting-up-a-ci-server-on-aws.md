---
layout:     post
title:      "Setting up a Drone CI server on AWS EC2"
subtitle:   " \"Make testing, deploy and everything automatic\""
date:       2018-04-10 21:15:01
author:     "Nero"
header-img: "img/post-bg-container.jpg"
tags:
    - AWS
    - Drone
    - Docker
    - CI
    - 學習筆記
---

> “學習如何在 AWS 上建立一個 Drone CI Server 並連結 Github 進行自動化測試”

## 什麼是 Drone ?
Drone 是一個以容器技術為基礎，以Go編寫的持續交付系統。它使用一個簡單的yaml配置檔，類似 docker-compose 文件，用於在 Docker 容器內設定和執行pipelines。

## 事前準備
* 建立一個 Github 帳號
* 建立一個 AWS 帳號

## 在 AWS 建立一個 EC2 instance
1. 透過 AWS services -> Compute -> EC2 -> Launch Instance。
2. 這邊可以選擇自己習慣用的機器，範例選擇 Amazon Linux AMI 2017.09.1 (HVM), SSD Volume Type。
3. 選擇 Instance Type 這邊選擇 t2.micro 免費又好用。
4. 這邊 Add Storage 要注意 Size 最少要有8G。
5. 這邊 Configure Security Group 開 22 port for SSH 、 443 for HTTPS 與 80 和 8000 port for drone 的服務。

## 安裝 docker
利用 ssh 登入主機後，執行下列指令安裝 docker。
```
$ sudo su
$ curl -L "https://github.com/docker/compose/releases/download/1.18.0/docker-compose-$(uname -s)-$(uname -m)" > /usr/local/bin/docker-compose
$ chmod +x /usr/local/bin/docker-compose
$ docker-compose version
```

## 建立專案
在 Github 上建立一個簡單的 repo ，可以參考 [hello_wold_drone_ci_server](https://github.com/NeroCube/hello_wold_drone_ci_server) ，這個範例主要是以 Go 寫成的 hello_wold 並包含 unit tests，要特別注意的是要在 Drone CI 上跑的專案必須加入 .drone.yml。
```
pipeline:
  testing:
    image: golang:1.10.0
    commands:
      - echo "Hello World,Drone!"
      - go test -v .
```
透過以上範例，在我們 push commit 時 Github 會去 trigger 你設定的 Drone Server 並執行 commands 中的指令。

## 註冊 Github application
1. 透過個人設定點選 Settings —> Developer settings —> OAuth Apps —> Register
2. Homepage URL 輸入AWS Public DNS (IPv4) 並加上 `[drone-port]` ，範例這邊開的是 8000 port
```
http://[public-ip-of-ec2-instance]:[drone-port]
```
3. Authorization callback UR 輸入AWS Public DNS (IPv4) 並加上 `[drone-port]/authorize` ，範例這邊開的是 8000 port
```
http://[public-ip-of-ec2-instance]:[drone-port]/authorize
```
<p align="center">
  <img width="60%" height="60%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2018-04-10-setting-up-a-ci-server-on-aws/Screen Shot 2018-04-12 at 10.51.56 PM.png">
</p>

## 建立 docker-compose
在下列指令建立一個 docker-compose file
```
$ touch docker-compose.yml
$ vi docker-compose.yml
```
編寫 drone 的 docker-compose.yml，以下為範例。

```
version: '3'

services:
  drone-server:
    image: drone/drone:0.8.1
    ports:
      - 8000:8000
      - 9000:9000
    volumes:
      - ./drone:/var/lib/drone/
    restart: always
    environment:
      - DRONE_OPEN=true
      - DOCKER_API_VERSION=1.26
      - DRONE_HOST=127.0.0.0
      - DRONE_SECRET=[RANDOM_SECRET_OF_YOUR_CHOOSING]
      - DRONE_ADMIN=[YOUR_GITHUB_ACCOUNT]
      - DRONE_GITHUB=true
      - DRONE_GITHUB_CLIENT=[CLIENT_KEY_FROM_OAUTH]
      - DRONE_GITHUB_SECRET=[SECRET_FROM_OAUTH]

  drone-agent:
    image: drone/agent:0.8.1
    restart: always
    depends_on:
      - drone-server
    volumes:
      - /var/run/docker.sock:/var/run/docker.sock
    environment:
      - DOCKER_API_VERSION=1.26
      - DRONE_SERVER=drone-server:9000
      - DRONE_SECRET=[MUST_BE_SAME_AS_IN_SERVER]
```
這邊可以透過 `openssl rand -base64 32`生成 DRONE_SECRET ，並填入 Github 取得的`DRONE_GITHUB_CLIENT`與`DRONE_GITHUB_SECRET`，然後`:wq`寫入。

## 啟動 Drone Server
執行以下指令，啟動 docker-compose
```
$ docker-compose up
```
接著就可以透過瀏覽器輸入`http://[public-ip-of-ec2-instance]:8000` ，便可以在瀏覽器上看到你的 CI 服務了。

![Testing on Drone](https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2018-04-10-setting-up-a-ci-server-on-aws/Screen%20Shot%202018-04-12%20at%2010.52.56%20PM.png)
每次 commit 到 Github 上 CI Server 便會自動化做測試。

## 參考資料
[drone-tutorial](https://github.com/go-training/drone-tutorial)

[drone-on-docker-compose](https://github.com/appleboy/drone-on-docker-compose/blob/master/docker-compose.yml)

[amazon-ec2-user-guide](https://github.com/awsdocs/amazon-ec2-user-guide/blob/master/doc_source/concepts.md)
