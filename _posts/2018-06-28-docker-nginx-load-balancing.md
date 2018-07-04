---
layout:     post
title:      "Use Nginx as load balancing in Docker."
subtitle:   " \"Reduce the backend loading with Nginx load balance.\""
date:       2018-06-28 14:00:00
author:     "Nero"
header-img: "img/post-bg-os-metro.jpg"
tags:
    - Docker
    - Nginx
    - 學習筆記
---

> “學習如何在 Docker 中使用 Nginx 的 load balance”

## 什麼是 Load Balance ?
[負載平衡](https://zh.wikipedia.org/wiki/%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1)（Load balancing）是一種用來在多個電腦（電腦集群）、網路連線、CPU、磁碟驅動器或其他資源中分配負載的技術。
在 [Nginx](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/) 中提供了以下幾種的負載平衡規則。

- Round Robin: 將請求平均分佈於部署的伺服器上，這個方法為預設方法。
- Least Connections: 將請求發送到活動連接數最少的伺服器。
- IP Hash: 發送請求的伺服器根據客戶端IP地址決定。在這種情況下，使用 IPv4 地址的前三個八位位組或全部 IPv6 地址來計算雜湊值。該方法保證來自同一地址的請求到達同一個伺服器，直到它不可用為止。
- Generic Hash: 請求發送到的伺服器由用戶定義的 key 確定，key 可以是 text string，variable 或 combination。例如，key 可以配對來源IP地址和端口，讓同一接口的請求對到同一伺服器。

## 為什麼需要 Load Balance ?
透過 Load balancing 可以達到以下的目的。

- 最佳化資源使用
- 最大化吞吐率
- 最小化回應時間
- 同時避免過載
- 提高可靠性

## Architecture

## Load Balancing Settings
這邊 Load Balance 的設置如下。
- weight: NGINX 分配請求的權重比例。
- max_fails: 設置 NGINX 將伺服器標記為不可用的連續失敗嘗試次數。
- fail_timeout: 當該機器達到 `max_fails` 時，NGINX 認為該機器不可用的時間。

```
worker_processes 4;

events { worker_connections 1024; }

http {

    upstream node-app {
        least_conn;
        server node1:3001 weight=10 max_fails=3 fail_timeout=30s;
        server node2:3002 weight=10 max_fails=3 fail_timeout=30s;
        server node3:3003 weight=10 max_fails=3 fail_timeout=30s;
    }

    server {
        listen 80;

        location / {
            proxy_pass http://node-app;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection 'upgrade';
            proxy_set_header Host $host;
            proxy_cache_bypass $http_upgrade;
        }
    }
}
```

## 測試
可以透過 Terminal 執行下方指令，觀察 Load Balance 情況。
```
$ curl -i http://localhost:8888/
```

執行結果如下:
```
load-balance_1  | 172.22.0.1 - - [28/Jun/2018:03:54:53 +0000] "GET / HTTP/1.1" 200 26 "-" "curl/7.43.0"
node2_1         | Service:node2 is called
load-balance_1  | 172.22.0.1 - - [28/Jun/2018:03:54:55 +0000] "GET / HTTP/1.1" 200 26 "-" "curl/7.43.0"
node3_1         | Service:node3 is called
load-balance_1  | 172.22.0.1 - - [28/Jun/2018:03:54:56 +0000] "GET / HTTP/1.1" 200 26 "-" "curl/7.43.0"
node1_1         | Service:node1 is called
load-balance_1  | 172.22.0.1 - - [28/Jun/2018:03:54:56 +0000] "GET / HTTP/1.1" 200 26 "-" "curl/7.43.0"
node2_1         | Service:node2 is called
load-balance_1  | 172.22.0.1 - - [28/Jun/2018:03:54:57 +0000] "GET / HTTP/1.1" 200 26 "-" "curl/7.43.0"
load-balance_1  | 172.22.0.1 - - [28/Jun/2018:03:54:58 +0000] "GET / HTTP/1.1" 200 26 "-" "curl/7.43.0"
node3_1         | Service:node3 is called
node1_1         | Service:node1 is called
load-balance_1  | 172.22.0.1 - - [28/Jun/2018:03:54:59 +0000] "GET / HTTP/1.1" 200 26 "-" "curl/7.43.0"
```

## Benchmark
Upstream with 3 server 
```
$ echo "GET http://localhost:8888" | vegeta attack -rate=100 -connections=1 -duration=1s | tee results.bin | vegeta report

Requests      [total, rate]            100, 101.01
Duration      [total, attack, wait]    993.69113ms, 989.999ms, 3.69213ms
Latencies     [mean, 50, 95, 99, max]  2.428016ms, 2.371484ms, 3.163846ms, 6.979051ms, 7.686791ms
Bytes In      [total, mean]            2600, 26.00
Bytes Out     [total, mean]            0, 0.00
Success       [ratio]                  100.00%
Status Codes  [code:count]             200:100
```
Upstream with 1 server 
```
echo "GET http://localhost:8888" | vegeta attack -rate=100 -connections=1 -duration=1s | tee results.bin | vegeta report
Requests      [total, rate]            100, 101.01
Duration      [total, attack, wait]    992.414335ms, 989.999ms, 2.415335ms
Latencies     [mean, 50, 95, 99, max]  2.159423ms, 2.092387ms, 3.045662ms, 3.590794ms, 3.69478ms
Bytes In      [total, mean]            2600, 26.00
Bytes Out     [total, mean]            0, 0.00
Success       [ratio]                  100.00%
Status Codes  [code:count]             200:100
 ```
## 完整程式碼
[docker-nginx-load-balancing](https://github.com/NeroCube/docker-nginx-load-balancing)
