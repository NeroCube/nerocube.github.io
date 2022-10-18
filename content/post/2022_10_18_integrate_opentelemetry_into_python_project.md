---
layout:     post
title:      "Integrate OpenTelemetry into Python project"
subtitle:   " \"OpenTelemetry is all you need\""
URL: "/2022/10/18/integrate_opentelemetry_into_python_project/"
date:       2022-10-18 10:00:00
image:      "img/post-bg-os-metro.jpg"
author:     "Nero"
showonlyimage: true
published:  false 
tags:
    - Monitor
    - Python
    - 學習筆記

description: "將 OpenTelemetry 整合到你的 Python 專案"

---

{{< myblockquote "colorquote info" "“將 OpenTelemetry 整合到你的 Python 專案”" >}}

## 痛點
1. 在一個新的技術導入時常常會遇到合作單位或工程師說：『我知道這個機制對系統穩定性與快速找到問題點很有幫助，但我們目前沒有人力去做或沒有人改得動 code 甚至是上萬條的 pipeline 無從下手。』

2. 或聽到這機制很棒，我們也花人力去改了，但收集的 log 太大量導致部門成本過高被 highlight。

## 參考
- [OpenTelemetry Automatic Instrumentation](https://opentelemetry.io/docs/instrumentation/python/automatic/)
- [Opentelemetry Python](https://github.com/open-telemetry/opentelemetry-python/tree/main/docs/examples/auto-instrumentation)
