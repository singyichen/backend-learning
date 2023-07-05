---
title: Koa Dependencies ( Database )
description: 資料庫套件
published: true
date: 2023-05-17T03:59:40.879Z
tags: koa, framework
editor: markdown
dateCreated: 2022-07-26T03:16:45.183Z
---

# 資料庫套件介紹 ( Database Dependencies )
## prisma
> Prisma is a next-generation ORM.
### Install
```shell
npm install prisma@4.1.0 --save
```
> [reference](https://www.npmjs.com/package/prisma) 
{.is-info}

### Usage
> [reference](https://www.koyeb.com/tutorials/deploy-a-rest-api-using-koa-prisma-and-aiven)
> [official github](https://github.com/prisma/prisma)
{.is-info}

### Init
This will create a new prisma directory that contains a schema.prisma file and a .env and file in the project root. The schema.prisma file contains things like the Prisma client generator, the database connection, and the Prisma schema which will be used to define the database schema.
- 會生成一個資料夾 prisma 與 .env(根目錄)
```shell
npx prisma init --datasource-provider sqlserver
```
```
✔ Your Prisma schema was created at prisma/schema.prisma
  You can now open it in your favorite editor.
  
warn Prisma would have added DATABASE_URL but it already exists in .env
warn You already have a .gitignore file. Don't forget to add `.env` in it to not commit any private information.

Next steps:
1. Set the DATABASE_URL in the .env file to point to your existing database. If your database has no tables yet, read 
https://pris.ly/d/getting-started
2. Run prisma db pull to turn your database schema into a Prisma schema.
3. Run prisma generate to generate the Prisma Client. You can then start querying your database.

More information in our documentation:
https://pris.ly/d/getting-started
```
- 設定.env如下
```shell
# Environment variables declared in this file are automatically made available to Prisma.
# See the documentation for more detail: https://pris.ly/d/prisma-schema#accessing-environment-variables-from-the-schema

# Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB and CockroachDB (Preview).
# See the documentation for all the connection string options: https://pris.ly/d/connection-strings

DATABASE_URL="sqlserver://192.168.25.21:1433;database=ITSoftware_T1;user=sa;password=mis8856;encrypt=DANGER_PLAINTEXT"

# DATABASE_URL="sqlserver://192.168.25.180:1433;database=;user=sa;password=mis8856;"

JWT_PASSPORT_SECRET=THISISAREALLYNICESCRETKRYs

# IP_ADDRESS will be your's own local IP
IP_ADDRESS='192.168.25.180'

# PORT
PORT = '10000'

# AD_URL
AD_URL = 'ldap://192.168.25.62'

# AD_BASEDN
AD_BASEDN = 'OU=SW_GROUP,DC=hokia,DC=com,DC=tw'

# BACKEND_IP
BACKEND_ID = 'http://192.168.25.180'

# FRONTEND_IP
FRONTEND_ID = 'http://192.168.25.100'

# BACKEND_PASSWORD
BACKEND_PASSWORD = 'abc123'
```
- 將現有 DB 中的 table 轉換成 Prisma schema，將 schema 寫入 schema.prisma 中
Introspect the database：Prisma will update your schema with the changes made directly in the database.
```shell
npx prisma db pull
```

![npx prisma db pull.png](http://192.168.25.60:8000/files/file_storage/d6d3b9a5.png)

## @prisma/client
> Prisma Client JS is an auto-generated query builder that enables type-safe database access and reduces boilerplate.

> [reference](https://www.npmjs.com/package/@prisma/client) 
{.is-info}

### Install
```shell
npm install @prisma/client@4.0.0 --save
```
### Usage
#### 設定 schema 抓取路徑
```json
// package.json
  "prisma": {
    "schema": "./src/prisma/schema.prisma"
  }
```
#### 生成 Prisma Client
- 尚未有 schema.prisma 時使用
```shell
npx prisma generate 
```
- 已經有 schema.prisma 時使用
- --schema 參數放 schema.prisma 的路徑
```shell
npx prisma generate --schema=./src/prisma/schema.prisma
```

> 此段 shell 在佈署到 docker 時，需寫入到 dockerfile 中
{.is-info}

- 生成 Prisma Client成功
```shell
Prisma schema loaded from src\prisma\schema.prisma

✔ Generated Prisma Client (3.14.0 | library) to .\node_modules\@prisma\client in 83ms
You can now start using Prisma Client in your code. Reference: https://pris.ly/d/client
import { PrismaClient } from '@prisma/client'
const prisma = new PrismaClient()
```

![npx prisma generate.png](http://192.168.25.60:8000/files/file_storage/35b8b821.png)

#### 生成 migration
```shell
npx prisma migrate dev --create-only --name init
```

#### 執行 migration
```shell
npx prisma migrate dev
```

```
Environment variables loaded from .env
Prisma schema loaded from src\prisma\schema.prisma
Datasource "db" - SQL Server

- The migration `20220817062730_init` failed.
- The migration `20220817062730_init` was modified after it was applied.

√ We need to reset the database.
Do you want to continue? All data will be lost. ... yes

Applying migration `20220817062730_init`

The following migration(s) have been applied:

migrations/
  └─ 20220817062730_init/
    └─ migration.sql

Your database is now in sync with your schema.

✔ Generated Prisma Client (4.1.1 | library) to .\node_modules\@prisma\client in 102ms
```


