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
前陣子在學習 Python 用到了 input() 這個函式，在 Happy Case 下好像很OK，但想說測試一下卻發現結果不如預期，俗話說：『 魔鬼藏在細節裡應該就是指這個意思 』，到底 input() 是個怎樣的東西呢？讓我們繼續看下去。

## BackDoor in the Code?
以下是一個讓用戶輸入花費並紀錄在資料庫中的一個小程式，這邊簡化不詳細實作 record_costs() 連線資料庫，這樣的一段程式，在執行上會有什麼問題嗎？

```
config = {'user': 'postgres', 'password': 'cohondob', 'database': 'testdb', 'host': '127.0.0.1', 'port': '5432'}

def record_costs(costs):
	status = 200
	# make a db connect with config and record costs in db
	print 'status code: {}'.format(status)
	return status

try:
	costs = int(input('please input your cost :'))
	record_costs(costs)

except Exception as e:
	print e
```

## 解謎篇
在 Happy Case 下我們輸入的只要是數值型別都是可以正常運作的。
```
$ python backdoor_in_input.py
please input your cost :1
status code: 200
```
但如果我們輸入的不是數值是一個字串呢？
```
$ python backdoor_in_input.py
please input your cost :hi
name 'hi' is not defined
```
輸入一個字串做數值的轉型會錯好像也蠻正常的啊，但我們只仔細看錯誤訊息，好像哪裡怪怪的，如果轉型失敗為什麼是 `name 'hi' is not defined`？
我們稍微修改一下程式成下面這樣，方便讓我們知道是哪種錯誤。
```
config = {'user': 'postgres', 'password': 'cohondob', 'database': 'testdb', 'host': '127.0.0.1', 'port': '5432'}

def record_costs(costs):
	status = 200
	# make a db connect with config and record costs in db
	print 'status code: {}'.format(status)
	return status

try:
	costs = int(input('please input your cost :'))
	record_costs(costs)

except Exception as e:
	template = "An exception of type {0} occurred. Arguments:\n{1!r}"
	message = template.format(type(e).__name__, e.args)
	print message
```
再試一次輸入不是數值的字串。
```
$ python backdoor_in_input.py
please input your cost : hi
An exception of type NameError occurred. Arguments:
("name 'hi' is not defined",)
```
查了一下照理說轉型失敗不是[ValueError](https://docs.python.org/2/library/exceptions.html#exceptions.ValueError)嗎？再看看那個錯誤訊息  `name 'hi' is not defined` ，所以如果輸入的是有在定義中的變數會發生什麼事？
```
$ python backdoor_in_input.py
please input your cost :config
An exception of type TypeError occurred. Arguments:
("int() argument must be a string or a number, not 'dict'",)
```
誒～輸入的字串被識別成一個變數了，而且系統直接存取這個變數來用，感覺好像不太妙，因為是 `dict` 所以好像也還好沒有很嚴重，這邊大膽地做一個測試，如果我輸入的是`str(config)`會發生什麼事呢？
```
$ python backdoor_in_input.py
please input your cost :str(config)
An exception of type ValueError occurred. Arguments:
('invalid literal for int() with base 10: "{\'port\': \'5432\', \'host\': \'127.0.0.1\', \'password\': \'cohondob\', \'user\': \'postgres\', \'database\': \'testdb\'}"',)
```
嚇死寶寶了！！整個 `dict` 的內容被完整的吐出來了，等等越想越可怕，他剛剛做了什麼？他去執行了 `str()` 這個函式誒，所以我猜想的沒錯，我輸入`record_costs(1)`應該也會執行。
```
please input your cost :record_costs(1)
status code: 200
status code: 200
```
果不其然真的跑了(抖，這代表只要程式中有，而且我可以寫成一行，我就可以輸入任意片段去執行我想要做的事！！！！！
## 修正篇
這邊調查了一下 `input()` 這個函式。
* Python3.x 中 `input()` 函數接受一個標準輸入數據，返回為 `string` 類型。
* Python2.x 中 `input()` 相等於 `eval(raw_input(prompt))`

所以如果用的是 Python3.x 就沒有什麼問題他預設返回 string 類型，但 Python2.x 建議改為 `raw_input()` 讓他返回為 `string` 類型，除非這是你要的功能 :smirk:。

