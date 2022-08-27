---
layout:     post
title:      "Hello World, Redis"
subtitle:   " \"Accelerate the network access with NoSQL database\""
date:       2018-04-02 22:54:00
author:     "Nero"
image: "img/post-bg-data visualization.jpg"
tags:
    - Redis
    - Docker
    - 學習筆記
---

{{< myblockquote "colorquote info" "“初步認識什麼是 Redis”" >}}

## 什麼是 Redis ？

[Redis](https://redis.io/support) 是一個 in-memory 的 key-value 資料庫，具有以下特性。

* 高效性 – Redis 能讀的速度是110000次/s,寫的速度是81000次/s 。
* 支援性 - 支援多種資料結構，包含Hashs、Lists、Sets、Sorted Sets、HyperLogLog。
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

## 使用 docker-compose 創建個 redis server
建立一個檔案並命名為 redis.yml，在檔案中加入以下內容。
```
redis:
  restart: always
  container_name: redis
  image: redis:alpine
  ports:
    - 6379:6379
  volumes:
    - /tmp/redis-data:/tmp
```
輸入以下指令啟動 redis server，指令中`-f`為指定檔案，`-d`為在背景中執行 container。
```
$ docker-compose -f docker_redis_server.yml up -d
```
透過 redis-cli 連結 6379 port 的 redis server
```
$ docker run -it --link redis:redis --rm redis sh -c 'exec redis-cli -h redis -p 6379'
$ redis:6379> ping
PONG
```
進行簡單的 SET, GET ，Redis 支援的每一條指令可以參考 [Redis官網](https://redis.io/commands)
```
$ redis:6379> SET mykey "Hello"
OK
$ redis:6379> GET mykey
"Hello"
```
## Redis Benchmark
透過以下指令同時執行 10000 個請求，了解執行效能。

```
$ docker run -it --link redis:redis --rm redis sh -c 'exec redis-benchmark -h redis -p 6379  -n 10000 -q'

PING_INLINE: 42194.09 requests per second
PING_BULK: 42194.09 requests per second
SET: 41666.67 requests per second
GET: 42372.88 requests per second
INCR: 41666.67 requests per second
LPUSH: 40160.64 requests per second
RPUSH: 41841.00 requests per second
LPOP: 41493.78 requests per second
RPOP: 41841.00 requests per second
SADD: 40983.61 requests per second
HSET: 40816.32 requests per second
SPOP: 41322.31 requests per second
LPUSH (needed to benchmark LRANGE): 40485.83 requests per second
LRANGE_100 (first 100 elements): 30303.03 requests per second
LRANGE_300 (first 300 elements): 15847.86 requests per second
LRANGE_500 (first 450 elements): 12706.48 requests per second
LRANGE_600 (first 600 elements): 10504.20 requests per second
MSET (10 keys): 38910.51 requests per second
```
## 參考資料

[Redis AWS](https://aws.amazon.com/tw/redis/)

[Redis Runoob](http://www.runoob.com/redis/redis-data-types.html)

[Redis CN](http://www.redis.cn/)

[Redis Docker Hub](https://hub.docker.com/_/redis/)

[資料庫的好夥伴：Redis](https://blog.techbridge.cc/2016/06/18/redis-introduction/)

