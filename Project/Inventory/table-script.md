---
title: Table
description: 
published: true
date: 2023-11-03T09:07:38.379Z
tags: project
editor: markdown
dateCreated: 2023-10-23T02:44:08.739Z
---

# Table
## hardware_attribute
- 資產類別資料表
```sql
-- Table: public.hardware_attribute

-- DROP TABLE IF EXISTS public.hardware_attribute;

CREATE TABLE IF NOT EXISTS public.hardware_attribute
(
    attr_id character(2) COLLATE pg_catalog."default" NOT NULL DEFAULT '00'::bpchar,
    attr_name character varying(50) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::character varying,
    style character(1) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    CONSTRAINT hardware_attribute_pkey PRIMARY KEY (attr_id)
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.hardware_attribute
    OWNER to "11542";

COMMENT ON TABLE public.hardware_attribute
    IS '資產類別資料表';

COMMENT ON COLUMN public.hardware_attribute.attr_id
    IS '資產類別編號';

COMMENT ON COLUMN public.hardware_attribute.attr_name
    IS '資產類別名稱';

COMMENT ON COLUMN public.hardware_attribute.style
    IS '資產類別種類';
```

## hardware_data
- 資產資料表
```sql
-- Table: public.hardware_data

-- DROP TABLE IF EXISTS public.hardware_data;

CREATE TABLE IF NOT EXISTS public.hardware_data
(
    id character(9) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    attribution character(2) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    brand character(10) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    style_no character(20) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    spec character varying(500) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    parts character varying(255) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    department character(3) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    serial_no character varying(50) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    buy_date timestamp with time zone,
    warrant_year double precision DEFAULT 0,
    user_id character varying(5) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    user_name character varying(14) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    trans_user_id character varying(7) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    trans_user_name character varying(14) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    install_soft character varying(255) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    area character varying(20) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    location character varying(20) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    in_use character(1) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    dropped character varying(1) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    memo character varying(100) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    update_time timestamp with time zone,
    static_ip character varying(15) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    check_ip character(10) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    check_shutdown character(10) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    spec_code character varying(150) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    CONSTRAINT hardware_data_pkey PRIMARY KEY (id),
    CONSTRAINT hardware_data_attribution_fkey FOREIGN KEY (attribution)
        REFERENCES public.hardware_attribute (attr_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.hardware_data
    OWNER to "11542";

COMMENT ON TABLE public.hardware_data
    IS '資產資料表';

COMMENT ON COLUMN public.hardware_data.id
    IS '資產編號';

COMMENT ON COLUMN public.hardware_data.attribution
    IS '資產類別編號';

COMMENT ON COLUMN public.hardware_data.brand
    IS '廠牌';

COMMENT ON COLUMN public.hardware_data.style_no
    IS '型號';

COMMENT ON COLUMN public.hardware_data.spec
    IS '資產規格';

COMMENT ON COLUMN public.hardware_data.parts
    IS '零配件';

COMMENT ON COLUMN public.hardware_data.department
    IS '電腦名稱尾碼';

COMMENT ON COLUMN public.hardware_data.serial_no
    IS '序號';

COMMENT ON COLUMN public.hardware_data.buy_date
    IS '購買日期';

COMMENT ON COLUMN public.hardware_data.warrant_year
    IS '保固年限';

COMMENT ON COLUMN public.hardware_data.user_id
    IS '保管人員編';

COMMENT ON COLUMN public.hardware_data.user_name
    IS '保管人姓名';

COMMENT ON COLUMN public.hardware_data.trans_user_id
    IS '移交人員編';

COMMENT ON COLUMN public.hardware_data.trans_user_name
    IS '移交人姓名';

COMMENT ON COLUMN public.hardware_data.install_soft
    IS '軟體清單';

COMMENT ON COLUMN public.hardware_data.area
    IS '存放區域';

COMMENT ON COLUMN public.hardware_data.location
    IS '存放位置';

COMMENT ON COLUMN public.hardware_data.in_use
    IS '資產狀態';

COMMENT ON COLUMN public.hardware_data.memo
    IS '備註';

COMMENT ON COLUMN public.hardware_data.update_time
    IS '更新時間';

COMMENT ON COLUMN public.hardware_data.static_ip
    IS 'IP';
```

## user
- 員工資料表
```sql
-- Table: public.user

-- DROP TABLE IF EXISTS public."user";

CREATE TABLE IF NOT EXISTS public."user"
(
    user_id character(5) COLLATE pg_catalog."default" NOT NULL DEFAULT '10000'::bpchar,
    user_name character varying(100) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::character varying,
    admin boolean NOT NULL DEFAULT false,
    active boolean NOT NULL DEFAULT true,
    CONSTRAINT user_pkey PRIMARY KEY (user_id)
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public."user"
    OWNER to "11542";

COMMENT ON TABLE public."user"
    IS '員工資料表';

COMMENT ON COLUMN public."user".user_id
    IS '員工編號';

COMMENT ON COLUMN public."user".user_name
    IS '員工姓名';

COMMENT ON COLUMN public."user".admin
    IS '管理員啟用';

COMMENT ON COLUMN public."user".active
    IS '帳號啟用';
```

## user_token
- 員工存取令牌資料表
```sql
-- Table: public.user_token

-- DROP TABLE IF EXISTS public.user_token;

CREATE TABLE IF NOT EXISTS public.user_token
(
    user_id character(5) COLLATE pg_catalog."default" NOT NULL DEFAULT '10000'::bpchar,
    token_id character varying(300) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    CONSTRAINT user_token_pkey PRIMARY KEY (user_id),
    CONSTRAINT user_id_token_id_fkey FOREIGN KEY (user_id)
        REFERENCES public."user" (user_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.user_token
    OWNER to "11542";

COMMENT ON TABLE public.user_token
    IS '員工存取令牌資料表';

COMMENT ON COLUMN public.user_token.user_id
    IS '員工編號';

COMMENT ON COLUMN public.user_token.token_id
    IS '存取令牌';
```

## inventory_list
- 盤點單資料表
```sql
-- Table: public.inventory_list

-- DROP TABLE IF EXISTS public.inventory_list;

CREATE TABLE IF NOT EXISTS public.inventory_list
(
    inventory_list_id character(8) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    year character(4) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    quarter character(2) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    other_remark character varying(50) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    first_check_id character(5) COLLATE pg_catalog."default" NOT NULL DEFAULT '10000'::bpchar,
    second_check_id character(5) COLLATE pg_catalog."default" NOT NULL DEFAULT '10000'::bpchar,
    status integer NOT NULL DEFAULT 0,
    created_date timestamp with time zone NOT NULL DEFAULT now(),
    created_id character(5) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    modify_date timestamp with time zone,
    modify_id character(5) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    CONSTRAINT inventory_list_pkey PRIMARY KEY (inventory_list_id),
    CONSTRAINT inventory_list_first_check_id_fkey FOREIGN KEY (first_check_id)
        REFERENCES public."user" (user_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID,
    CONSTRAINT inventory_list_second_check_id_fkey FOREIGN KEY (second_check_id)
        REFERENCES public."user" (user_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
        NOT VALID
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.inventory_list
    OWNER to "11542";

COMMENT ON TABLE public.inventory_list
    IS '盤點單資料表';

COMMENT ON COLUMN public.inventory_list.inventory_list_id
    IS '盤點單編號';

COMMENT ON COLUMN public.inventory_list.year
    IS '年份';

COMMENT ON COLUMN public.inventory_list.quarter
    IS '季度';

COMMENT ON COLUMN public.inventory_list.other_remark
    IS '其他備註';

COMMENT ON COLUMN public.inventory_list.first_check_id
    IS '初盤人員員編';

COMMENT ON COLUMN public.inventory_list.second_check_id
    IS '複盤人員員編';

COMMENT ON COLUMN public.inventory_list.status
    IS '盤點狀態';

COMMENT ON COLUMN public.inventory_list.created_date
    IS '建立日期';

COMMENT ON COLUMN public.inventory_list.created_id
    IS '建立者';

COMMENT ON COLUMN public.inventory_list.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.inventory_list.modify_id
    IS '修改者';
```

## inventory_list_detail
- 盤點清冊資料表
```sql
-- Table: public.inventory_list_detail

-- DROP TABLE IF EXISTS public.inventory_list_detail;

CREATE TABLE IF NOT EXISTS public.inventory_list_detail
(
    inventory_list_id character(8) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    inventory_id character(9) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    attr_id character(2) COLLATE pg_catalog."default" NOT NULL DEFAULT '00'::bpchar,
    brand character(10) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    style_no character(50) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    serial_no character(50) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    area_id character(2) COLLATE pg_catalog."default" NOT NULL DEFAULT '00'::bpchar,
    area_name character(20) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    location_id character(2) COLLATE pg_catalog."default" NOT NULL DEFAULT '00'::bpchar,
    location_name character(20) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    parts character varying(255) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    created_date timestamp with time zone NOT NULL DEFAULT now(),
    created_id character(5) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    modify_date timestamp with time zone,
    modify_id character(5) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    CONSTRAINT inventory_list_detail_pkey PRIMARY KEY (inventory_list_id, inventory_id),
    CONSTRAINT inventory_list_detail_attr_id_fkey FOREIGN KEY (attr_id)
        REFERENCES public.hardware_attribute (attr_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION,
    CONSTRAINT inventory_list_detail_inventory_list_id_fkey FOREIGN KEY (inventory_list_id)
        REFERENCES public.inventory_list (inventory_list_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.inventory_list_detail
    OWNER to "11542";

COMMENT ON TABLE public.inventory_list_detail
    IS '盤點清冊資料表';

COMMENT ON COLUMN public.inventory_list_detail.inventory_list_id
    IS '盤點單編號';

COMMENT ON COLUMN public.inventory_list_detail.inventory_id
    IS '資產編號';

COMMENT ON COLUMN public.inventory_list_detail.attr_id
    IS '資產類別編號';

COMMENT ON COLUMN public.inventory_list_detail.brand
    IS '廠牌';

COMMENT ON COLUMN public.inventory_list_detail.style_no
    IS '型號';

COMMENT ON COLUMN public.inventory_list_detail.serial_no
    IS '序號';

COMMENT ON COLUMN public.inventory_list_detail.area_id
    IS '存放區域編號';

COMMENT ON COLUMN public.inventory_list_detail.area_name
    IS '存放區域名稱';

COMMENT ON COLUMN public.inventory_list_detail.location_id
    IS '存放位置編號';

COMMENT ON COLUMN public.inventory_list_detail.location_name
    IS '存放位置名稱';

COMMENT ON COLUMN public.inventory_list_detail.parts
    IS '零配件';

COMMENT ON COLUMN public.inventory_list_detail.created_date
    IS '建立日期';

COMMENT ON COLUMN public.inventory_list_detail.created_id
    IS '建立者';

COMMENT ON COLUMN public.inventory_list_detail.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.inventory_list_detail.modify_id
    IS '修改者';
```

## inventory_record
- 盤點紀錄資料表
```sql
-- Table: public.inventory_record

-- DROP TABLE IF EXISTS public.inventory_record;

CREATE TABLE IF NOT EXISTS public.inventory_record
(
    inventory_list_id character(8) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    inventory_id character(9) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    check_point integer NOT NULL DEFAULT 0,
    attr_id character(2) COLLATE pg_catalog."default" NOT NULL DEFAULT '00'::bpchar,
    serial_no character varying(50) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    maintain_status boolean NOT NULL DEFAULT false,
    check_remark character varying(100) COLLATE pg_catalog."default" DEFAULT ''::character varying,
    created_date timestamp with time zone NOT NULL DEFAULT now(),
    created_id character(5) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::bpchar,
    modify_date timestamp with time zone,
    modify_id character(5) COLLATE pg_catalog."default" DEFAULT ''::bpchar,
    CONSTRAINT inventory_record_pkey PRIMARY KEY (inventory_list_id, inventory_id, check_point),
    CONSTRAINT inventory_record_attr_id_fkey FOREIGN KEY (attr_id)
        REFERENCES public.hardware_attribute (attr_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.inventory_record
    OWNER to "11542";

COMMENT ON TABLE public.inventory_record
    IS '盤點紀錄資料表';

COMMENT ON COLUMN public.inventory_record.inventory_list_id
    IS '盤點單編號';

COMMENT ON COLUMN public.inventory_record.inventory_id
    IS '資產編號';

COMMENT ON COLUMN public.inventory_record.check_point
    IS '盤點步驟';

COMMENT ON COLUMN public.inventory_record.attr_id
    IS '資產類別編號';

COMMENT ON COLUMN public.inventory_record.serial_no
    IS '序號';

COMMENT ON COLUMN public.inventory_record.maintain_status
    IS '維護狀態';

COMMENT ON COLUMN public.inventory_record.check_remark
    IS '備註';

COMMENT ON COLUMN public.inventory_record.created_date
    IS '建立日期';

COMMENT ON COLUMN public.inventory_record.created_id
    IS '建立者';

COMMENT ON COLUMN public.inventory_record.modify_date
    IS '修改日期';

COMMENT ON COLUMN public.inventory_record.modify_id
    IS '修改者';
```

## area
- 存放區域資料表
```sql
-- Table: public.area

-- DROP TABLE IF EXISTS public.area;

CREATE TABLE IF NOT EXISTS public.area
(
    area_id character(2) COLLATE pg_catalog."default" NOT NULL DEFAULT '00'::bpchar,
    area_name character varying(20) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::character varying,
    CONSTRAINT area_pkey PRIMARY KEY (area_id)
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.area
    OWNER to "11542";

COMMENT ON TABLE public.area
    IS '存放區域資料表';

COMMENT ON COLUMN public.area.area_id
    IS '存放區域編號';

COMMENT ON COLUMN public.area.area_name
    IS '存放區域名稱';
```

## location
- 存放位置資料表
```sql
-- Table: public.location

-- DROP TABLE IF EXISTS public.location;

CREATE TABLE IF NOT EXISTS public.location
(
    area_id character(2) COLLATE pg_catalog."default" NOT NULL DEFAULT '00'::bpchar,
    location_id character(2) COLLATE pg_catalog."default" NOT NULL DEFAULT '00'::bpchar,
    location_name character varying(20) COLLATE pg_catalog."default" NOT NULL DEFAULT ''::character varying,
    CONSTRAINT location_pkey PRIMARY KEY (area_id, location_id),
    CONSTRAINT location_area_id_fkey FOREIGN KEY (area_id)
        REFERENCES public.area (area_id) MATCH SIMPLE
        ON UPDATE NO ACTION
        ON DELETE NO ACTION
)

TABLESPACE pg_default;

ALTER TABLE IF EXISTS public.location
    OWNER to "11542";

COMMENT ON TABLE public.location
    IS '存放位置資料表';

COMMENT ON COLUMN public.location.area_id
    IS '存放區域編號';

COMMENT ON COLUMN public.location.location_id
    IS '存放位置編號';

COMMENT ON COLUMN public.location.location_name
    IS '存放位置名稱';
```
