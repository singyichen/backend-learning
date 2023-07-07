---
title: Airflow
description: Airflow 介紹
published: true
date: 2023-07-07T02:31:59.181Z
tags: airflow
editor: markdown
dateCreated: 2023-05-23T07:49:41.975Z
---

# Airflow
- [ ] [官方文件](https://airflow.apache.org/docs/apache-airflow/stable/index.html)
- [ ] [中文文件](https://airflow.apachecn.org/#/)
- [ ] [youtube](https://www.youtube.com/@ApacheAirflow/videos)
- [ ] [ETL best practices with Airflow documentation site](https://gtoonstra.github.io/etl-with-airflow/index.html#)
- [ ] [運用 Airflow 讓 Python 開發的工作流自動化](https://medium.com/twdsmeetup/%E5%A6%82%E4%BD%95%E8%AE%93%E7%A8%8B%E5%BC%8F%E8%87%AA%E5%8B%95%E8%B7%91%E8%B5%B7%E4%BE%86-c3f7f3783df6)
- [ ] [拋棄混亂無章的工作排程－使用 Airflow 管理](https://www.lukehong.tw/post/scheduling-task-using-airflow-732e27d2ac8a/)
- [ ] [Apache Airflow：工作流程管理控制台](https://tech.hahow.in/apache-airflow-%E5%B7%A5%E4%BD%9C%E6%B5%81%E7%A8%8B%E7%AE%A1%E7%90%86%E6%8E%A7%E5%88%B6%E5%8F%B0-4dc8e6fc1a6a)
- [ ] [一段 Airflow 與資料工程的故事：談如何用 Python 追漫畫連載](https://leemeng.tw/a-story-about-airflow-and-data-engineering-using-how-to-use-python-to-catch-up-with-latest-comics-as-an-example.html)
- [ ] [Airflow 動手玩：（一）簡介](https://tw.coderbridge.com/series/c012cc1c8f9846359bb9b8940d4c10a8/posts/6c90609c64c14df284f141b703c03702)
- [ ] [Airflow 介紹](https://hackmd.io/@JohnCena/Hki7kBTlP)
- [ ] [Apache Airflow 2.1 基礎教學(1)：從超量 cron job 排程工作轉守為攻](https://3lexw.medium.com/apache-airflow-2-1-%E5%9F%BA%E7%A4%8E%E6%95%99%E5%AD%B8-1-%E5%BE%9E%E8%B6%85%E9%87%8F-cron-job-%E6%8E%92%E7%A8%8B%E5%B7%A5%E4%BD%9C%E8%BD%89%E5%AE%88%E7%82%BA%E6%94%BB-c2a523f18708)
- [ ] [Python & Airflow 學習筆記_環境架設](https://ithelp.ithome.com.tw/articles/10286480)
- [ ] [Awesome Apache Airflow](https://github.com/jghoman/awesome-apache-airflow)
- [ ] [英文學習資源](https://academy.astronomer.io/page/apache-airflow)
- [ ] [Understanding Apache Airflow’s key concepts](https://medium.com/@dustinstansbury/understanding-apache-airflows-key-concepts-a96efed52b1a)
- [ ] [How Quizlet uses Apache Airflow in practice](https://medium.com/@dustinstansbury/how-quizlet-uses-apache-airflow-in-practice-a903cbb5626d)
- [ ] [Apache Airflow (一)Intro](https://medium.com/sq-catch-and-note/apache-airflow-%E4%B8%80-intro-27839f177bb5)
- [ ] [Step by step: build a data pipeline with Airflow](https://towardsdatascience.com/step-by-step-build-a-data-pipeline-with-airflow-4f96854f7466)
- [ ] [Airflow introduction and installation: Airflow Tutorial P1](https://www.youtube.com/watch?v=z7xyNOF8tak&ab_channel=coder2j&loop=0)

# 關於 Airflow

![airflow logo.png](http://192.168.25.60:8000/files/file_storage/16343105.png)

Apache Airflow 是一個基於 python 的輕量級調度系統，管理 crontab 表示式進行任務的調度，我們只需要編寫相對應的 python 指令碼，即可完成任務的調度。

Apache Airflow 是一個提供基於 DAG 有向無環圖來編排工作流的、可視化的分佈式任務調度平台，與 Oozie、Azkaban 等任務流調度平台類似。Airflow 在 2014 年由 Airbnb 發起，2016 年 3 月進入 Apache 基金會，在 2019 年 1 月成為頂級項目。Airflow 採用 Python 語言編寫，提供可程式設計方式定義 DAG 工作流，可以定義一組有依賴的任務，按照依賴依次執行， 實現任務管理、調度、監控功能。

另外，Airflow 提供了 WebUI 可視化介面，提供了工作流節點的運行監控，可以查看每個節點的運行狀態、運行耗時、執行日誌等。也可以在介面上對節點的狀態進行操作，如：標記為成功、標記為失敗以及重新運行等。在 Airflow 中工作流上每個 task 都是原子可重試的，一個工作流某個環節的 task 失敗可自動或手動進行重試，不必從頭開始跑。


# 架構圖

![airflow general architecture.png](http://192.168.25.60:8000/files/file_storage/859bee36.png)

![airflow offical architecture.png](http://192.168.25.60:8000/files/file_storage/a1f5bba9.png)

## Metadata Database
Airflow 使用 SQL Database 儲存 meta 資訊。比如 DAG、DAG RUN、task、task instance 等資訊。

## WebServer
WebServer 顧名思義，就是提供網頁功能的伺服器，WebServer 是可以單獨運作的，所以可以被分開部署，但如果只有 WebServer 而沒有下面三個 Components，也就只能查看 DAG、Task 及 Connections 等一些資訊而已。

## Scheduler
Scheduler 負責排程，這裡的排程並不是直接指定哪個 Task 給哪個 Worker 運行，而是從 Metadata Database 中找尋 DAG 跟 Task 的狀態，並判斷是否將哪些 Task 傳送給 Executor 安排執行。

## Executor
Executor 是一個 Queue Process，從 Scheduler 接收要執行的 Task，並將這些資訊存進 Queue，並從 Queue 中取出 Task 安排給閒置的 Worker 執行。如果仔細觀察 WebServer 或是 Scheduler 運行時的資訊，會發現 Terminal 有時會印出 INFO - Using executor SequentialExecutor，代表我們使用的是 SequentialExecutor，SequentialExecutor 通常用於 debug，一次只能運行一個 Task，也是唯一可以跟 SQLite 配合的 Executor，因為 SQLite 不支援多個連線。

除了 SequentialExecutor 還有其他幾個 Executor，可以在 Airflow 的 Source Code 看到，像是 LocalExecutor、CeleryExecutor、KubernetesExecutor 等。

### LocalExecutor
單機版的 Executor，雖然是單機版，但還是可以透過 MultiProcess 同時運行多個 Worker。

### CeleryExecutor
使用 Celery 分散式的 Task Queue 當作 Executor，所以可以在多台電腦同時運行多個 Worker。

### KubernetesExecutor
使用 Kubernetes 當作 Executor，所以每個 Task 都會被包裝成一個 Pod 運行，下下篇我們要將 Airflow 架設在 Kuberentes 上就會用到 KubernetesExecutor。

## Worker
Worker 負責實際執行 Task。

## User interface
Airflow 提供了一個使用者介面，讓您可以查看 DAG（Directed Acyclic Graph，有向非循環圖）及其任務的執行狀態，觸發 DAG 的運行，查看日誌，以及對 DAG 進行一些有限的調試和問題解決。

這個使用者介面通常被稱為 Airflow UI 或 Airflow Web UI。它提供以下功能：

- 查看 DAG 及其任務的執行狀態和進度。
- 觸發 DAG 的手動運行。
- 查看任務的日誌並瀏覽任務細節。
- 進行有限的調試和故障排除工作。
- 存取 DAG 的視覺化表示和依賴關係。
- 監控和管理任務的排程和執行。

Airflow 使用者介面提供了一個便捷的方式來與工作流程進行交互和監控，使得管理和排除數據管道更加容易。

# 元件
> 一個 DAG 是由多個 Tasks 組成，每個 Task 是分開執行的，Task 是 Airflow 執行基礎單位。
## DAG
> DAG（Directed Acyclic Graph，有向無環圖）是 Airflow 的核心概念，將任務集合在一起，按照依賴關係和關聯性組織起來，以指定它們的運行方式。

在 Graph View 頁面，可以看到每個 DAG 都是由多個 Tasks 組成，所以代表我們待會在寫 DAG 時，除了宣告 DAG 物件代表不同的 DAG，同時也會宣告很多 Task 物件來組成 DAG 要執行的內容。

![airflow Directed Acyclic Graph.png](http://192.168.25.60:8000/files/file_storage/08af0462.png)

## Task
> 在 Airflow 中，任務（Task）是執行的基本單位。任務被組織成 DAG（有向無環圖），然後在它們之間設定上游和下游依賴關係，以表達它們應該運行的順序。

在 Grid 或 Graph 的頁面，可以看 Task 的狀態有很多種，如果仔細觀察的話，每個 Task 都是要先經過 queued 的狀態，才會進到 Running 的狀態，而每個 Task 之間又存在著相依的關係。

每個 Task 都是分開執行的，也意味著，即使把 Task 寫在同一份 Python file 裡，也無法透過 Global variable 傳遞 Task 執行的結果，因為每一次執行的 context 是不一樣的。那不同的 Task 之間要如何傳遞執行中產生的變數呢？Airflow 提供了 XCom 功能，將執行的結果存在 Database 裡，供後面執行的 Task 調用前面 Task 執行的結果，在待會的範例中，也會示範如何在 Task 裡面將想傳遞的變數存入 XCom。

![airflow task status.png](http://192.168.25.60:8000/files/file_storage/cac01bf0.png)

![airflow task instance states.png](http://192.168.25.60:8000/files/file_storage/daf22f31.png)


## Task 狀態
最後來看一下，Task 在 Airflow 裡從開始到結束會有哪些狀態。
### No status
當我們手動 Trigger DAG 或是 Scheduler 排程 DAG 後，這時 DAG 的 Task 會先被創造成 Task Instance 並寫進 Database ，這時 Task 的狀態就是 no status。
### Scheduled
當 Scheduler 確認某個 Task 需要被執行時，這時 Task 的狀態就會變成 Scheduled，像是當我們使用 BranchPythonOperator，執行結束後，Scheduler 會就依照執行的結果決定下一個 Task，這時那個 Task 的狀態就會是 Scheduled。
### Queued
當 Scheduler 把確定要執行的 Task 發送給 Executor 時，相當於把 Task 放入 Queue 裡，所以 Task 的狀態會變成 Queued。
### Running
當 Executor 把 Task 發送給閒置的 Worker 時，Task 的狀態就會變成 Running。
### Success/Failed
最後依據 Workere 執行的結果，Executor 會把 Task 標示成 Success 或是 Failed。


## Operators
> 運算子（Operator）在概念上是預定義任務的模板，可以在 DAG 內聲明式地定義，底層對應 python class。不同的 Operator 實現了不同的功能

Airflow提供了非常豐富的運算子（Operator）集合，其中一些是內建在核心或預安裝的提供者中的。一些常用的核心運算子包括：

### BashOperator
執行 Bash 命令

### PythonOperator
調用任意的 Python 函數

### EmailOperator
使用者發送郵件

### HttpOperators
使用者傳送HTTP請求