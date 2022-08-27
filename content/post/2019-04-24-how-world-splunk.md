---
layout:     post
title:      "Hello World, Splunk"
subtitle:   " \"Take the sh out of IT.\""
date:       2019-04-24 22:03:00
author:     "Nero"
image: "img/post-bg-data visualization.jpg"
tags:
    - 學習筆記
    - Splunk
    - Data Science
    - ETL
---

{{< myblockquote "colorquote info" "“The Search Engine in the Enterprise！”" >}}


## 什麼是 Splunk


Splunk 是及時的 machine log 分析服務，Splunk 於官方網站的定義為 Log Server 軟體與 IT 搜尋引擎的結合，從單一平台集中瀏覽整個公司的機房 IT 架構，讓系統管理員從單一管理介面針對所有不同的系統、硬體紀錄與歷史紀錄檔，進行搜尋、報告、監視、警示及分析，系統管理員可於短時間鎖定系統、硬體、應用程式問題及偵測安全事件，而不用耗上很多的時間及人力尋找系統問題 (trouble shooting)，更可監視系統，避免服務效能降低或中斷、以提升系統可用度，並依照管理者設定需求提出警示。

## Splunk 可以帶來什麼效益


傳統上為了分析 machine log 後端需要針對不同的 log format 寫不同的 parser 再針對不同的 format 在資料庫中設計不同的 table 儲存資料，往往當需求有所變動時需要重新撰寫 parser 與修改資料庫中的欄位，遭成如需求不明確時工程師需要花許多resource 在維護 uploader ，而前端需要與後端溝通欄位的定義，然後再修正所有對應的前端畫面，在 Splunk 中預設了需多常見的 log format，可以幫助我們處理 parser 任務，針對特殊的格式我們可以使用 regular expression 進行資料萃取，再透過類似 linux shell 的 SPL 對資料進行搜尋與統計，最後可以透過拖拉點選的方式視覺畫呈現結果，讓我們可以快速的完成上級交辦的分析任務準確快數的發現問題點。

## Splunk 的運作機制


![](/img/in-post/2019-04-24-how-world-splunk/splunk-components.png)

Splunk有3個主要組件：
- Splunk Forwarder，用於數據轉發
- Splunk Indexer，用於解析和索引數據
- Search Head是一個用於搜索，分析和報告的GUI

### Splunk Forwarder

Splunk Forwarder 是用於收集日誌的組件。假設你要從遠端主機收集日誌，那麼你可以通過獨立於主  Splunk 實例的 Splunk Remote Forwarder 來實現這一目標。可以在多台主機上安裝多個 Forwarder，這些 Forwarder 會將日誌數據轉發到 Splunk Indexer 進行處理和存儲。如果你想對數據進行實時分析怎麼辦？Splunk Forwarder 也可用於此目的。你可以配置 Forwarder 以即時向 Splunk 索引器發送數據。可以將它們安裝在多個系統中，並即時從不同的主機同時收集數據。

### Splunk Indexer

Indexer 是 Splunk 組件，你必須使用它來索引和存儲來自 Forwarder 的數據。Splunk 實例將傳入的數據轉換為事件並將其存儲在索引中，以便有效地執行搜索操作。如果你從 Forwarder 接收數據，則索引器將首先解析數據然後將其編入索引。當 Splunk 實例為你的數據編制索引時，它會創建許多文件，這些文件駐留在稱為存儲桶的目錄集中。

這些文件包含以下內容：

- 壓縮形式的原始數據
- 指向原始數據的索引（索引文件，也稱為tsidx文件），以及一些元數據文件


Splunk 以下方式實現快速搜索和分析：

- 將數據流分成單獨的可搜索事件
- 創建或標識時間戳
- 提取主機，源和源類型等字段
- 對傳入數據執行用戶定義的操作，例如標識自定義字段，屏蔽敏感數據，編寫新的或修改的密鑰，應用多行事件的中斷規則，過濾不需要的事件以及將事件路由到指定的索引或服務器

Splunk Indexer 的另一個好處是數據複製。你無需擔心數據丟失，因為 Splunk 會保留索引數據的多個副本。此過程稱為索引複製或索引器群集。這是在 Indexer 集群的幫助下實現的，該集群是一組配置為複制彼此的數據的索引器。

### Splunk Search Head

Search Head 提供了用於執行各種操作的圖形用戶界面。可以通過輸入搜索詞來搜索和查詢存儲在索引器中的數據，可以將 Search Head 安裝在單獨的服務器上，也可以安裝在同一服務器上的其他 Splunk 組件上。 Search Head 沒有單獨的安裝檔，只需在 Splunk 服務器上啟用 splunkweb 服務即可啟用它。

Splunk 實例既可以用作 Search Head ，也可僅執行搜索而不進行索引的。

在 Splunk 實例中， Search Head 可以將搜索請求發送給一組 Indexer 或搜索實體，這些 Indexer 對其 Indexer 執行實際搜索。 Search Head 然後合併結果並將它們發送回用戶。這是一種更快速的搜索數據的技術，稱為分佈式搜索。

Search Head 群集是協調搜索活動的 Search Head 組。群集協調 Search Head 的活動，根據當前負載分配作業，並確保所有 Search Head 都可以訪問同一組 knowledge objects。

## 環境安裝


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

4. 接受許可協議`SPLUNK_START_ARGS=--accept-license`。必須在每個splunk/splunk 容器上明確接受，否則Splunk 將無法啟動。

容器成功啟動後會進入`healthy`狀態，可透過瀏覽器使用 `admin` 訪問 http://localhost:8000 開始使用 Splunk 。

## 適合的情境


- 不做修改與刪除的 Log 資料
- 每筆記錄具有時間戳記
- 資料的內容及格式是用戶可解析的

## 資料探索


Splunk 提供了一種名為 SPL 的搜尋語言，讓用戶方便搜索，關聯，分析和可視化任何數據。

![](/img/in-post/2019-04-24-how-world-splunk/splunk-example.png)

如上圖 top 命令為轉換命令。會在表格中組織搜尋結果。使⽤轉換命令來產⽣可⽤來建⽴視覺化 (例如柱狀圖、折線圖、區域圖及圓餅圖) 的結果。由於轉換命令以表格格式傳回搜尋結果；因此，結果會顯⽰在 [統計資料] 索引標籤中。在這個針對成功購買的搜尋中，找到了七個不同的類別 ID。清單會根據事件中類別 ID 值的頻率，依最⾼到最低順序顯⽰類別 ID 值。

## 資料視覺化

![](/img/in-post/2019-04-24-how-world-splunk/splunk-example2.png)

可以在 [視覺化] 索引標籤中檢視轉換搜尋的結果，並設定圖表類型的格式。
1. 按⼀下 [視覺化] 索引標籤。
依預設，[視覺化] 索引標籤會以柱狀圖開啟。
2. 按⼀下 [柱狀圖] 以開啟視覺化類型選取器。

## 參考資料


- [Serch Command Reference](https://docs.splunk.com/Documentation/Splunk/7.2.5/SearchReference/Abstract)
- [Regx101:Online regex tester and debugger](https://regex101.com/)