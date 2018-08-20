---
layout:     post
title:      "Hello World, Tableau"
subtitle:   " \"The idea is to go from numbers to information to understanding – Hans Rosling.\""
date:       2018-08-11 22:17:00
author:     "Nero"
toc:        true
header-img: "img/post-bg-data visualization.jpg"
tags:
    - 學習筆記
    - Tableau
    - BI
    - Visualization
---

> “從數位資料中理解真實世界的運作！”
{: .colorquote .info}

## 什麼是 Data Visualization ?
{:toc}

Data Visualization 資料視覺化是以視覺方式呈現數據的一種表示方式，人類對於視覺方式的吸收能力遠大於文字，並以視覺的方式呈現有助於我們對於資料整體上的相對關係與理解更為直覺。

## 為什麼需要 Data Visualization ?
{:toc}

[研究](http://www.epochtimes.com/b5/14/3/11/n4103733.htm)指出大腦對從不同感覺器官輸入的信息有不同的吸收率。一項實驗結果表明，在通常情況下，大腦對視覺輸入的信息吸收率最高，可達83%；對聽覺輸入信息的吸收率為11%；嗅覺可達3.5%；對觸覺和味覺輸入信息的吸收率最低，依序為1.5%和1%。

比起文字與數字與是覺得方式呈現想要表達的的訊息會讓人覺得更為活潑有興趣參與，一個出色的視覺化圖表可以達到以下優點。

1. 幫助聽眾快速地理解一個抽象的概念
2. 使報告的內容更為引人入勝
3. 可以降低資料的維度，凸顯出想表達客戶關注的部分


## 2018年排名前7的數據可視化工具
{:toc}

資料視覺化已成為現今[商業智慧](https://zh.wikipedia.org/wiki/%E5%95%86%E4%B8%9A%E6%99%BA%E8%83%BD)（Business Intelligence, BI）不可或缺的重要技術，以下為 [2018 年排名前7的數據可視化工具](https://www.business.com/articles/top-seven-data-visualization-tools-2018/)。

1. Tableau
2. FusionCharts
3. Datawrapper
4. Highcharts
5. TeamMate Ａnalytics
6. Sisense
7. Plotly


## 什麼是 Tableau ?
{:toc}

人人都是資料分析師，Tableau 是一套將 Data Visualization 的視覺化工具，用戶只需要專注在如何呈現想要說明的故事上，工具有使用過 Excel 的話十分的容易上手，可以拖拉的方式完成 Dashboard ，並有多種視圖包括條型圖、區域圖、、區域圖、餅圖、、區域圖、餅圖、散點圖等，可以選擇。

## 如何安裝 Tableau ?
{:toc}

可以在[官方網站](https://www.tableau.com/products/desktop/download)下載，或點擊[連結](https://www.tableau.com/downloads/desktop/mac)進行下載。

## 資料分析流程
{:toc}

<p align="center">
  <img width="100%" height="100%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post//2018-08-11-hello-world-tableau/data_analysis_process.jpeg">
</p>

1. 暸解你分析的目的是什麼？你希望別人從你給出的分析可以做出怎樣的決策或你能讓別人做出怎樣的決策？
2. 如何取得資料？資料格式是什麼？資料是否足夠讓人做出決策？
3. 探索資料從中找出適合解釋問題的特徵
4. 依照適合的情境與流程建立 model。
5. 將資料以適合的呈現方式 Visualization ，可以參考[這個網站](https://www.data-to-viz.com/)的決策規則。

## Tableau 操作
{:toc}

#### Dimensions(維度)
當連接數據時，Tableau 會將離散類型的屬性(例如：型別是字串或布林)分配到 Dimensions 中，當你從 Dimensions 中拖出一個屬性放入到一個視圖中時，Tableau 都會將它默認為是離散型的，同時為其分配一個藍色的背景。

#### Ｍeasures(度量)
任何從 Measures 上拖到視圖中的屬性默認都會是連續的，它的背景色也將會是綠色。但是如果你點擊字段並選擇 Discrete ，這些相應的值就會變成表格上方的標題。

#### Discrete(離散) 
而離散的概念在於不可分割，像是骰子跟教室裡面的學生，在定義上不會出現半點或半個學生，在 Tableau 終會以藍色進行表示。

#### Continus（連續) 
連續指一個不間段的範圍，在 Tableau 中會以綠色進行表示。

#### Columns(直列)
在工作表中橫向，即 X 軸中希望表達表達的表格屬性。

#### Row(橫行)
在工作表中直向，即 Ｙ 軸中希望表達表達的表格屬性。

#### Marks(標記)
有多個屬性可以添加，如下。
- Color
- Size
- Label
- Detail
- Tooltip

## 參考資料
{:toc}

* [資料視覺化Data Visualization：圖表設計](https://medium.com/uxeastmeetswest/%E8%B3%87%E6%96%99%E8%A6%96%E8%A6%BA%E5%8C%96data-visualization-%E5%9C%96%E8%A1%A8%E8%A8%AD%E8%A8%88-9ef17943a2d4)

* [The Top 7 Data Visualization Tools for 2018](https://www.business.com/articles/top-seven-data-visualization-tools-2018/)

* [I ranked every Intro to Data Science course on the internet, based on thousands of data points](https://medium.freecodecamp.org/i-ranked-all-the-best-data-science-intro-courses-based-on-thousands-of-data-points-db5dc7e3eb8e)