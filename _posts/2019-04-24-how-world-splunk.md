---
layout:     post
title:      "Hello World, Splunk"
subtitle:   " \"Take the sh out of IT.\""
date:       2019-04-24 22:03:00
author:     "Nero"
toc:        true
header-img: "img/post-bg-data visualization.jpg"
tags:
    - 學習筆記
    - Splunk
    - Data Science
    - ETL
---

> “The Search Engine in the Enterprise！”
{: .colorquote .info}

## 什麼是 Splunk
{:toc}

Splunk 是及時的 machine log 分析服務，Splunk 於官方網站的定義為 Log Server 軟體與 IT 搜尋引擎的結合，從單一平台集中瀏覽整個公司的機房 IT 架構，讓系統管理員從單一管理介面針對所有不同的系統、硬體紀錄與歷史紀錄檔，進行搜尋、報告、監視、警示及分析，系統管理員可於短時間鎖定系統、硬體、應用程式問題及偵測安全事件，而不用耗上很多的時間及人力尋找系統問題 (trouble shooting)，更可監視系統，避免服務效能降低或中斷、以提升系統可用度，並依照管理者設定需求提出警示。

## Splunk 可以帶來什麼效益
{:toc}

傳統上為了分析 machine log 後端需要針對不同的 log format 寫不同的 parser 再針對不同的 format 在資料庫中設計不同的 table 儲存資料，往往當需求有所變動時需要重新撰寫 parser 與修改資料庫中的欄位，遭成如需求不明確時工程師需要花許多resource 在維護 uploader ，而前端需要與後端溝通欄位的定義，然後再修正所有對應的前端畫面，在 Splunk 中預設了需多常見的 log format，可以幫助我們處理 parser 任務，針對特殊的格式我們可以使用 regular expression 進行資料萃取，再透過類似 linux shell 的 SPL 對資料進行搜尋與統計，最後可以透過拖拉點選的方式視覺畫呈現結果，讓我們可以快速的完成上級交辦的分析任務準確快數的發現問題點。

## Splunk 的運作機制
{:toc}

<p align="center">
  <img width="60%" height="60%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-04-24-how-world-splunk/splunk-components.png">
</p>

Splunk有3個主要組件：
- Splunk Forwarder，用於數據轉發
- Splunk Indexer，用於解析和索引數據
- Search Head是一個用於搜索，分析和報告的GUI

### Splunk Forwarder

Splunk Forwarder 是用於收集日誌的組件。假設你要從遠端主機收集日誌，那麼你可以通過獨立於主  Splunk 實例的 Splunk Remote Forwarder 來實現這一目標。可以在多台主機上安裝多個 Forwarder，這些 Forwarder 會將日誌數據轉發到 Splunk Indexer 進行處理和存儲。如果你想對數據進行實時分析怎麼辦？Splunk Forwarder 也可用於此目的。你可以配置 Forwarder 以即時向 Splunk 索引器發送數據。可以將它們安裝在多個系統中，並即時從不同的主機同時收集數據。

## 環境安裝
{:toc}

Splunk 官方除了 Splunk Enterprise 還提供了 Docker 版本供下載。

**Splunk Enterprise**

可透過[官方網站](https://www.splunk.com/zh-hant_cn/download/splunk-enterprise.html)下載 Windows、Linux或者 Mac OS 版本的 Splunk Enterprise 可有 60 天的免費試用。

**Docker Splunk**

下載遠端 docker image 到本地端 docker engine。

```
$ docker pull splunk/splunk:latest
```
透過下列指令啟動 Splunk Universal Forwarder。

```
$ docker run -d -p 8000:8000 -e "SPLUNK_START_ARGS=--accept-license" -e "SPLUNK_PASSWORD=" --name splunk splunk/splunk:latest
```

上方指令作用如下：

1. 在 Docker 容器中啟動 `splunk/splunk:latest` image

2. 將本機上的 `8000` port 與容器 `8000` port 對接。

3. 指定登入時使用的密碼 `SPLUNK_PASSWORD`，`` 替換為符合 [Splunk Enterprise 密碼](https://docs.splunk.com/Documentation/Splunk/latest/Security/Configurepasswordsinspecfile) 要求的任何字串。

4. 接受許可協議`SPLUNK_START_ARGS=--accept-license`。必須在每個splunk/splunk 容器上明確接受，否則Splunk將無法啟動。

容器成功啟動後會進入`healthy`狀態，可透過瀏覽器使用 `admin` 訪問 http://localhost:8000 開始使用 Splunk 。

## 適合的情境
{:toc}

## 資料匯入
{:toc}

## 資料探索
{:toc}

## 資料視覺化
{:toc}

## 參考資料
{:toc}

- [Serch Command Reference](https://docs.splunk.com/Documentation/Splunk/7.2.5/SearchReference/Abstract)
- [Regx101:Online regex tester and debugger](https://regex101.com/)