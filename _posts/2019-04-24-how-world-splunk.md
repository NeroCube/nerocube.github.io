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

## 牛刀小試

### 環境安裝
### 適合的情境
### 資料匯入
### 資料探索
### 資料視覺化
## 參考資料
- [Serch Command Reference](https://docs.splunk.com/Documentation/Splunk/7.2.5/SearchReference/Abstract)
- [Regx101:Online regex tester and debugger](https://regex101.com/)