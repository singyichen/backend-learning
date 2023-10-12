---
title: Migration
description: 
published: true
date: 2023-10-04T01:24:10.929Z
tags: 
editor: markdown
dateCreated: 2023-06-07T01:47:56.437Z
---

# Migration
- [ ] [How to Migrate from Microsoft SQL Server to PostgreSQL](https://www.endpointdev.com/blog/2019/01/migrate-from-sql-server-to-postgresql/)
- [ ] [sqlserver2pgsql](https://github.com/dalibo/sqlserver2pgsql)
- [ ] [將 Microsoft SQL Server Database 資料轉到 PostgreSQL - BCP/Import](https://rainmakerho.github.io/2021/02/05/Migrate-MS-SQL-to-PostgreSQL-bcp/)
- [ ] [將 Microsoft SQL Server Database 資料轉到 PostgreSQL - SSMS Export](https://rainmakerho.github.io/2021/02/04/Migrate-MS-SQL-to-PostgreSQL-1/)
- [ ] [如何將資料從SQL Server 遷移到PostgreSQL？將資料從SQL Server 遷移到PostgreSQL方法分析！](https://www.w3cschool.cn/article/25696537.html)
- [ ] [Migrating from MSSQL to PostgreSQL – What You Should Know](https://severalnines.com/blog/migrating-mssql-postgresql-what-you-should-know)

# 目的
將 SQL Server (MSSQL) 資料庫移轉到 Postgresql 時，須使用 migration 建立 table

![sql server 產製 migration 流程.png](http://192.168.25.60:8000/files/file_storage/0fcbf75b.png)


# 使用 2 個工具
## SQL Server Management Studio ( SSMS )
SSMS 提供工具來設定、監視以及管理SQL Server 執行個體和資料庫。
## sqlserver2pgsql 
sqlserver2pgsql 是用 Perl 編寫的。這是一個開源遷移工具，用於自動將 Microsoft SQL Server 資料庫轉換為 PostgreSQL 資料庫。

# Migration 產製步驟

## 用 SSMS 匯出 MSSQL Table Schema Script
- 選取匯出 DB 按右鍵 ➔ 工作 ➔ 產生指令碼

![ssms 產製 sql script 工作產生指令碼.png](http://192.168.25.60:8000/files/file_storage/ca19e089.png)

- 選擇物件 ➔ 選取特定的資料庫物件 ➔ 勾選資料表 ➔ 下一步

![ssms 產製 sql script 選擇物件選取特定的資料庫物件.png](http://192.168.25.60:8000/files/file_storage/a5cd71fc.png)

- 設定指令碼編寫選項 ➔ 儲存為指令檔 ➔ 單一指令檔 ➔ 匯出檔案名稱 `A_SY004.sql` ➔ 勾選覆寫現有檔案 ➔ 下一步

![ssms 產製 sql script 設定指令碼編寫選項儲存為指令檔單一指令檔.png](http://192.168.25.60:8000/files/file_storage/1068e509.png)

- 摘要 ➔ 下一步

![ssms 產製 sql script 摘要按下一步.png](http://192.168.25.60:8000/files/file_storage/b200e5f7.png)

- 儲存指令碼 ➔ 完成

![ssms 產製 sql script 儲存指令碼按完成.png](http://192.168.25.60:8000/files/file_storage/3cbb60d7.png)

- 檔案路徑：`C:\Users\11542\Documents`

![ssms 產製 sql script 檔案路徑.png](http://192.168.25.60:8000/files/file_storage/3d5f6851.png)


### 先製作一份 EXCEL 檔盤點所有表格
- EXCEL 檔：[DB.xlsx](http://192.168.25.58/Products/Files/DocEditor.aspx?fileid=112)
- 依照各 DB 填寫一份盤點清單
```sql
-- MSSQL
-- 列出 DB 所有 table
SELECT * FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE='BASE TABLE' ORDER BY TABLE_NAME;
-- 計算 DB 中的 table 數量
SELECT COUNT(1) FROM sys.tables
```

### 將匯出的 Table Schema Script 與 EXCEL 檔案進行比對
- 比對數量是否一致：確認匯出的 table script 清單與 EXCEL 清單一致

- A_SY004 中有兩張 table ( `dtproperties` 、 `sysdiagrams` )為 sql 工具產生的，不須匯出

## 下載並安裝 perl
因 sqlserver2pgsql 需要 Perl 執行，故到 [官網](https://strawberryperl.com/) 下載 `strawberry-perl-5.32.1.1-64bit.msi`

## 用 sqlserver2pgsql 將 MSSQL Table Schema Script 轉成 PostgreSQL 的 Table Schema Script

![sqlserver2pgsql 產製 script 1個原始檔產生3個檔案.png](http://192.168.25.60:8000/files/file_storage/c5ae64ad.png)

### 到要放置 sqlserver2pgsql 的資料夾下，下載安裝 sqlserver2pgsql

```shell
cd Project
git clone https://github.com/dalibo/sqlserver2pgsql.git
```

### 將要轉換的 sql script 放到 sqlserver2pgsql 資料夾底下
- `A_SY004.sql`：MSSQL Table Schema Script

```shell
cd sqlserver2pgsql
```

### 執行 sqlserver2pgsql 進行轉換，會自動生成 3 個檔案(有執行順序)
- 使用 `-f` 指定要轉換的 sql script
- 使用 `-b`、`-a`、`-u` 三個參數產生 3 個檔案，檔名可先自行命名再產出

```shell
perl sqlserver2pgsql.pl -f A_SY004.sql -b A_SY004_before.sql -a A_SY004_after.sql -u A_SY004_unsure.sql
```

- `A_SY004.sql`：要轉換的 sql script
- `A_SY004_before.sql`：建立 PostgreSQL table schema
- `A_SY004_after.sql`：建立 PostgreSQL table 的 primary key
- `A_SY004_unsure.sql`：如果有任何表未被轉換，則執行所需的操作

### 將檔案中的小寫英文轉換成大寫英文，"public" 須維持小寫
- 使用 `Notepad++` 的編輯轉換大小寫功能處理轉換成大寫英文
- 最後將 `"PUBLIC"` 取代為 `"public"`

### 整理 `A_SY004_before.sql`
- 移除一開始不必要的語法

```sql
-- 移除
\set ON_ERROR_STOP
\set ECHO ALL
BEGIN;
```
- 整理好的 CREATE script
```sql
CREATE TABLE "public"."ACPMA"( 
    "COMPANY" CHAR(10),
    "CREATOR" CHAR(10),
    "USR_GROUP" CHAR(10),
    "CREATE_DATE" CHAR(8),
    "MODIFIER" CHAR(10),
    "MODI_DATE" CHAR(8),
    "FLAG" SMALLINT,
    "MA001" VARCHAR(20),
    "MA002" VARCHAR(10),
    "MA003" VARCHAR(20),
    "MA004" VARCHAR(10),
    "MA005" VARCHAR(20),
    "MA006" VARCHAR(10),
    "MA007" VARCHAR(20),
    "MA008" VARCHAR(10),
    "MA009" VARCHAR(20),
    "MA010" VARCHAR(10),
    "MA011" VARCHAR(20),
    "MA012" VARCHAR(20));
```

###  整理 `A_SY004_after.sql`
- 移除一開始不必要的語法

```sql
-- 移除
\SET ON_ERROR_STOP
\SET ECHO ALL
BEGIN;
\SET ECHO ALL
```

- 整理好的 ALTER script

```sql
ALTER TABLE "public"."ACPTA" ADD CONSTRAINT "PK_ACPTA" PRIMARY KEY ("TA001","TA002");
```

# 在 PostgreSQL 建立 table
## 依序執行 script 建立 table
- 先執行 `A_SY004_before.sql` 建立 table
- 後執行 `A_SY004_after.sql` 建立 table 的 primary key

## 測試是否能 insert 資料

```sql
INSERT INTO "public"."ACPMA" values('A_SY004','MIS','MIS','20151215','11359','20160510','1','1102001','','2141','','7349','','7149','','5204','','1256','1268');
```

## 檢查 table 數量

```sql
-- PostgreSQL
-- 列出 DB 所有 table
SELECT * FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE='BASE TABLE' AND TABLE_SCHEMA='public' ORDER BY TABLE_NAME;
-- 計算 DB 中的 table 數量
SELECT (*) FROM INFORMATION_SCHEMA.TABLES WHERE TABLE_TYPE='BASE TABLE' AND TABLE_SCHEMA='public' ORDER BY TABLE_NAME;
```

# 在 PostgreSQL 匯入資料
## 用 SSMS 中的匯出資料功能
- 選取匯出 DB 按右鍵 ➔ 工作 ➔ 匯出資料

![ssms 匯出資料精靈.png](http://192.168.25.60:8000/files/file_storage/494255b7.png)

## 選擇資料來源
- 資料來源：`SQL Server Native Client 11.0`
- 使用 SQL Server 驗證

![ssms 匯出資料精靈 選擇資料來源.png](http://192.168.25.60:8000/files/file_storage/67822480.png)

## 選擇目的地
- 目的地： .NET Framework Data Provider for Odbc
- ConnectingSring：`Driver={PostgreSQL UNICODE};Server=192.168.25.59;Port=5432;Database=signin;UID=11542;PWD=自己的密碼`

![ssms 匯出資料精靈 選擇目的地.png](http://192.168.25.60:8000/files/file_storage/07cfb66f.png)

## 選取來源資料表和檢視
- 選取要匯出的表格 ➔ 編輯對應 ➔ 可調整欄位資料型態
- 若目的地已經有表格可勾選 `卸除並重新建立目的地資料表` 這個選項

![ssms 匯出資料精靈 選取來源資料表和檢視.png](http://192.168.25.60:8000/files/file_storage/2a4348e7.png)

- 可修改成自定義的建表格語法，自動生成的 SQL 不見得正確

![ssms 匯出資料精靈 選取來源資料表和檢視 編輯對應 編輯 SQL.png](http://192.168.25.60:8000/files/file_storage/48c0bc3c.png)

## 成功執行

![ssms 匯出資料精靈 已成功執行.png](http://192.168.25.60:8000/files/file_storage/f58e9023.png)







