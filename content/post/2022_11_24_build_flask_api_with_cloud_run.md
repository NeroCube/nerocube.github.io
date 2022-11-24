---
layout:     post
title:      "實作將 Flask 部署到 Cloud Run"
subtitle:   "使用 GCP 全代管的無伺服器"
URL: "/2022/11/24/build_flask_api_with_cloud_run/"
date:       2022-11-24 10:00:00
image:      "img/post-bg-os-metro.jpg"
author:     "Nero"
showonlyimage: true
published:  true 
tags:
    - GCP
    - Python
    - 學習筆記

description: "使用 Docker 建立一個簡易的 Flask API 並部署到 Cloud Run。"

---

{{< myblockquote "colorquote info" "使用 Docker 建立一個簡易的 Flask API 並部署到 Cloud Run。" >}}

## 前置作業
1. 在 Google Cloud Console 中頁面上[選擇或創建一個 Google Cloud 項目](https://console.cloud.google.com/projectselector2/home/dashboard)。
1. 確保 Cloud 項目已啟用結算功能。

1. 安裝 Google Cloud CLI

    選擇對應 OS 的版本下載並用 `python3 -V` 確認版本是Python 3（3.5 到3.9）。
    <img width="868" alt="image" src="https://user-images.githubusercontent.com/8331034/203817504-5d82ce67-d6e0-4b65-8555-1281c34d8b81.png">

    執行以下指令安裝，結束後打開新的 Terminal 使更改生效。
    ```
    ./google-cloud-sdk/install.sh
    ```
    如果需要初始化 gcloud CLI，執行以下指令
    ```
    ./google-cloud-sdk/bin/gcloud init
    ```
1. 初始化專案
    將 `PROJECT_ID` 替換為你項目的 ID。
    ```
    gcloud config set project PROJECT_ID
    ```
## 範例實作
Cloud Run 範例專案結構如下：
```
.
├── Dockerfile
├── LICENSE
├── README.md
├── main.py
└── requirements.txt
```
### Flask API
建立 main.py 檔案，其內容如下：
``` python
import os

from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    name = os.environ.get("NAME", "World")
    return "Hello {}!".format(name)

if __name__ == "__main__":
    app.run(debug=True, host="0.0.0.0", port=int(os.environ.get("PORT", 8080)))
```
### Python 套件
建立 requirements.txt 檔案，其內容如下：
```
Flask==2.1.0
gunicorn==20.1.0
```
加入範例所需的套件。

### Dockerfile
建立 Dockerfile 檔案，其內容如下：
```
# Use the official lightweight Python image.
# https://hub.docker.com/_/python
FROM python:3.10-slim

# Allow statements and log messages to immediately appear in the Knative logs
ENV PYTHONUNBUFFERED True

# Copy local code to the container image.
ENV APP_HOME /app
WORKDIR $APP_HOME
COPY . ./

# Install production dependencies.
RUN pip install --no-cache-dir -r requirements.txt

# Run the web service on container startup. Here we use the gunicorn
# webserver, with one worker process and 8 threads.
# For environments with multiple CPU cores, increase the number of workers
# to be equal to the cores available.
# Timeout is set to 0 to disable the timeouts of the workers to allow Cloud Run to handle instance scaling.
CMD exec gunicorn --bind :$PORT --workers 1 --threads 8 --timeout 0 main:app
```
以 Gunicorn 在 PORT 環境變量上建立 Flask 服務

## 署到 Cloud Run
將代碼上傳到 Artifact Registry 並部署到 Cloud Run

```
gcloud run deploy
```
1. 如果系統提示啟用 API，請輸入 `y` 進行啟用。

1. 當系統提示輸入代碼位置時，請按 `Enter` 部署當前資料夾。

1. 當系統提示輸入服務名稱時，請按 `Enter` 接受默認名稱 `gcp-cloud-run-example`。

1. 如果系統提示您啟用 Artifact Registry API，請按 `y` 。

1. 當系統提示輸入區域時：選擇你的區域，例如 `us-central1`。

1. 系統會提示是否允許未通過身份驗證的呼叫：輸入 `y`。

1. 然後等待部署完成。Terminal 會顯示服務網址。

```
gcloud run deploy
Deploying from source. To deploy a container use [--image]. See https://cloud.google.com/run/docs/deploying-source-code for more details.
Source code location (/Users/nero/Desktop/Github/gcp_cloud_run_example):
Next time, use `gcloud run deploy --source .` to deploy the current directory.

Service name (gcpcloudrunexample):  gcp-cloud-run-example
Please specify a region:
 [1] asia-east1
 [2] asia-east2
 [3] asia-northeast1
 [4] asia-northeast2
 [5] asia-northeast3
 [6] asia-south1
 [7] asia-south2
 [8] asia-southeast1
 [9] asia-southeast2
 [10] australia-southeast1
 [11] australia-southeast2
 [12] europe-central2
 [13] europe-north1
 [14] europe-southwest1
 [15] europe-west1
 [16] europe-west2
 [17] europe-west3
 [18] europe-west4
 [19] europe-west6
 [20] europe-west8
 [21] europe-west9
 [22] me-west1
 [23] northamerica-northeast1
 [24] northamerica-northeast2
 [25] southamerica-east1
 [26] southamerica-west1
 [27] us-central1
 [28] us-east1
 [29] us-east4
 [30] us-east5
 [31] us-south1
 [32] us-west1
 [33] us-west2
 [34] us-west3
 [35] us-west4
 [36] cancel
Please enter numeric choice or text value (must exactly match list item):
Please enter a value between 1 and 36, or a value present in the list:  us-central1

To make this the default region, run `gcloud config set run/region us-central1`.

API [artifactregistry.googleapis.com] not enabled on project [135458577460]. Would you like to enable and retry (this will take a few minutes)? (y/N)?  y

Enabling service [artifactregistry.googleapis.com] on project [135458577460]...
Operation "operations/acat.p2-135458577460-c07540ac-6cc9-4503-9051-d77ccdf562b5" finished successfully.
Deploying from source requires an Artifact Registry Docker repository to store built containers. A repository named [cloud-run-source-deploy] in region
[us-central1] will be created.

Do you want to continue (Y/n)?  y

This command is equivalent to running `gcloud builds submit --tag [IMAGE] /Users/nero/Desktop/Github/gcp_cloud_run_example` and `gcloud run deploy gcp-cloud-run-example --image [IMAGE]`

Allow unauthenticated invocations to [gcp-cloud-run-example] (y/N)?  y

Building using Dockerfile and deploying container to Cloud Run service [gcp-cloud-run-example] in project [galvanic-veld-316101] region [us-central1]
⠶ Building and deploying new service... Uploading sources.
  ✓ Creating Container Repository...
  ✓ Uploading sources...
⠶ Building and deploying new service... Uploading sources.
✓ Building and deploying new service... Done.
  . Routing traffic...
  . Setting IAM Policy...
  ✓ Building Container... Logs are available at [https://console.cloud.google.com/cloud-build/builds/74608788-0e2d-45c2-af7e-a116d3042acd?project=135458577460
  ].
  ✓ Creating Revision... Waiting for container to become healthy.
  ✓ Routing traffic...
  ✓ Setting IAM Policy...
Done.
Service [gcp-cloud-run-example] revision [gcp-cloud-run-example-00001-haw] has been deployed and is serving 100 percent of traffic.
Service URL: https://gcp-cloud-run-example-5tnsyx4dqq-uc.a.run.app
```
<img width="598" alt="image" src="https://user-images.githubusercontent.com/8331034/203814492-339e9ae2-d587-473c-b4d9-bac4a01b972c.png">

## 使用 Local Docker Image 部署至 Cloud Run
使用 Docker 在你的主機上 build image
```
docker build -t gcp-cloud-run-example:latest .
```
在 local 上建置完成的 docker image 進行測試
```
docker run --rm -p 8080:8080 gcp-cloud-run-example:latest
```
使用 Cloud build 建置 docker image 並推到 Artifact Registry
```
gcloud builds submit --tag asia.gcr.io/{project_id}/gcp-cloud-run-example
```
將 Image 部署到 Cloud Run

```
$ gcloud run deploy --image asia.gcr.io/{project_id}/gcp-cloud-run-example \
--platform managed \
--port 8080 \
--memory 1Gi \
--timeout=2m \
```
## 注意
雖然 Cloud Run 不會對未在使用中的服務計費，但您可能仍然需要支付將容器映像存儲在 Artifact Registry 中而產生的相關費用。為避免產生費用，您可以刪除容器映像或刪除Cloud 項目。刪除Cloud 項目後，系統即會停止對該項目中使用的所有資源計費。

### Project ID 要求：

- 長度必須為 6 到 30 個字符。
- 只能包含小寫字母、數字和連字符(不能底線)。
- 必須以字母開頭。
- 不能以連字符結尾。
- 無法使用或以前使用過，包括已刪除的項目。
- 不能包含受限制的字符串，如 google 和ssl。

## Reference
- [https://cloud.google.com/sdk/docs/install](https://cloud.google.com/sdk/docs/install)
- [https://cloud.google.com/resource-manager/docs/creating-managing-projects](https://cloud.google.com/resource-manager/docs/creating-managing-projects)
- [https://cloud.google.com/run/docs/quickstarts/build-and-deploy/deploy-python-service](https://cloud.google.com/run/docs/quickstarts/build-and-deploy/deploy-python-service)

