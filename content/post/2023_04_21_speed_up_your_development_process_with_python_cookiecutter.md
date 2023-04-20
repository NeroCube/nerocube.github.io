---
layout:         post
title:        "Speed up your development process with Python Cookiecutter"
subtitle:     "Templates is all you need"
URL:          "2023/04/21/speed_up_your_development_process_with_python_cookiecutter/"
date:         "2023-04-21 00:57:38"
image:        "img/post-bg-os-metro.jpg"
author:       "Nero"
showonlyimage: true
published:     false 
tags:          
    - Python
    - 學習筆記
 
description: "使用 Python Cookiecutter 站在巨人的肩膀上"

---
{< myblockquote "colorquote info" "使用 Python Cookiecutter 站在巨人的肩膀上" >}

## Background
如果曾經想過這些問題也許你可以試試 Cookiecutter。
1. 寫技術部落格大架構跟設定都是類似的每次都要從頭寫，複製貼上又少改到什麼。
2. 想要建立一個 Open Github Source Project 但對這個語言是新手，不知一個目錄架構該長成怎樣，自己還不知道什麼。
3. 專案要 fan-out 但大家的 coding style 都不同，是否能有一致的標準與規範且共用的功能架構不用重複造輪子。

Cookiecutter 可以幫助解決以下問題：
1. 重複性的項目設置：Cookiecutter 可以幫助快速生成一個新的項目目錄結構，這樣您就不必每次都從頭開始設置一個新的項目。這可以節省大量時間和精力。
2. 項目標準化：Cookiecutter 可以讓您在生成新的項目時，使用預定義的模板和文件結構，這樣您可以確保所有的項目都具有一致的標準和結構。這有助於使您的項目更容易維護和擴展。
3. 模板化的代碼生成：Cookiecutter 可以將代碼中的變量提取出來，並使用 Jinja2 模板語言生成定制化的代碼。這可以幫助您快速生成常見的代碼塊，並將其集成到您的項目中。

## Solution

### Features
- 跨平台：官方支持Windows、Mac、Linux。

- 無需了解/寫 Python 代碼即可使用 Cookiecutter。

- 適用於 Python 3.7、3.8、3.9、3.10

- 專案模板可以採用任何編程語言或標記格式：Python、JavaScript、Ruby、CoffeeScript、RST、Markdown、CSS、HTML。可以在同一項目模板中使用多種語言。

### 環境要求
- Python 2.7 或更高版本
- Cookiecutter Libary

### 安裝步驟
首先需要安裝 Python Cookiecutter。可以使用 pip 工具來安裝它，如下所示，安裝好後輸入 `cookiecutter --version` 確認是否安裝成功。

```
pip install cookiecutter

cookiecutter --version
```
### 導入既有模板 
1. 需要選擇一個合適的 Cookiecutter 模板。可以通過在網絡上搜索來查找可用的模板，或者使用 [Cookiecutter Templates](https://www.cookiecutter.io/templates) 來快速開始項目。

<img width="913" alt="image" src="https://user-images.githubusercontent.com/8331034/233442886-1c510cb0-3950-4196-a891-d0100c716ede.png">

2. 找到了想要使用的模板，就可以下指令使用。
- Github
```bash
cookiecutter https://github.com/{GitHub_Username}/{Repo_Name}
```
- Local
```bash
cookiecutter cookiecutter-pypackage/
```
- Python
```python
from cookiecutter.main import cookiecutter

# Create project from the cookiecutter-pypackage/ template
cookiecutter('cookiecutter-pypackage/')

# Create project from the cookiecutter-pypackage.git repo template
cookiecutter('https://github.com/audreyfeldroy/cookiecutter-pypackage.git')
```
3. 執行上述命令後，Cookiecutter 會提示輸入一些基本信息，例如項目名稱、作者名稱、許可證等。這些信息將用於生成您的項目。如果不想輸入可以使用`--no-input`，否則系統會提示輸入。
4. 輸入完所有必要的信息後，Cookiecutter 將為生成一個基本的 Python 項目模板。可以按照模板中的文件結構來編寫您的代碼，也可以根據您的需要進行修改。

5. 最後，可以使用喜歡的開發工具打開生成的項目，開始編寫您的代碼。

### 客製化模板 
模板 100% 使用 [Jinja](https://jinja.palletsprojects.com/en/3.1.x/) 完成，目錄名和文件名都可以模板化。例如：
```
{{cookiecutter.repo_name}}/{{cookiecutter.repo_name}}/{{cookiecutter.repo_name}}.py
```
只需在文件中定義模板變量 `cookiecutter.json`，例如：
```json
{
  "full_name": "Nero Chen",
  "email": "nerocube.tw@gmail.com",
  "project_name": "Complexity",
  "repo_name": "complexity",
  "project_short_description": "Nero blog generator.",
  "release_date": "2013-07-10",
  "year": "2013",
  "version": "0.1.1"
}
```
預設是讀取 `cookiecutter.json` ，跨平台支持 `~/.cookiecutterrc`。
```json
default_context:
  full_name: "Nero Chen"
  email: "nerocube.tw@gmail.com"
  github_username: "NeroCube"
cookiecutters_dir: "~/.cookiecutters/"
```
## Reference
- [Cookiecutter PyPI](https://pypi.python.org/pypi/cookiecutter)
- [Cookiecutter GitHub](https://github.com/cookiecutter/cookiecutter)
- [Cookiecutter Templates](https://www.cookiecutter.io/templates)
- [Cookiecutter Documentation](https://cookiecutter.readthedocs.io)
