---
layout:     post
title:      "Hello World, Redis"
subtitle:   " \"Accelerate the network access with NoSQL database\""
date:       2018-04-02 22:54:00
author:     "Nero"
header-img: "img/post-bg-data visualization.jpg"
tags:
    - Redis
    - NoSQL
    - 學習筆記
---
> “初步認識什麼是 Redis”

## 什麼是 Redis ？

[Redis](https://redis.io/support) 是一個 in-memory 的 key-value 資料庫。

* 性能極高 – Redis 能讀的速度是110000次/s,寫的速度是81000次/s 。
* 支援多種資料結構 - 包含Hashs、Lists、Sets、Sorted Sets、HyperLogLog。
* 原子性 – Redis 的所有操作都是原子性的，只分為成功執行與失敗完全不執行。
* 持久性 – Redis 運行時在記憶體中但是可以儲存到硬碟，但高速存取時資料量不可以大於硬體的記憶體容量。

## 使用 Redis 可以改善什麼？

在需要快取（Cache）一些資料的場合，可以減輕許多後端資料庫的壓力

## Redis 可以實作哪些應用？

* 快取
* 遊戲排行榜
* 豐富的媒體串流
* 聊天、簡訊和佇列
* 工作階段存放區
* 地理空間
* 機器學習
* 即時分析

## 參考資料

[AWS Redis](https://aws.amazon.com/tw/redis/)

[資料庫的好夥伴：Redis](https://blog.techbridge.cc/2016/06/18/redis-introduction/)

[Runoob Redis](http://www.runoob.com/redis/redis-data-types.html)

