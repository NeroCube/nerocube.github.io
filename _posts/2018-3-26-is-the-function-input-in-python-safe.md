---
layout:     post
title:      "Is the function input() in Python safe?"
subtitle:   " \"Don't trust the input given to you by any user\""
date:       2018-03-26 22:09:00
author:     "Nero"
header-img: "img/post-bg-network.gif"
tags:
    - Python
    - Security
    - 學習筆記
---
> “學習 Python 中的 input() 函式”

## 前言
前陣子在學習 Python 用到了 input() 這個函式，在 Happy path 下好像很OK，但像說測試一下卻發現結果不如預期，俗話說：『 魔鬼藏在細節裡應該就是指這個意思 』，到底 input() 是個怎樣的東西呢？讓我們繼續看下去。

## BackDoor in the Code?
以下是一個讓用戶輸入花費並紀錄在資料庫中的一個小程式，這邊簡化不詳細實作 record_costs() 連線資料庫，這樣的一段程式，在執行上會有什麼問題嗎？

```
config = {'user': 'postgres', 'password': 'cohondob', 'database': 'testdb', 'host': '127.0.0.1', 'port': '5432'}

def record_costs(costs):
	status = 200
	# make a db connect with config and record costs in db
	return status

try:
	costs = int(input('please input your cost :'))
	record_costs(costs)

except Exception as e:
	print e
```
