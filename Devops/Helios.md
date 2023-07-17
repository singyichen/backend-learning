---
title: Helios
description: 
published: true
date: 2023-07-12T02:35:13.758Z
tags: helios
editor: markdown
dateCreated: 2023-07-12T02:18:25.530Z
---

# Helios
- [ ] [Helios Docs](https://docs.gethelios.dev/)
- [ ] [Node.js installation instructions](https://docs.gethelios.dev/docs/nodejs-sdk-installation-instructions)

# 關於 Helios
Helios 是一個開發者平台，旨在使 Node.js 應用程序的監控和故障排除變得輕鬆，提供可操作性的見解。它利用 OpenTelemetry 的上下文傳播框架，在微服務、無服務功能、數據庫和第三方 API 等各個組件中提供全面的可見性。
## 特點
- 支援多種語言，包括 Python、JavaScript、Node.js、Java、Ruby、.NET、Go、C++ 和 Collector。
- 提供對應用程序中數據流的統一視圖。
- 為 Lambda 調用、 HTTP 請求、Kafka 和 RabbitMQ 消息等提供準確的工作流程重現。
- 與現有日誌、測試、錯誤監控等輕鬆集成。
- 對於複雜的數據流可進行可視化，例如無服務調用、消息隊列、事件流、 HTTP 請求和 gRPC 調用。
# 使用步驟
## 官網註冊帳號
- 由 setting 取得 api key

## 安裝套件
```bash
npm install --save helios-opentelemetry-sdk
```

## 設定環境變數
- 需在 git bash 中設定

```bash
export NODE_OPTIONS="--require helios-opentelemetry-sdk"
export HS_TOKEN=6660f82915f1beab9e0d
export HS_SERVICE_NAME=e_board_api
export HS_ENVIRONMENT=process.env.NODE_ENV
```

## 起 api 服務並呼叫 api 進行追蹤
- Helios 啟動成功會顯示
```
Helios tracing initialized (
            service: e_board_api,
            token: 666*****,
            environment: process.env.NODE_ENV)

```

![helios tracing initialized.png](http://192.168.25.60:8000/files/file_storage/cfe3efdd.png)

## 進行 api 監控
- 進入 [Helios server](https://app.gethelios.dev/) 查看

![helios api tracing.png](http://192.168.25.60:8000/files/file_storage/5180f80f.png)




