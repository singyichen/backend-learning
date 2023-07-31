---
title: Controller Service
description: 
published: true
date: 2023-07-24T06:26:39.794Z
tags: nifi
editor: markdown
dateCreated: 2023-07-14T02:35:59.873Z
---

# Controller Service
有時候我們會需要從 Database、DatawareHouse 或是雲端的服務來取得資料，進而透過我們在 NiFi 建立的 Data Pipeline 來做一連串的處理步驟，最後在寫入。但是如果同時有多個 Data Pipieline 都需要對同一個 DB 或是一個 Datasource 建立多個連線的話，則對於另一端可能會造成不可預期的影響。所以若能透過同一一個 Object 來建立其中的連線的話，對於 Pipeline 的處理一方面更加單純，另一方面對於提供資料的那一端也不會有太多不必要且重複的網路連線。

Controller Service 大概的特性與分類：

## 與 DB、Cloud 建立連線的設定

### AWSCredentialsProviderControllerServcie
可讓使用者設定好 access_key_id，secret_access_key 來存取對應的 AWS 資源

### DBConnectionPool
使用者可以指定好 DB 的 JDBC Driver 之路徑、class 與 Connection URL，就可以讓 NiFi 對於 DB 來做存取。

ex：MySQL，AWS Athena，AWS Redshift等

當然還有很多其他原生提供的 Controller Service

ex：CassandraSessionProvider，RedisConnectionPoolService，HiveConnectionPool，GCPCredentialsControllerService 等這些都是比較常用到的，而運作原理也跟我上述的描述雷同。

## 讀取或寫入特定 format 的設定
Controller Service 的另外一種比較常見的用法是針對特定檔案格式的 Reader 和 Writer，讓我們可以去利用對應的檔案格式讀取或寫入檔案：

### Reader
常見的有 AvroReader，CsvReader，JsonTreeReader，ParquetReader，XMLReader 等

### Writer
常見的有 AvroRecordSetWriter，CSVRecordSetWriter，JsonRecordSetWriter，ParquetRecordSetWriter，XMLRecordSetWriter 等

如此一來 NiFi 就可以根據不同的檔案格式去解析與寫入對應的資料檔案。

# 新增 Controller Service
在主頁的左方的『齒輪符號』，接者選擇 『CONTROLLER SERVICES』，且點選右手邊的『+』符號，接著就會跳出所有 Controller Service的種類，與 Processors 的選擇畫面雷同，我們就可以在上面選擇要的 Controller Service

![nifi add controller service.gif](http://192.168.25.60:8000/files/file_storage/688f604d.gif)

# 設定 Controller Service
## local_sql_server_conn
### 新增一個 `DBCPConnectionPool` 連接 `MSSQL`
- Database Connection URL：`jdbc:jtds:sqlserver://192.168.25.21:1433/Leader`
- Database Driver Class Name：`net.sourceforge.jtds.jdbc.Driver`
- Database Driver Location(s)：`/opt/nifi/nifi-current/lib/jtds-1.3.1.jar`
- Database User：`sa`
- Password ：`password`
- Validation query：`Select 1`

### 連接測試
點選 `打勾符號` ( Verify Properties ) 進行連接測試 ➔ 測試成功 ➔ 點選 `APPLY`

![nifi controller service connect mssql success.png](http://192.168.25.60:8000/files/file_storage/ad309e11.png)

### 將 Service enabled
點選 `閃電符號` ( Enable ) ➔ 點選 `ENABLE`

![nifi controller service connect mssql enabled.png](http://192.168.25.60:8000/files/file_storage/6707fab3.png)

## local_postgres_conn
### 新增一個 `DBCPConnectionPool` 連接 `PostgresSQL`
- Database Connection URL：`jdbc:postgresql://192.168.25.59:5432/A_SY004`
- Database Driver Class Name：`org.postgresql.Driver`
- Database Driver Location(s)：`/opt/nifi/nifi-current/lib/postgresql-42.5.1.jar`
- Database User：`11542`
- Password ：`password`
- Validation query：`Select 1`

### 連接測試
點選 `打勾符號` ( Verify Properties ) 進行連接測試 ➔ 測試成功 ➔ 點選 `APPLY`

![nifi controller service connect postgres success.png](http://192.168.25.60:8000/files/file_storage/cb0847f5.png)

### 將 Service enabled
點選 `閃電符號` ( Enable ) ➔ 點選 `ENABLE`

![nifi controller service connect postgres enabled.png](http://192.168.25.60:8000/files/file_storage/9bada704.png)