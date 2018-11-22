---
layout:     post
title:      "Hello World Pytorch"
subtitle:   " \"The new stage of artificial intelligence\""
date:       2018-11-19 22:17:00
author:     "Nero"
toc:        true
header-img: "img/post-bg-ml.jpg"
tags:
    - 學習筆記
    - 機器學習
    - Pytorch
---
> “初探機器學習，Pytorch 基礎！”
{: .colorquote .info}

## 簡介
{:toc}

## Tensor
{:toc}
Tensor 即張量表示的是一個多維的矩陣，零為是一個點，一維是向量，二維是平面矩陣，多為相當連續的數組。
`torch.Tensor`是一中包含單一數據類型的多維陣列，Torch定義了七種CPU張量類型和八種GPU張量類型，`torch.Tensor`默認的tensor類型為（torch.FlaotTensor）。

數據type       | CPU張量 |GPU張量 |
--------------|:------:|:------:|
32位浮點	     | torch.FloatTensor   |torch.cuda.FloatTensor
64位浮點        | torch.DoubleTensor  |torch.cuda.DoubleTensor
16位浮點    	 | N / A       	       |torch.cuda.HalfTensor
8位整數（無符號） |	torch.ByteTensor	|torch.cuda.ByteTensor
8位整數（帶符號） |torch.CharTensor     |torch.cuda.CharTensor
16位整數（帶符號）|	torch.ShortTensor	|torch.cuda.ShortTensor
32位整數（帶符號）|	torch.IntTensor	    |torch.cuda.IntTensor
64位整數（帶符號）|	torch.LongTensor	|torch.cuda.LongTensor

以下範例為利用一個 python 的 list 建立一個tensor。
```
>>> torch.FloatTensor([[1, 2, 3], [4, 5, 6]])

1 2 3
4 5 6
[torch.FloatTensor of size 2x3]
```
也可以創建一個內容全為0的 Tensor 
```
>>> torch.zeros((3, 2))

tensor([[0., 0.],
        [0., 0.],
        [0., 0.]])
```
或者亂數產生隨機初始值
```
>>> torch.randn((3, 2))

tensor([[ 1.8436, -0.2691],
        [ 0.5289, -1.6964],
        [-0.3884, -0.0773]])
```
可以透過 index 指定改變的數值
```
>>> a = torch.Tensor([[1, 2, 3], [4, 5, 6]])
>>> a[1, 2]=100
>>> print(a)

tensor([[  1.,   2.,   3.],
        [  4.,   5., 100.]])
```
torch.Tensor 可轉換為 numpy.ndarray。
```
>>> a = torch.FloatTensor([[1, 2, 3], [4, 5, 6]])
>>> numpy_a = a.numpy()
>>> print(numpy_a)

array([[1., 2., 3.],
       [4., 5., 6.]], dtype=float32)
```
也可以從 numpy.ndarray 轉換為 torch.Tensor。
```
>>> a = np.array([[2,3],[4,5]])
>>> torch_a = torch.from_numpy(a)
>>> print(torch_a)
```
而需要透過 GPU 加速時只需要加上 cuda()。
```
if torch.cuda.is_available():
    a_cuda = a.cuda()
    print(a_cuda)
```
## Variable
{:toc}

## Dataset
{:toc}

## Module
{:toc}

## Optimizer
{:toc}

## 參考資料
{:toc}
