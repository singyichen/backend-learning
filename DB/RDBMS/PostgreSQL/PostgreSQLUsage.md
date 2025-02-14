---
title: PostgreSQL Usage
description: 
published: true
date: 2023-08-09T02:11:01.674Z
tags: postgresql
editor: markdown
dateCreated: 2023-08-09T02:11:01.674Z
---

# PostgreSQL Usage


# Trigger
[PostgreSQL Trigger](https://ithelp.ithome.com.tw/articles/10227402)

- trigger 主要需寫兩個物件
- (1) trigger function：寫入資料的 function，可共用
- (2) trigger：觸發條件設定，可共用

![trigger concept.png](http://192.168.25.60:8000/files/file_storage/91ae65f3.png)

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
        CURRENT_TIMESTAMP
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
        CURRENT_TIMESTAMP
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
        CURRENT_TIMESTAMP
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
        CURRENT_TIMESTAMP
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
        CURRENT_TIMESTAMP
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
- 需求：自動生成一個五位數的 id
## 建立一個 Sequence 自增序列
### 手動建立
- Sequences ➔ 右鍵選取 Create ➔ 選取 sequence

![postgresql create sequence general.png](http://192.168.25.60:8000/files/file_storage/fa28e8b5.png)

- Increment：`1`，每次 + 1
- Start：`0`，從 0 開始遞增
- Minimum：`0`，序列最小值
- Maximum：`99999999`，序列最大值
- Cache：`1`，緩存值

![postgresql create sequence definition.png](http://192.168.25.60:8000/files/file_storage/1acc0a8c.png)

### 使用 sql 語法建立
- 從 0 開始遞增，每次 + 1
```sql 
-- SEQUENCE: public.group_id_seq

-- DROP SEQUENCE IF EXISTS public.group_id_seq;

CREATE SEQUENCE IF NOT EXISTS public.group_id_seq
    INCREMENT 1
    START 0
    MINVALUE 0
    MAXVALUE 99999999
    CACHE 1;

ALTER SEQUENCE public.group_id_seq
    OWNER TO "11542";
    
COMMENT ON SEQUENCE public.group_id_seq
        IS 'table group 的 PK group_id 自增序列';
```
## 設定 table group 的 pk group_id
- 使用 左補 0 函式 lpad 將 group_id 設定為 char(5)
```sql
-- lpad 左補 0 函式
alter table public.group alter column group_id set default lpad(nextval('public.group_id_seq')::TEXT, 5, '0');
```
## prisma 中的 table schema
```
model group {
  group_id             String                 @id @default(dbgenerated("lpad((nextval('group_id_seq'::regclass))::text, 5, '0'::text)")) @db.Char(5)
  group_name           String                 @default("") @db.VarChar(20)
  active               Boolean                @default(false)
  created_date         DateTime               @default(dbgenerated("CURRENT_DATE")) @db.Date
  created_time         DateTime               @default(dbgenerated("CURRENT_TIME")) @db.Time(6)
  created_id           String                 @default("") @db.Char(5)
  modify_date          DateTime?              @db.Date
  modify_time          DateTime?              @db.Time(6)
  modify_id            String?                @default("") @db.Char(5)
  group_lasting        group_lasting[]
  group_lasting_detail group_lasting_detail[]
  sign_log             sign_log[]
  user                 user[]
}
```
