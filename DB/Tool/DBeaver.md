---
title: DBeaver
description: 資料庫管理工具
published: true
date: 2023-07-24T08:56:37.879Z
tags: 
editor: markdown
dateCreated: 2023-07-21T01:54:06.956Z
---

# DBeaver
- [ ] [dbeaver](https://dbeaver.io/)
- [ ] [免費的跨平台資料庫管理工具-DBeaver](https://medium.com/@softjobdays/%E5%85%8D%E8%B2%BB%E7%9A%84%E8%B7%A8%E5%B9%B3%E5%8F%B0%E8%B3%87%E6%96%99%E5%BA%AB%E7%AE%A1%E7%90%86%E5%B7%A5%E5%85%B7-dbeaver-ca823eeccc2c)

# 關於 DBeaver
DBeaver 是一個使用 Java 語言開發基於 Eclipse 平台的跨平台通用資料庫管理工具，支援 MSSQL、MySQL、Oracle、PostgreSQL、DB2、Sybase、HSQLDB、H2 等其他支援 JDBC 的資料庫，對於其他資料庫（NoSQL），它使用專用的資料庫驅動程式。

DBeaver 提供一個圖形介面來查看資料庫結構，執行 SQL 查詢，瀏覽及編輯資料。

![DBeaver logo.png](http://192.168.25.60:8000/files/file_storage/d8762414.png)

# 安裝
## 到 [官網](https://dbeaver.io/download/) 下載安裝檔案

- 點選 `Windows (installer)` 下載安裝檔案 `dbeaver-ce-23.1.2-x86_64-setup.exe` 
- 語言選取英文並直接安裝

# 新增資料庫連線
New Database Connection ➔ 選擇要連線的資料庫

## SQL Server (2008)
### 輸入連線資訊 ➔ 連線測試( Test Connection )
- database：`SQL Server(Old driver,jTDS)`
- Host：`192.168.25.21`
- Database/Schema：`Leader`
- URL：`jdbc:jtds:sqlserver://192.168.25.21/Leader` (會自動生成)
- Username：`sa` 
- Password：`Password`

![dbeaver connect mssql.png](http://192.168.25.60:8000/files/file_storage/ba6a8c5b.png)

### 查看 Driver 資訊 ➔ 點選 Driver Settings


## PostgreSQL
- database：



## SQLite
- database：