---
title: Web Server
description: 
published: true
date: 2024-01-19T03:14:48.714Z
tags: web server
editor: markdown
dateCreated: 2023-05-18T07:36:51.367Z
---

# Web Server
- [ ] [[網際網路] Web Server 是什麼](https://pjchender.dev/internet/internet-webserver/)
- [ ] [Day4– 前端小字典三十天【每日一字】– Web Server](https://ithelp.ithome.com.tw/articles/10158054)
- [ ] [何謂網路伺服器？](https://developer.mozilla.org/zh-TW/docs/Learn/Common_questions/Web_mechanics/What_is_a_web_server)
- [ ] [Comparing Caddy to Nginx and Apache](https://dev.to/shingaiz/comparing-caddy-to-nginx-and-apache-iok)
- [ ] [Protect Multiple NAS Devices with HTTPS Security](https://www.asustor.com/admv2?type=2&subject=8&sub=153&lan=en)
## 比較表 (Web Server 和 Application Server)
> Web Server 和 Application Server 是兩種重要的伺服器軟體，用於提供網站和應用程式的服務。

| 特徵 | Web Server | Application Server |
|:--:|:--:|:--:|
| 主要功能 | 處理靜態資源、負載平衡和代理 | 處理動態資源和業務邏輯 |
| 主要用途 | 處理大量靜態檔案的場景 | 需要執行較複雜的業務邏輯的場景 |
| 內容生成 | 直接從檔案系統提供內容 | 與資料庫和其他應用程式互動生成內容 |
| 負責處理 | 將請求轉發到應用程式伺服器，並將應用程式伺服器的回應返回給客戶端 | 處理應用程式的業務邏輯，包括資料庫操作、計算、資料處理等 |
| 處理請求 | 處理 HTTP 請求，傳送檔案 | 處理多種協議（HTTP、FTP、SMTP 等），執行應用程式邏輯 |
| 處理請求的過程 | 通常較快 | 可能較慢 |
| 多執行緒 | 通常不使用多執行緒 | 使用多執行緒，以同時處理請求 |
| 可用性 | 通常使用輕量級的伺服器軟體，如 Nginx、Apache | 可以使用各種程式語言建立的伺服器 |
| 與其他服務的互動 | 不直接與資料庫、快取等其他服務進行互動 | 可以直接與資料庫、快取等其他服務進行互動 |
| 設定方式 | 一般情況下，可以通過組態檔進行簡單的設定 | 需要編寫程式碼實現業務邏輯和功能的定製化 |
| 可靠性和擴展性 | 可以通過反向代理和負載平衡提高可靠性和擴展性 | 可以通過叢集和分佈式部署提高可靠性和擴展性 |
| 部署位置 | 通常運行在前端，處理請求的第一層 | 通常運行在後端，處理請求的第二層 |
| 常見用途 | 架設網站、部落格、線上商店 | 電子商務平台、內容管理系統、線上學習平台 |


## 比較表 (Caddy、Nginx、Apache)
| Web Server | Caddy | Nginx | Apache |
|:--:|:--:|:--:|:--:|
| 開發語言 | Go | C | C |
| 授權條款 | Apache 2.0 | 2-Clause BSD-like | Apache 2.0 |
| 自動 HTTPS | 是（預設支援） | 需要額外模組和設定 | 需要額外模組和設定 |
| HTTP/2 和 HTTP/3 支援 | 支援 | 支援 | 支援 |
| 設定方法 | Caddyfile | nginx.conf | .htaccess 和 httpd.conf |
| 反向代理 | 原生支援 | 原生支援 | 需要 mod_proxy 模組 |
| 負載平衡 | 原生支援 | 原生支援 | 需要 mod_proxy_balancer 模組 |
| 模組/外掛系統 | 支援（動態載入） | 支援（通常靜態編譯） | 支援（動態載入） |
| 效能 | 高（特別是在預設設定下） | 高 | 中等（但可以優化） |
| 安全性 | 以安全為設計目標（預設啟用 HTTPS） | 安全，但需要注意設定和模組 | 安全，但需要注意設定和模組 |
| 易用性 | 高（自動 HTTPS，簡單設定）| 中等（設定較為複雜）| 低（設定和模組管理較為複雜） |
| 初學者友善 | 是（自動 HTTPS，簡單設定） | 否 | 否 |
| 跨平台 | 是 | 是 | 是 |

## Nginx
這是一套開源的 web server 服務，其中包含了反向代理、負載平衡、郵件代理（mail proxy）、HTTP 快取等功能，知名的使用者包括 Dropbox, Netflix 和 Zynga。

## Apache HTTP Server
又稱作 Apache，是另一個非常熱門的開源免費 web server。

## Apache Tomcat
主要提供 Java application server 使用

## Glassfish
主要提供 Java application server 使用