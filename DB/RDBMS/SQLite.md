---
title: SQLite
description: 
published: true
date: 2023-05-17T01:32:11.484Z
tags: db, sqlite
editor: markdown
dateCreated: 2022-12-20T08:36:11.878Z
---

# SQLite
- [ ] [SQLite 官網](https://sqlitebrowser.org/)
- [ ] [SQLite教學](https://www.1ju.org/sqlite/index)
- [ ] [SQLite Tutorial](https://www.quackit.com/sqlite/tutorial/)
- [ ] [DB Browser for SQLite 視覺化的 SQLite 管理工具，新增、瀏覽資料超方便](https://www.minwt.com/website/server/21758.html)
- [ ] [【SQLite資料庫】#01 DB Browser安裝與操作](https://raymond-investment.com/blog/sqlite-db-browser-install)
- [ ] [SQLite](https://hackmd.io/@wootu/Bk_pYqWIM?type=view)
- [ ] [prisma connect sqlite](https://www.prisma.io/docs/concepts/database-connectors/sqlite)
- [ ] [prisma-schema-reference](https://www.prisma.io/docs/reference/api-reference/prisma-schema-reference#model-field-scalar-types)
- [ ] [SQLite 與 MySQL 的差別](https://medium.com/erens-tech-book/sqlite-%E8%88%87-mysql-%E7%9A%84%E5%B7%AE%E5%88%A5-a14926030ddd)
- [ ] [foreignkeys](https://www.sqlite.org/foreignkeys.html)
- [ ] [sqlite-trigger](https://www.sqlitetutorial.net/sqlite-trigger/)
- [ ] [SQLite觸發器](https://www.1ju.org/sqlite/triggers)
- [ ] [CREATE TRIGGER](https://www.sqlite.org/lang_createtrigger.html)

# 什麼是 SQLite？

![SQLite slogan.png](http://192.168.25.60:8000/files/file_storage/9a0361a3.png)

**SQLite** 是嵌入式關係數據庫管理系統。 它是獨立的，無服務器的，零組態和事務性 SQL 數據庫引擎

**SQLite** 可以自由地用於商業或私有的任何目的。 換句話說，「 **SQLite** 是一種開源，零組態，獨立的，獨立的，旨在嵌入到應用程序中的事務關係數據庫引擎」

**SQLite** 與其他 SQL 數據庫不同，**SQLite** 沒有單獨的服務器進程。 它直接讀取和寫入普通磁盤檔案。 具有多個表，索引，觸發器和視圖的完整 SQL 數據庫包含在單個磁盤檔案中

# 安裝 圖形化使用者管理介面 GUI
## 下載 DB.Browser.for.SQLite-3.12.2-win64.msi 並安裝
在 [官網下載區](https://sqlitebrowser.org/dl/) 下載 **DB Browser for SQLite - Standard installer for 64-bit Windows**

  
# 使用 SQLite
## 新增資料庫
- 新建資料庫 ➔ 設定資料庫檔案儲存位置、檔名 ➔ 存檔
- 資料庫檔案儲存位置：路徑放在專案底下
- 檔名：**sqlite.db**

![SQLite 新增資料庫 位置放置在專案底下.png](http://192.168.25.60:8000/files/file_storage/e30f57ab.png)

## 建立資料表
- Create Table ➔ 輸入資料表名稱 ➔ Add ➔ 輸入欄位名稱 ➔ OK

![SQLite 新增資料表 create table.png](http://192.168.25.60:8000/files/file_storage/38401db4.png)

- Data model mapping

![prisma sqlite data model mapping.png](http://192.168.25.60:8000/files/file_storage/4e9291e7.png)

- Create Table Sql

```sql
CREATE TABLE "user" (
	"user_id"	TEXT NOT NULL,
	"user_name"	TEXT NOT NULL,
	"token"	TEXT,
	"created_date"	DATETIME NOT NULL DEFAULT (datetime(CURRENT_TIMESTAMP, 'localtime')),
	"created_id"	TEXT,
	PRIMARY KEY("user_id")
);
```

```sql
CREATE TABLE "leave" (
	"user_id"	TEXT NOT NULL,
	"deputy_id"	TEXT NOT NULL,
	"leave_date"	TEXT NOT NULL,
	"leave_time"	TEXT NOT NULL,
	"created_date"	DATETIME NOT NULL DEFAULT (datetime(CURRENT_TIMESTAMP, 'localtime')),
	"created_id"	TEXT,
	FOREIGN KEY("user_id") REFERENCES "user"("user_id"),
	FOREIGN KEY("deputy_id") REFERENCES "user"("user_id"),
	CONSTRAINT "user_id_leave_date" PRIMARY KEY("user_id","leave_date")
);
```

```sql
CREATE TABLE "water_rotation" (
	"user_id"	TEXT NOT NULL,
	"order"	INTEGER,
	"created_date"	DATETIME NOT NULL DEFAULT (datetime(CURRENT_TIMESTAMP, 'localtime')),
	"created_id"	TEXT,
	PRIMARY KEY("user_id"),
	FOREIGN KEY("user_id") REFERENCES "user"("user_id")
);
```

## 新增/修改資料
- Browse Data ➔ 選取資料表 ➔ `綠色 + 號` ➔ 輸入資料 ➔ Apply ➔ `ctrl + s` 儲存資料

![prisma sqlite data model mapping.png](http://192.168.25.60:8000/files/file_storage/1d4256ed.png)

- 設定預設值或 FK

![SQLite 設定欄位預設值或 FK.png](http://192.168.25.60:8000/files/file_storage/b1cdd302.png)

## Trigger
- 建立 觸發器（Trigger） 的基本語法如下：

```
CREATE TRIGGER [IF NOT EXISTS] trigger_name 
   [BEFORE|AFTER|INSTEAD OF] [INSERT|UPDATE|DELETE] 
   ON table_name
   [WHEN condition]
BEGIN
 statements;
END;
```

- 當 water_shift 有資料刪除時，同步更新各資料順序 order_number，可能單筆或多筆

```
CREATE TRIGGER water_shift_if_deleted
   AFTER DELETE ON water_shift
BEGIN
   UPDATE water_shift SET queue_number = queue_number -1 WHERE queue_number > old.queue_number;
END
```

- 當 water_shift 有資料更新時，同步更新各資料順序 order_number，只會是單筆

```
CREATE TRIGGER water_shift_if_updated
   AFTER UPDATE ON water_shift
BEGIN
   UPDATE water_shift SET queue_number = old.queue_number WHERE queue_number = new.queue_number AND user_id <> old.user_id;
END
```

# 生成 prisma schema 與 Client
## 生成一個資料夾 prisma 與 .env(根目錄)

```shell
npx prisma init --datasource-provider sqlite
```

## 設定 .env 如下：路徑為放置 sqlite 檔案的路徑
- 路徑：sqlite.db 放置在專案根目錄，故路徑為 file:../sqlite.db
```
# DATABASE
# TEST
# sqlite
DATABASE_URL="file:../sqlite.db"
```

## 將現有 DB 中的 table 轉換成 Prisma schema，將 schema 寫入 schema.prisma 中

```shell
npx prisma db pull
```

## 設定 schema 抓取路徑
```json
// package.json
  "prisma": {
    "schema": "./prisma/schema.prisma"
  }
```

## 生成 Prisma Client：已經有 schema.prisma 時使用
- --schema 參數放 schema.prisma 的路徑

```shell
npx prisma generate --schema=./prisma/schema.prisma
```

## 新增 sqlite.db 到 .gitignore
```
feat：add sqlite.db into .gitignore

原因：使用 db sqlite，故新增 sqlite.db 到 .gitignore

調整項目：
1. .gitignore：新增忽略 sqlite.db
```

# Docker 部屬
## 調整 dockerfile 中 生成 Prisma Client 的 script
```
# 生成 Prisma Client
RUN npx prisma generate --schema=./prisma/schema.prisma
```

## 部屬時需要將資料夾 prisma 與 sqlite.db 一起上傳

![winscp docker 部屬使用 sqlit 相關專案需將資料夾 prisma 與 db 一同上傳.png](http://192.168.25.60:8000/files/file_storage/c7df8723.png)

## 調整 docker-compose
- 新增 volumes script：將 sqlite.db 資料備份到本機
- 本機資料夾路徑：/data/sqlite_data/sqlite.db

```
  # 資訊看板系統前台
  information_board_api:
    build:
      context: ./Information_Board_API
      dockerfile: dockerfile
    image: information_board_api
    container_name: information_board_api
    ports:
      - "10009:10009"
    volumes:
      - "/var/log/logs/information_board_api_logs/logs:/src/logs"
      - "/data/sqlite_data/sqlite.db:/src/sqlite.db"
    depends_on:
      - websignin_fastify_core_api
      - redis
```
- 執行 `docker-compose up -d` 生成資料夾 sqlite_data

```
docker-compose up -d
```

- 手動將 sqlite_data 資料夾讀寫權限設定 777

```
sudo chmod 777 sqlite_data
```

- 先複製一份 sqlite.db 到資料夾中

![winscp docker bind mount 檔案需先將檔案生成出來.png](http://192.168.25.60:8000/files/file_storage/f4ee673b.png)

- 執行 `docker-compose up -d` 成功 build 好 image 後即可看到資料可進行備份

```
docker-compose up -d
```

# SQLite 特性
## SQLite 是完全免費的
SQLite 是開源的。因此，不需要許可證就可以自由地使用它

## SQLite 是無服務器的
SQLite 不需要服務器進程或系統來操作

## SQLite 非常靈活
它可以在同一個會話上同時處理多個數據庫

## SQLite 不需要組態
SQLite 無需設置或管理

## SQLite 是一個跨平臺的數據庫系統
除了在大多數平臺，如 Windows，Mac OS，Linux 和 Unix。 它也可以用於許多嵌入式操作系統，如 Symbian , Android 和 Windows CE 上使用

## 存儲數據很容易
SQLite 提供了一種有效的數據存儲方式

## 列長度可變
列的長度是可變的，不是固定的。 它有助於您只分配一個欄位所需的空間。 例如，如果您有一個 varchar(200) 的列，並且在其上放置了一個 10 個字元的長度值，那麼 SQLite 將僅為該值分配 20 個字元的空間，而不是整個 200 個空間

## 提供大量的 API
SQLite 為大多數的編程語言提供了 API 。 例如： .Net 語言 ( Visual Basic，C＃ )，PHP，Java，Objective C，Python 和許多其他編程語言提供了相應的 API 

## SQLite 是用 ANSI-C 編寫的，提供簡單易用的 API

## SQLite 在 UNIX ( Linux，Mac OS-X，Android，iOS ) 和 Windows ( Win32，WinCE，WinRT )上均可用

# SQLite 優點和缺點
## SQLite 的優點
- SQLite 是一個非常輕量級的數據庫。 因此在電腦，手機，相機，家用電子設備等設備的嵌入式軟件是非常好的選擇

- SQLite 的數據存儲非常簡單高效。 當您需要存儲檔案存檔時，SQLite 可以生成較小數據量的存檔，並且包含常規 ZIP 存檔的大量元數據

- SQLite 可以用作臨時數據集，以對應用程序中的一些數據進行一些處理

- 在 SQLite 數據庫中，數據查詢非常簡單。 您可以將數據加載到 SQLite 內存數據庫中，並隨時提取數據。可以按照您想要的方式提取數據

- SQLite 提供了一種簡單有效的方式來處理數據，而不是以內存變量來做數據處理。 例如：如果您正在開發一個程序，並且有一些記錄要對其進行一些計算。 然後，您可以創建一個 SQLite 數據庫並在其中插入記錄，查詢，可以選擇記錄並直接進行所需的計算

- SQLite 非常容易學習和使用。它不需要任何安裝和組態。只需複製計算機中的 SQLite 庫，就可以創建數據庫了

## SQLite 的缺點
- SQLite 一般用於處理小到中型數據存儲，對於高併發高流量的應用不適用
- 不提供網路訪問
- 不適用大型應用程式
- 缺少提升效能的手段
- 沒有用戶管理

# SQLite 應用場景
## 嵌入式設備及物聯網
## 作為磁碟檔案的替代儲存格式
## 中低流量網站
## 資料分析
你能輕易地使用者種格式處理資料集，且輕易地將其分享給其他人使用

## 資料緩存
許多場景中會將 SQLite 作為資料緩存的手段，避免了網路往返和中央資料庫伺服器的負載

## 伺服器端資料庫
你可以使用 SQLite 作為伺服器端資料庫的底層儲存引擎

## 開發或測試階段的替代及臨時方案
## 適合教育及培訓
輕易地設置性很適合用於 SQL 的教學引擎