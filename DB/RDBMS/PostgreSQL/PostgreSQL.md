---
title: PostgreSQL
description: 
published: true
date: 2023-07-13T00:10:22.267Z
tags: postgresql, db
editor: markdown
dateCreated: 2022-10-26T00:34:10.467Z
---

# PostgreSQL
- [ ] [PostgreSQL](https://www.postgresql.org/)
- [ ] [PostgreSQL 正體中文使用手冊](https://docs.postgresql.tw/)
- [ ] [PostgreSQL official documentation](https://www.postgresql.org/docs/current/)
- [ ] [PostgreSQL Tutorial](https://www.postgresqltutorial.com/)
- [ ] [Day 8：資料管理伺服器 (6) - 使用 PostgreSQL 資料庫儲存資料](https://ithelp.ithome.com.tw/articles/10234856)
- [ ] [PostgreSQL 安裝、備份、還原](https://jonny-huang.github.io/projects/05_postgresql_00/)
- [ ] [Prisma PostgreSQL](https://www.prisma.io/docs/concepts/database-connectors/postgresql)
- [ ] [PostgreSQL Trigger](https://ithelp.ithome.com.tw/articles/10227402)
- [ ] [PostgreSQL如何为主键创建自增序列(Sequences)](https://blog.csdn.net/timo1160139211/article/details/78191470?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-78191470-blog-99950883.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-78191470-blog-99950883.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=2)
- [ ] [Postgresql对不足位数的查询结果进行前后补0](https://blog.csdn.net/cao_mr/article/details/106617958)
- [ ] [Why is PostgreSQL voted as the most loved database by Stackoverflow 2022 Developer Survey?](https://blog.bytebytego.com/p/ep30-why-is-postgresql-the-most-loved?utm_source=profile&utm_medium=reader2)

#  為什麼 PostgreSQL 被評為最受歡迎的數據庫

![Why is PostgreSQL voted as the most loved database by developers.png](http://192.168.25.60:8000/files/file_storage/77497cd2.png)

## OLTP (Online Transaction Processing)
We can use PostgreSQL for CRUD (Create-Read-Update-Delete) operations.

## OLAP (Online Analytical Processing)
We can use PostgreSQL for analytical processing. PostgreSQL is based on HTAP (Hybrid transactional/analytical processing) architecture, so it can handle both OLTP and OLAP well.

## FDW (Foreign Data Wrapper)
A FDW is an extension available in PostgreSQL that allows us to access a table or schema in one database from another.

## Streaming
PipelineDB is a PostgreSQL extension for high-performance time-series aggregation, designed to power real-time reporting and analytics applications.

## Geospatial
PostGIS is a spatial database extender for PostgreSQL object-relational database. It adds support for geographic objects, allowing location queries to be run in SQL.

## Time Series
Timescale extends PostgreSQL for time series and analytics. For example, developers can combine relentless streams of financial and tick data with other business data to build new apps and uncover unique insights.

## Distributed Tables
CitusData scales Postgres by distributing data & queries. 

# Install
[PostgreSQL 安裝、備份、還原](https://jonny-huang.github.io/projects/05_postgresql_00/)


# Trigger
[PostgreSQL Trigger](https://ithelp.ithome.com.tw/articles/10227402)

- trigger 主要需寫兩個物件
- (1) trigger function：寫入資料的 function，可共用
- (2) trigger：觸發條件設定，可共用

![postgresql triggers and trigger functions.png](http://192.168.25.60:8000/files/file_storage/16311bc9.png)


## user
- trigger function：update 或 delete 時寫入資料的 function
```
CREATE OR REPLACE FUNCTION user_if_updated_or_deleted()
    RETURNS trigger AS
$$
BEGIN
    INSERT INTO user_log (
        user_id,
        user_name,
        active,
        admin,
				group_id,
				created_date,
				created_id,
				modify_date,
				modify_id,
				log_date
		)
    VALUES (			
				OLD.user_id,
        OLD.user_name,
        OLD.active,
				OLD.admin,
        OLD.group_id,
				OLD.created_date,
				OLD.created_id,
        OLD.modify_date,
        OLD.modify_id,
				now()
		);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

- trigger：update 或 delete 時觸發 trigger

```
CREATE TRIGGER user_update_or_delete
  AFTER UPDATE OR DELETE
  ON public.user
  FOR EACH ROW
  EXECUTE PROCEDURE user_if_updated_or_deleted();
```

## group
- trigger function：update 或 delete 時寫入資料的 function
```
CREATE OR REPLACE FUNCTION group_if_updated_or_deleted()
    RETURNS trigger AS
$$
BEGIN
    INSERT INTO group_log (
        group_id,
        group_name,
        active,
				created_date,
				created_id,
				modify_date,
				modify_id,
				log_date
		)
    VALUES (			
				OLD.group_id,
        OLD.group_name,
        OLD.active,
				OLD.created_date,
				OLD.created_id,
        OLD.modify_date,
        OLD.modify_id,
				now()
		);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

- trigger：update 或 delete 時觸發 trigger

```
CREATE TRIGGER group_update_or_delete
  AFTER UPDATE OR DELETE
  ON public.group
  FOR EACH ROW
  EXECUTE PROCEDURE group_if_updated_or_deleted();
```

## group_lasting
- trigger function：update 或 delete 時寫入資料的 function
```
CREATE OR REPLACE FUNCTION group_lasting_if_updated_or_deleted()
    RETURNS trigger AS
$$
BEGIN
    INSERT INTO group_lasting_log (
        group_id,
        begin_date,
        end_date,
				created_date,
				created_id,
				modify_date,
				modify_id,
				log_date
		)
    VALUES (			
				OLD.group_id,
        OLD.begin_date,
				OLD.end_date,
				OLD.created_date,
				OLD.created_id,
        OLD.modify_date,
        OLD.modify_id,
				now()
		);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

- trigger：update 或 delete 時觸發 trigger

```
CREATE TRIGGER group_lasting_update_or_delete
  AFTER UPDATE OR DELETE
  ON public.group_lasting
  FOR EACH ROW
  EXECUTE PROCEDURE group_lasting_if_updated_or_deleted();
```

## group_lasting_detail
- trigger function：update 或 delete 時寫入資料的 function
```
CREATE OR REPLACE FUNCTION group_lasting_detail_if_updated_or_deleted()
    RETURNS trigger AS
$$
BEGIN
    INSERT INTO group_lasting_detail_log (
        group_id,
        user_id,
        created_date,
        created_id,
        modify_date,
        modify_id,
        log_date
		)
    VALUES (			
				OLD.group_id,
        OLD.user_id,
				OLD.created_date,
				OLD.created_id,
        OLD.modify_date,
        OLD.modify_id,
        now()
		);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

- trigger：update 或 delete 時觸發 trigger

```
CREATE TRIGGER group_lasting_detail_update_or_delete
  AFTER UPDATE OR DELETE
  ON public.group_lasting_detail
  FOR EACH ROW
  EXECUTE PROCEDURE group_lasting_detail_if_updated_or_deleted();
```

## user_lasting
- trigger function：update 或 delete 時寫入資料的 function
```
CREATE OR REPLACE FUNCTION user_lasting_if_updated_or_deleted()
    RETURNS trigger AS
$$
BEGIN
    INSERT INTO user_lasting_log (
        user_id,
        begin_date,
				end_date,
        created_date,
        created_id,
        modify_date,
        modify_id,
        log_date
		)
    VALUES (			
				OLD.user_id,
        OLD.begin_date,
				OLD.end_date,
				OLD.created_date,
				OLD.created_id,
        OLD.modify_date,
        OLD.modify_id,
        now()
		);
    RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

- trigger：update 或 delete 時觸發 trigger

```
CREATE TRIGGER user_lasting_update_or_delete
  AFTER UPDATE OR DELETE
  ON public.user_lasting
  FOR EACH ROW
  EXECUTE PROCEDURE user_lasting_if_updated_or_deleted();
```

# Comment
## user
```sql
COMMENT ON TABLE public."user"
    IS '員工資料表';

COMMENT ON COLUMN public."user".user_id
    IS '員工編號';

COMMENT ON COLUMN public."user".user_name
    IS '員工姓名';

COMMENT ON COLUMN public."user".active
    IS '帳號啟用';

COMMENT ON COLUMN public."user".admin
    IS '管理員';

COMMENT ON COLUMN public."user".group_id
    IS '組別';

COMMENT ON COLUMN public."user".created_date
    IS '建立日期';

COMMENT ON COLUMN public."user".created_id
    IS '建立者';

COMMENT ON COLUMN public."user".modify_date
    IS '修改日期';

COMMENT ON COLUMN public."user".modify_id
    IS '修改者';
```
## group
```sql
COMMENT ON TABLE public."group"
    IS '組別資料表';

COMMENT ON COLUMN public."group".group_id
    IS '組別編號';

COMMENT ON COLUMN public."group".group_name
    IS '組別名稱';

COMMENT ON COLUMN public."group".active
    IS '組別啟用';

COMMENT ON COLUMN public."group".created_date
    IS '建立日期';

COMMENT ON COLUMN public."group".created_id
    IS '建立者';

COMMENT ON COLUMN public."group".modify_date
    IS '修改日期';

COMMENT ON COLUMN public."group".modify_id
    IS '修改者';
```
## group_lasting
```sql
COMMENT ON TABLE public.group_lasting
    IS '組別／居家辦公期間資料表';

COMMENT ON COLUMN public.group_lasting.group_id
    IS '組別編號';

COMMENT ON COLUMN public.group_lasting.begin_date
    IS '起始日期';

COMMENT ON COLUMN public.group_lasting.end_date
    IS '結束日期';

COMMENT ON COLUMN public.group_lasting.created_date
    IS '建立日期';

COMMENT ON COLUMN public.group_lasting.created_id
    IS '建立者';

COMMENT ON COLUMN public.group_lasting.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.group_lasting.modify_id
    IS '修改者';
```
## group_lasting_detail
```sql
COMMENT ON TABLE public.group_lasting_detail
    IS '組別成員資料表';

COMMENT ON COLUMN public.group_lasting_detail.group_id
    IS '組別編號';

COMMENT ON COLUMN public.group_lasting_detail.user_id
    IS '員工編號';

COMMENT ON COLUMN public.group_lasting_detail.created_date
    IS '建立日期';

COMMENT ON COLUMN public.group_lasting_detail.created_id
    IS '建立者';

COMMENT ON COLUMN public.group_lasting_detail.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.group_lasting_detail.modify_id
    IS '修改者';
```
## user_lasting
```sql
COMMENT ON TABLE public.user_lasting
    IS '員工／居家辦公期間資料表';

COMMENT ON COLUMN public.user_lasting.user_id
    IS '員工編號';

COMMENT ON COLUMN public.user_lasting.begin_date
    IS '起始日期';

COMMENT ON COLUMN public.user_lasting.end_date
    IS '結束日期';

COMMENT ON COLUMN public.user_lasting.created_date
    IS '建立日期';

COMMENT ON COLUMN public.user_lasting.created_id
    IS '建立者';

COMMENT ON COLUMN public.user_lasting.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.user_lasting.modify_id
    IS '修改者';
```
## sign_log
```sql
COMMENT ON TABLE public.sign_log
    IS '員工打卡記錄資料表';

COMMENT ON COLUMN public.sign_log.user_id
    IS '員工編號';

COMMENT ON COLUMN public.sign_log.group_id
    IS '組別編號';

COMMENT ON COLUMN public.sign_log.sign_date
    IS '打卡日期';

COMMENT ON COLUMN public.sign_log.status
    IS '狀態';

COMMENT ON COLUMN public.sign_log.ip
    IS 'IP 位址';

COMMENT ON COLUMN public.sign_log.user_agent
    IS 'User Agent';
```
## user_token
```sql
COMMENT ON TABLE public.user_token
    IS '員工存取令牌資料表';

COMMENT ON COLUMN public.user_token.user_id
    IS '員工編號';

COMMENT ON COLUMN public.user_token.token_key
    IS '存取令牌';
```
## login_log
```sql
COMMENT ON TABLE public.login_log
    IS '員工登入記錄表';

COMMENT ON COLUMN public.login_log.user_id
    IS '員工編號';

COMMENT ON COLUMN public.login_log.login_date
    IS '登入日期';

COMMENT ON COLUMN public.login_log.status
    IS '狀態';

COMMENT ON COLUMN public.login_log.ip
    IS 'IP 位址';
```
## user_log
```sql
COMMENT ON TABLE public.user_log
    IS '員工資料異動歷史記錄表';

COMMENT ON COLUMN public.user_log.user_id
    IS '員工編號';

COMMENT ON COLUMN public.user_log.user_name
    IS '員工姓名';

COMMENT ON COLUMN public.user_log.active
    IS '帳號啟用';

COMMENT ON COLUMN public.user_log.admin
    IS '管理員';

COMMENT ON COLUMN public.user_log.group_id
    IS '組別';

COMMENT ON COLUMN public.user_log.created_date
    IS '建立日期';

COMMENT ON COLUMN public.user_log.created_id
    IS '建立者';

COMMENT ON COLUMN public.user_log.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.user_log.modify_id
    IS '修改者';

COMMENT ON COLUMN public.user_log.log_date
    IS '記錄日期';
```
## group_log
```sql
COMMENT ON TABLE public.group_log
    IS '組別／居家辦公期間資料表';

COMMENT ON COLUMN public.group_log.group_id
    IS '組別編號';

COMMENT ON COLUMN public.group_log.group_name
    IS '組別名稱';

COMMENT ON COLUMN public.group_log.active
    IS '組別啟用';

COMMENT ON COLUMN public.group_log.created_date
    IS '建立日期';

COMMENT ON COLUMN public.group_log.created_id
    IS '建立者';

COMMENT ON COLUMN public.group_log.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.group_log.modify_id
    IS '修改者';

COMMENT ON COLUMN public.group_log.log_date
    IS '記錄日期';
```
## group_lasting_log
```sql
COMMENT ON TABLE public.group_lasting_log
    IS '組別／居家辦公期間異動歷史記錄表';

COMMENT ON COLUMN public.group_lasting_log.group_id
    IS '組別編號';

COMMENT ON COLUMN public.group_lasting_log.begin_date
    IS '起始日期';

COMMENT ON COLUMN public.group_lasting_log.end_date
    IS '結束日期';

COMMENT ON COLUMN public.group_lasting_log.created_date
    IS '建立日期';

COMMENT ON COLUMN public.group_lasting_log.created_id
    IS '建立者';

COMMENT ON COLUMN public.group_lasting_log.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.group_lasting_log.modify_id
    IS '修改者';

COMMENT ON COLUMN public.group_lasting_log.log_date
    IS '記錄日期';
```
## group_lasting_detail_log
```sql
COMMENT ON TABLE public.group_lasting_detail_log
    IS '組別成員異動歷史記錄表';

COMMENT ON COLUMN public.group_lasting_detail_log.group_id
    IS '組別編號';

COMMENT ON COLUMN public.group_lasting_detail_log.user_id
    IS '員工編號';

COMMENT ON COLUMN public.group_lasting_detail_log.created_date
    IS '建立日期';

COMMENT ON COLUMN public.group_lasting_detail_log.created_id
    IS '建立者';

COMMENT ON COLUMN public.group_lasting_detail_log.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.group_lasting_detail_log.modify_id
    IS '修改者';

COMMENT ON COLUMN public.group_lasting_detail_log.log_date
    IS '記錄日期';
```
## user_lasting_log
```sql
COMMENT ON TABLE public.user_lasting_log
    IS '員工／居家辦公期間異動歷史記錄表';

COMMENT ON COLUMN public.user_lasting_log.user_id
    IS '員工編號';

COMMENT ON COLUMN public.user_lasting_log.begin_date
    IS '起始日期';

COMMENT ON COLUMN public.user_lasting_log.end_date
    IS '結束日期';

COMMENT ON COLUMN public.user_lasting_log.created_date
    IS '建立日期';

COMMENT ON COLUMN public.user_lasting_log.created_id
    IS '建立者';

COMMENT ON COLUMN public.user_lasting_log.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.user_lasting_log.modify_id
    IS '修改者';
	
COMMENT ON COLUMN public.user_lasting_log.log_date
    IS '記錄日期';
```

# Sequence
## 建立一個 Sequence 自增序列
- 從 1 開始遞增，每次 + 1
```sql 
-- SEQUENCE: public.group_id_seq

-- DROP SEQUENCE IF EXISTS public.group_id_seq;

CREATE SEQUENCE IF NOT EXISTS public.group_id_seq
    INCREMENT 1
    START 1
    MINVALUE 1
    MAXVALUE 99999999
    CACHE 1;

ALTER SEQUENCE public.group_id_seq
    OWNER TO "11542";
```
## 設定 table group 的 pk group_id
- 使用 左補 0 函式 lpad 將 group_id 設定為 char(5)
```sql
-- lpad 左補 0 函式
alter table public.group alter column group_id set default lpad(nextval('public.group_id_seq')::TEXT, 5, '0');
```
