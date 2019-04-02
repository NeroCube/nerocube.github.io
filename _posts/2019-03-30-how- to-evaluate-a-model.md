---
layout:     post
title:      "How to evaluate the performance of a model?"
subtitle:   "\"The thing before deploy your model you should know.\""
date:       2019-03-30 22:17:00
author:     "Nero"
toc:        true
mathjax:    true
header-img: "img/post-bg-ml.jpg"
tags:
    - 學習筆記
    - 機器學習
    - 深度學習
---
> “如何評估一個機器學習模型的好壞?”
{: .colorquote .info}

## 前情提要
{:toc}

常常在訓練模型時發現在測試資料上可以取得很高的正確率但專案一上線卻發現實際效果不如預期，而為什麼會造成這樣的現象呢？回想以前讀熟時是否常常有個情況是在家寫評量或習題都很順，但實際上真正考試卻不如預期，遭成的原因便是我們只熟記了考古題而非實際題目中的概念或者題目中多了什麼我們沒看過的變化，而機器跟我們一樣往往在測試資料上有很好的表現，只是因為熟記了測試資料中的題目而非真正我們希望機器學到的概念，在機器學習中我們將之稱為過擬合(Overfitting)，而閱讀過本篇後希望可以回答以下問題。

1. **要如何驗證機器是否有學到該學的東西(Robust)?**
2. **哪裡學的不好(Explainability)？**
3. **該往哪裡改進且怎麼改進？**

## 模型選擇
{:toc}

### 名詞解釋(Vocabulary)
{:toc}

在選擇模型時，我們將數據分為的3個不同部分：

| **訓練集** | **驗證集** | **測試集** |
|:---  |:---  |:---  |
|模型訓練<br>一般數據集中的80% |模型評估<br>一般數據集中的20%<br>又叫保留集或開發集 |模型預測<br>未知數據 |

一旦選擇了模型，就會在整個數據集上進行訓練，並在測試集上進行測試。如下圖所示：

<p align="center">
  <img width="80%" height="80%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-03-30-how-to-evaluate-a-model/train-val-test-zh-tw.png">
</p>

### 交叉驗證(Cross-validation)
{:toc}

交叉驗證，簡稱 CV，是一種不必特別依賴於初始訓練集的模型選擇方法。下表匯總了幾種不同的方式：

| **k-fold** | **Leave-p-out** |
|:---  |:---  |
|在 k-1 份上訓練，並在剩於的那份上評定<br>同常 k=5 or 10|在 n-p 份資料上訓練，並在剩於的 p 份上評定<br>當 p=1 時，稱作 **leave-one-out** |

最常用的方法稱為 k 折疊交叉驗證，並將訓練數據分成 k 個折疊以在一個折疊上驗證模型，同時在 k-1 個其他折疊上訓練模型，所有這些 k 次。 然後將誤差在 k 倍上平均，並命名為交叉驗證錯誤。

<p align="center">
  <img width="80%" height="80%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-03-30-how-to-evaluate-a-model/cross-validation-zh-tw.png">
</p>

### 正則化(Regularization)
{:toc}

正則化過程目的在於避免模型過度擬合訓練資料，從而處理高方差問題(high variance issues)。 下表總結了不同類型的常用正則化技術：

| **LASSO** | **Ridge** | **Elastic Net** |
|:---  |:---  |:---  |
| 將係數縮小為0<br>有利於變量選擇 | 使係數更小| 在變量選擇和小係數之間進行權衡 |
|<img width="226" height="226" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-03-30-how-to-evaluate-a-model/lasso.png">|<img width="226" height="226" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-03-30-how-to-evaluate-a-model/ridge.png">|<img width="226" height="226" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-03-30-how-to-evaluate-a-model/elastic-net.png">|
| $...+\lambda\|\|\theta\|\|_1 $<br>$\lambda\in\mathbb{R}$|$...+\lambda\|\|\theta\|\|_2^2$<br>$ \lambda\in\mathbb{R}$|$...+\lambda\Big[(1-\alpha)\|\|\theta\|\|_1+\alpha\|\|\theta\|\|_2^2\Big]$<br>$\lambda\in\mathbb{R},\alpha\in[0,1]$|

## 分類指標
{:toc}

在處理二元分類問題時，以下為一些常見的評估指標。

### 混沌矩陣(Confusion matrix)
{:toc}

<p align="center">
  <img width="80%" height="80%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-03-30-how-to-evaluate-a-model/confusion-matrix.png">
</p>

### 主要測量指標
{:toc}

|**性能指標**|**公式**|**說明**|
|:--:  |:--:  |:---  |
|準確率（accuracy）|$\displaystyle\frac{\textrm{TP}+\textrm{TN}}{\textrm{TP}+\textrm{TN}+\textrm{FP}+\textrm{FN}}$|正確預測的數量除以預測總數。|
|類別精度（precision）|$\displaystyle\frac{\textrm{TP}}{\textrm{TP}+\textrm{FP}}$|以 Positive 為例，表示當模型判斷一個點屬於該類的情況下，判斷結果的可信程度。|
|類別召回率（recall）|$\displaystyle\frac{\textrm{TP}}{\textrm{TP}+\textrm{FN}}$|以 Positive 為例，表示模型能夠檢測到該類的比率。|
|F1 分數 (F1 score)|$\displaystyle\frac{2 \textrm{TP}}{2 \textrm{TP}+\textrm{FP}+\textrm{FN}}$|以 Positive 為例，混合度量，對於不平衡類別非常有效。|

**對於一個給定類，精度和召回率的不同組合如下：**

- 高精度+高召回率：模型能夠很好地檢測該類。
- 高精度+低召回率：模型不能很好地檢測該類，但是在它檢測到這個類時，判斷結果是高度可信的。
- 低精度+高召回率：模型能夠很好地檢測該類，但檢測結果中也包含其他類的點。
- 低精度+低召回率：模型不能很好地檢測該類。


### ROC
{:toc}

受試者工作曲線，又叫做 ROC 曲線，它使用真正例率 (True Positive Rate) 作為縱軸和假正例率 (False Positive Rate ) 作為橫軸並且進過調整閾值繪製出來。下表匯總了這些度量標準：

|**性能指標**|**公式**|**等價形式**|
|:--:  |:--:  |:--  |
|True Positive Rate <br>(TPR)|$\displaystyle\frac{\textrm{TP}}{\textrm{TP}+\textrm{FN}}$|Recall, sensitivity|
|False Positive Rate <br>(FPR)|$\displaystyle\frac{\textrm{FP}}{\textrm{TN}+\textrm{FP}}$|1-specificity|

<p align="center">
  <img width="90%" height="90%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-03-30-how-to-evaluate-a-model/roc-benchmark.png">
</p>

有效性不同的模型的 ROC 曲線圖示。左側模型必須犧牲很多精度才能獲得高召回率；右側模型非常有效，可以在保持高精度的同時達到高召回率。

### AUC
{:toc}

AUROC（Area Under the ROC），即ROC曲線下面積。可以看出，AUROC 在最佳情況下將趨近於1.0(近於0.0也是辨識能力佳的模型，但須將曲線反向)，而在最壞情況下降趨向於0.5。同樣，一個好的 AUROC 分數意味著我們評估的模型並沒有為獲得某個類（通常是少數類）的高召回率而犧牲很多精度。

<p align="center">
  <img width="90%" height="90%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-03-30-how-to-evaluate-a-model/roc-auc-zh-tw.png">
</p>

## 回歸指標
{:toc}

## 診斷與改善
{:toc}

## 參考資料
{:toc}

- [CS 229 ― Machine Learning by Stanford](https://stanford.edu/~shervine/teaching/cs-229/)

- [Handling imbalanced datasets in machine learning](https://towardsdatascience.com/handling-imbalanced-datasets-in-machine-learning-7a0e84220f28)

- [Understanding the Bias-Variance Tradeoff](http://scott.fortmann-roe.com/docs/BiasVariance.html)

- [Stanford 機器學習](https://www.coursera.org/learn/machine-learning)

- [REGULARIZATION: An important concept in Machine Learning](https://towardsdatascience.com/regularization-an-important-concept-in-machine-learning-5891628907ea)