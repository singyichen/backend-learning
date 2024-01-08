---
title: Web Server
description: 
published: true
date: 2024-01-08T07:18:48.583Z
tags: web server
editor: markdown
dateCreated: 2023-05-18T07:36:51.367Z
---

# Web Server
- [ ] [[網際網路] Web Server 是什麼](https://pjchender.dev/internet/internet-webserver/)
- [ ] [Day4– 前端小字典三十天【每日一字】– Web Server](https://ithelp.ithome.com.tw/articles/10158054)
- [ ] [何謂網路伺服器？](https://developer.mozilla.org/zh-TW/docs/Learn/Common_questions/Web_mechanics/What_is_a_web_server)

## 比較表
> Web Server 和 Application Server 是兩種重要的伺服器軟體，用於提供網站和應用程式的服務。

| 特徵 | Web Server | Application Server |
|:--:|:--:|:--:|
| 主要功能 | 處理靜態資源、負載平衡和代理 | 處理動態資源和業務邏輯 |
| 主要用途 | 處理大量靜態檔案的場景 | 需要執行較複雜的業務邏輯的場景 |
| 內容生成 | 直接從檔案系統提供內容 | 與資料庫和其他應用程式互動生成內容 |
| 負責處理 | 將請求轉發到應用程式伺服器，並將應用程式伺服器的回應返回給客戶端 | 處理應用程式的業務邏輯，包括資料庫操作、計算、資料處理等 |
| 處理請求 | 處理 HTTP 請求，傳送檔案 | 處理多種協議（HTTP、FTP、SMTP 等），執行應用程式邏輯 |
| 處理請求的過程 | 通常較快 | 可能較慢 |
| 可用性 | 通常使用輕量級的伺服器軟體，如 Nginx、Apache | 可以使用各種程式語言建立的伺服器 |
| 與其他服務的互動 | 不直接與資料庫、快取等其他服務進行互動 | 可以直接與資料庫、快取等其他服務進行互動 |
| 設定方式 | 一般情況下，可以通過組態檔進行簡單的設定 | 需要編寫程式碼實現業務邏輯和功能的定製化 |
| 可靠性和擴展性 | 可以通過反向代理和負載平衡提高可靠性和擴展性 | 可以通過叢集和分佈式部署提高可靠性和擴展性 |
| 部署位置 | 通常運行在前端，處理請求的第一層 | 通常運行在後端，處理請求的第二層 |
| 常見用途 | 架設網站、部落格、線上商店 | 電子商務平台、內容管理系統、線上學習平台 |

## Nginx
這是一套開源的 web server 服務，其中包含了反向代理、負載平衡、郵件代理（mail proxy）、HTTP 快取等功能，知名的使用者包括 Dropbox, Netflix 和 Zynga。

## Apache HTTP Server
又稱作 Apache，是另一個非常熱門的開源免費 web server。

## Apache Tomcat
主要提供 Java application server 使用

## Glassfish
主要提供 Java application server 使用