---
layout:     post
title:      "How to evaluate the performance of a machine learning model?"
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
  <img width="90%" height="90%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-03-30-how-to-evaluate-a-model/train-val-test-zh-tw.png">
</p>

### 交叉驗證(Cross-validation)
{:toc}

交叉驗證，簡稱 CV，是一種不必特別依賴於初始訓練集的模型選擇方法。下表匯總了幾種不同的方式：

| **k-fold** | **Leave-p-out** |
|:---  |:---  |
|在 k-1 份上訓練，並在剩於的那份上評定<br>同常 k=5 or 10|在 n-p 份資料上訓練，並在剩於的 p 份上評定<br>當 p=1 時，稱作 **leave-one-out** |

最常用的方法稱為 k 折疊交叉驗證，並將訓練數據分成 k 個折疊以在一個折疊上驗證模型，同時在 k-1 個其他折疊上訓練模型，所有這些 k 次。 然後將誤差在 k 倍上平均，並命名為交叉驗證錯誤。

<p align="center">
  <img width="90%" height="90%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2019-03-30-how-to-evaluate-a-model/cross-validation-zh-tw.png">
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

## 回歸指標
{:toc}

## 診斷與改善
{:toc}

## 參考資料
{:toc}

- [CS 229 ― Machine Learning by Stanford](https://stanford.edu/~shervine/teaching/cs-229/)

- [Handling imbalanced datasets in machine learning](https://towardsdatascience.com/handling-imbalanced-datasets-in-machine-learning-7a0e84220f28)

- [判斷模型是overfit還是underfit -- learning curve 與bias/variance tradeoff](https://blog.csdn.net/tsinghuahui/article/details/80285952)

- [Machine Learning 第六周笔记一：评估学习算法和 bias-variance](https://marcovaldong.github.io/2016/04/01/Machine-Learning%E7%AC%AC%E5%85%AD%E5%91%A8%E7%AC%94%E8%AE%B0%E4%B8%80%EF%BC%9A%E8%AF%84%E4%BC%B0%E5%AD%A6%E4%B9%A0%E7%AE%97%E6%B3%95%E5%92%8Cbias-variance/)

- [Stanford 機器學習](https://www.coursera.org/learn/machine-learning)

- [REGULARIZATION: An important concept in Machine Learning](https://towardsdatascience.com/regularization-an-important-concept-in-machine-learning-5891628907ea)