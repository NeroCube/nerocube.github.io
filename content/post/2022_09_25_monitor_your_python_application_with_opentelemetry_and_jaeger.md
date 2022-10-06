---
layout:     post
title:      "Monitor your Python application with OpenTelemetry and Jaeger"
subtitle:   " \"OpenTelemetry is all you need\""
URL: "/2022/09/25/monitor_your_python_application_with_opentelemetry_and_jaeger/"
date:       2022-09-25 10:00:00
image:      "img/post-bg-os-metro.jpg"
author:     "Nero"
showonlyimage: true
published:  true 
tags:
    - Monitor
    - Python
    - 學習筆記

description: "使用 OpenTelemetry 和 Jaeger 監控您的 Python 應用程式"

---

{{< myblockquote "colorquote info" "“使用 OpenTelemetry 和 Jaeger 監控您的 Python 應用程式”" >}}

有關更詳細的範例，請[參考範例](https://github.com/open-telemetry/opentelemetry-python/tree/main/docs/examples)。

## 痛點
1. 微服務架構使開發人員能夠更快、更獨立地構建和發佈服務，不再受制於與單體架構相關的複雜發布流程。但隨著這些現在分佈式系統的擴展，開發人員越來越難以了解他們自己的服務如何依賴或影響其他服務，尤其是在部署之後，服務中斷時如何快速和準確地找到問題點成為了一個難解的議題。為了使系統可觀察，必須對其進行檢測。也就是說必須將檢測資料 (trace 、 metrics 和 logs) 發送到 Observability 後端。有許多 Observability 後端，從開源工具（例如 Jaeger 和 Zipkin）到商業 SAAS 產品。

2. 過去檢測程式碼的方式會有所不同，因為每個 Observability 後端都有自己的檢測邏輯，這意味著沒有用於將資料發送到 Observability 後端的標準化格式。所以如果一家公司選擇切換 Observability 後端，這意味著他們必須重新改寫他們的程式並配置新代理，才能將遙測資料發送到選擇的新工具。由於缺乏標準化，最終結果是缺乏資料可移植性和用戶維護儀器庫的負擔。

## 什麼是 OpenTelemetry？
認識到標準化的必要性，雲端社群走到了一起，並誕生了兩個開源項目：[OpenTracing](https://opentracing.io) ([雲原生計算基金會 (CNCF)項目](https://www.cncf.io)）和 [OpenCensus](https://opencensus.io)（[Google 開源社群](https://opensource.google/)項目）。

- **OpenTracing** 提供了一個供應商中立的 API，用於將遙測數據發送到 Observability 後端；但是，它依賴於開發人員實現他們自己的 library 來滿足規範。

- **OpenCensus** 提供了一組特定於語言的 library，開發人員可以使用這些 library 來檢測他們的程式並發送到他們支持的任何一個後端。

OpenCensus 和 OpenTracing 在 [2019 年 5 月](https://www.cncf.io/blog/2021/08/26/opentelemetry-becomes-a-cncf-incubating-project/)合併形成了 OpenTelemetry（簡稱 OTel） 。作為一個 CNCF 孵化項目，OpenTelemetry 將兩全其美。

OTel 的目標是提供一組與供應商無關的標準化 SDK、API 和 工具，用於攝取、轉換和發送資料到 Observability 後端（即開源或商業供應商）。OpenTelemetry 不是像 Jaeger 或 Prometheus 那樣的可觀察性後端。相反的它支持將資料導出到各種開源和商業後端。提供了一個可插拔的架構，因此可以輕鬆添加其他技術協議和格式。


## Hello World OpenTelemetry

首先安裝 opentelemetry API 和 SDK：

```bash
pip install opentelemetry-api
pip install opentelemetry-sdk
```

安裝後可以使用這些包從你的應用程序中發出跨度 (Spans) 。跨度 (Spans) 表示應用程序中要檢測的操作，例如 HTTP 請求或資料庫調用。檢測後可以提取有用的信息，例如操作花費了多長時間還可以向跨度添加任意屬性提供更多洞察力。

以下示例程序發出個一包含三個跨度 (Spans) 的追踪 (Trace)： `foo` 、 `bar` 和 `baz` ：

```python
# tracing_example.py
from opentelemetry import trace
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import (
    BatchSpanProcessor,
    ConsoleSpanExporter,
)

trace.set_tracer_provider(TracerProvider())
trace.get_tracer_provider().add_span_processor(
    BatchSpanProcessor(ConsoleSpanExporter())
)
tracer = trace.get_tracer(__name__)
with tracer.start_as_current_span("foo"):
    with tracer.start_as_current_span("bar"):
        with tracer.start_as_current_span("baz"):
            print("Hello world from OpenTelemetry Python!")
```

運行程序後可以看到追踪 (Trace) 顯示在控制台：

```python
$ python tracing_example.py
{
    "name": "baz",
    "context": {
        "trace_id": "0xb51058883c02f880111c959f3aa786a2",
        "span_id": "0xb2fa4c39f5f35e13",
        "trace_state": "{}"
    },
    "kind": "SpanKind.INTERNAL",
    "parent_id": "0x77e577e6a8813bf4",
    "start_time": "2020-05-07T14:39:52.906272Z",
    "end_time": "2020-05-07T14:39:52.906343Z",
    "status": {
        "status_code": "OK"
    },
    "attributes": {},
    "events": [],
    "links": []
}
{
    "name": "bar",
    "context": {
        "trace_id": "0xb51058883c02f880111c959f3aa786a2",
        "span_id": "0x77e577e6a8813bf4",
        "trace_state": "{}"
    },
    "kind": "SpanKind.INTERNAL",
    "parent_id": "0x3791d950cc5140c5",
    "start_time": "2020-05-07T14:39:52.906230Z",
    "end_time": "2020-05-07T14:39:52.906601Z",
    "status": {
        "status_code": "OK"
    },
    "attributes": {},
    "events": [],
    "links": []
}
{
    "name": "foo",
    "context": {
        "trace_id": "0xb51058883c02f880111c959f3aa786a2",
        "span_id": "0x3791d950cc5140c5",
        "trace_state": "{}"
    },
    "kind": "SpanKind.INTERNAL",
    "parent_id": null,
    "start_time": "2020-05-07T14:39:52.906157Z",
    "end_time": "2020-05-07T14:39:52.906743Z",
    "status": {
        "status_code": "OK"
    },
    "attributes": {},
    "events": [],
    "links": []
}
```
每個跨度 (Spans) 代表單個操作或工作單元。跨度 (Spans) 可以鑲嵌並且與其他跨度 (Spans) 具有父子關係。當給定的跨度 (Spans) 處於活動狀態時，新創建的跨度 (Spans) 會繼承活動跨度 (Spans) 的追踪 ID、選項和其上下文的其他屬性。沒有父跨度的跨度稱為根跨度，跟追踪一個根跨度及其後代組成。

在此示例中，OpenTelemetry Python 包創建一個包含三個跨度的追踪並將其打印到 STDOUT。

## 配置導出器

前面範例發出了有關所有跨度的資訊，但輸出有點難以閱讀。可以改為將此資料導出到應用程式效能監控後端以進行視覺化和查詢。將來自多個服務的跨度和追踪資訊整合到一個資料庫中也很常見的，因此需要多個服務的操作仍然可以全部一起視覺化。

這種整合跨度和追踪資訊的概念稱為分佈式追踪。其中一個這樣的分佈式追踪後端稱為 Jaeger。Jaeger 項目提供了一個具有 UI、資料庫和使用者一體化的 Docker 容器。

運行以下指令啟動 Jaeger：

```bash
docker run -p 16686:16686 -p 6831:6831/udp jaegertracing/all-in-one
```
此指令在本機端口 16686 上啟動 Jaeger，並在端口 6831 上公開 Jaeger thrift 代理。您可以通過 [http://localhost:16686](http://localhost:16686) 訪問 Jaeger 。

啟動端后後應用程序需要將追踪導出到該系統。雖然 `opentelemetry-sdk` 沒有為 Jaeger 提供導出器，但您可以使用以下指令安裝單獨的包：

```bash
pip install opentelemetry-exporter-jaeger
```
安裝導出器後，更新代碼以導入 Jaeger 導出器並改用它：

```python
# jaeger_example.py
from opentelemetry import trace
from opentelemetry.exporter import jaeger
from opentelemetry.sdk.trace import TracerProvider
from opentelemetry.sdk.trace.export import BatchExportSpanProcessor

trace.set_tracer_provider(TracerProvider())

jaeger_exporter = jaeger.JaegerSpanExporter(
    service_name="my-helloworld-service",
    agent_host_name="localhost",
    agent_port=6831,
)

trace.get_tracer_provider().add_span_processor(
    BatchExportSpanProcessor(jaeger_exporter)
)

tracer = trace.get_tracer(__name__)

with tracer.start_as_current_span("foo"):
    with tracer.start_as_current_span("bar"):
        with tracer.start_as_current_span("baz"):
            print("Hello world from OpenTelemetry Python!")
```
最後運行 Python 程式：

```bash
python jaeger_example.py

```
然後可以訪問 Jaeger UI，在`服務` 下查看你的服務，並找到你的踪跡！
![image](https://user-images.githubusercontent.com/8331034/192150447-10d46390-794c-4e45-ad74-d398f1e4f8bb.png)



## 參考
- [OpenTelemetry Official](https://opentelemetry.io/)
- [Getting Started with OpenTelemetry Python](https://open-telemetry.github.io/opentelemetry-python/getting-started.html)
