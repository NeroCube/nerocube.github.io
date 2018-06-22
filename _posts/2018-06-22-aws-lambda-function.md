---
layout:     post
title:      "Use AWS Lambda reduce the load of RDS"
subtitle:   " \"Automatically adjust your server architecture\""
date:       2018-06-22 09:12:01
author:     "Nero"
header-img: "img/post-bg-os-metro.jpg"
tags:
    - AWS
    - 學習筆記
---

> “學習如何在 AWS 使用 CloudWatch 去 Event Trigger Lambda 降低 RDS 負載”

## 什麼是 AWS Lambda ?

AWS Lambda 是 Amazon 提供的一種 Serverless Service，這項服務不是真的沒有 Server 而是用戶不需要自己建置伺服器，利用 AWS LAmbda 搭配 AWS 的其他服務，透過自定義邏輯可以自動化後端服務維運，如下，開發者需要做的只有一件事就是『 寫程式 』。
* 伺服器與作業系統維護
* 內容佈建與自動擴展
* 程式與安全修補自動部署
* 監控與紀錄

## AWS Cloud Architecture

<p align="center">
  <img width="100%" height="100%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2018-06-22-aws-lambda-function/AWS-lambda-reduce-load.png">
</p>

上圖為 lambda-reduce-load 的架構，建立步驟如下。

## 建立 AWS Lambda

1. 開啟AWS —> Services  —> Lambda —> 建立函示
2. 執行時間（運行環境）選擇 `Node.js 6.10`
3. 在 index.js 撰寫以下 code

```
console.log('Loading function');
var pg = require('pg');
var conString = "postgres://YourUserName:YourPassword@localhost:5432/YourDatabase";
var queryString = 'TRUNCATE TABLE YourOverloadTable'

exports.handler = function(event, context){
  console.log("trying to connect...");
  pg.connect(conString, function(err, client, done) {
    if (err) {
      return console.error('error fetching client from pool', err);
    }
    console.log("connected to database");
    client.query(queryString, function(err, result) {
      done();
      if (err) {
        return console.error('error running query', err);
      }
      console.log(result);
      callback(err, result);
      client.end(function (err) { callback(err, result);});
    });
  });
}
```

## 地雷1: 沒有設定需要的 Package
這邊直接執行會發生錯誤，因為我們的 Lambda 環境中是沒有 'pg' module

```
Response:
{
  "errorMessage": "Cannot find module 'pg'",
  "errorType": "Error",
  "stackTrace": [
    "Function.Module._load (module.js:417:25)",
    "Module.require (module.js:497:17)",
    "require (internal/module.js:20:19)",
    "Object.<anonymous> (/var/task/index.js:2:10)",
    "Module._compile (module.js:570:32)",
    "Object.Module._extensions..js (module.js:579:10)",
    "Module.load (module.js:487:32)",
    "tryModuleLoad (module.js:446:12)",
    "Function.Module._load (module.js:438:3)"
  ]
}

Request ID:
"3398facc-75c9-11e8-a900-816ad48630d2"

Function Logs:
START RequestId: 3398facc-75c9-11e8-a900-816ad48630d2 Version: $LATEST
Unable to import module 'index': Error
    at Function.Module._resolveFilename (module.js:469:15)
    at Function.Module._load (module.js:417:25)
    at Module.require (module.js:497:17)
    at require (internal/module.js:20:19)
    at Object.<anonymous> (/var/task/index.js:2:10)
    at Module._compile (module.js:570:32)
    at Object.Module._extensions..js (module.js:579:10)
    at Module.load (module.js:487:32)
    at tryModuleLoad (module.js:446:12)
    at Function.Module._load (module.js:438:3)
END RequestId: 3398facc-75c9-11e8-a900-816ad48630d2
REPORT RequestId: 3398facc-75c9-11e8-a900-816ad48630d2  Duration: 95.26 ms  Billed Duration: 100 ms     Memory Size: 128 MB Max Memory Used: 19 MB  
```

## 拆雷1: 將 Lambda 環境打包上傳
這邊建議在自己 local 環境開一個專案，步驟如下。

```
$ mkdir lambda-reduce-load
$ cd lambda-reduce-load
$ git init
$ npm init
```

將剛剛的 index.js 加入並將 package.json 設定如下。

```
{
  "name": "lambda-reduce-load",
  "version": "1.0.0",
  "description": "Reduce the load of EPO with AWS Lambda",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "zip -r lambda-reduce-load.zip index.js node_modules",
    "start": "node index.js"
  },
  "author": "Nero",
  "license": "ISC",
  "dependencies": {
    "pg": "^4.5.7"
  }
}
```

安裝 pg package。

```
$ npm install pg --save
```

測試一下是不是 work。

```
$ npm start
```

打包專案成 zip 檔

```
$ npm run build
```

## 上傳函式程式碼

1. AWS Lambda Services 中 —> 函式程式碼 —> 程式碼輸入類型 —> 上傳 .ZIP檔 —> 函式套件 —> 上傳 
2. 選擇剛剛壓縮的 lambda-reduce-load.zip


## 地雷2:Timeout Error

現在執行還是無法成功你會看到以下錯誤訊息。

```
START RequestId: 2366ffd4-75d0-11e8-839e-25905c00e509 Version: $LATEST
2018-06-22T03:55:52.282Z    2366ffd4-75d0-11e8-839e-25905c00e509    trying to connect...
END RequestId: 2366ffd4-75d0-11e8-839e-25905c00e509
REPORT RequestId: 2366ffd4-75d0-11e8-839e-25905c00e509  Duration: 10009.31 ms   Billed Duration: 10000 ms   Memory Size: 1024 MB    Max Memory Used: 22 MB  
2018-06-22T03:56:02.288Z 2366ffd4-75d0-11e8-839e-25905c00e509 Task timed out after 10.01 seconds
```
## 拆雷2: 設定相關權限

以上錯誤可能是因為沒有設定執行角色與網路或設定錯誤。
1. 執行角色: 這邊如果沒有開好擁有 RDS 權限的角色，需要在 IAM 中設定給予存取權限。
2. 網路: 這邊需要設定你的函式將存取的 VPC 與 VPC 對應的 Security Group 和 子網路。

## 設定觸發條件

1. 在 SNS 中建立 SNS Topics，設定發生事件時的對應操作，如寄 E-mail、啟動 Lambda。
2. 在 CloudWatch 中設定 Alarm 偵測 CPU 與 ELB Average Latency 的 Threshold ，並綁上剛剛設定的 SNS Topics。

## 執行

我們可以利用 AWS Lambda 提供的『設定測試事件』測試一下功能是否如我們的預期，運行結果如下。

```
START RequestId: 6e4fd461-75c6-11e8-b80b-7535e8228806 Version: $LATEST
2018-06-22T02:46:22.956Z    6e4fd461-75c6-11e8-b80b-7535e8228806    trying to connect...
2018-06-22T02:46:23.023Z    6e4fd461-75c6-11e8-b80b-7535e8228806    connected to database
2018-06-22T02:46:23.029Z    6e4fd461-75c6-11e8-b80b-7535e8228806    Result {...}
END RequestId: 6e4fd461-75c6-11e8-b80b-7535e8228806
REPORT RequestId: 6e4fd461-75c6-11e8-b80b-7535e8228806  Duration: 1076.72 ms    Billed Duration: 1100 ms    Memory Size: 1024 MB    Max Memory Used: 23 MB  
```

## 總結

AWS Lambda 是一個非常強大並且彈性很高的服務，可以省去部署伺服器的步驟，專注於希望處理的的事件，並且只需要告訴 AWS 發生這個事件是該怎麼做(Lambda Function)，透過 Lambda 可以將日常的工作自動化，而開發人員則能有更多的時間專注在更重要的問題上面。
