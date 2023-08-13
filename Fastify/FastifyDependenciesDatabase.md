---
title: Fastify Dependencies ( Database )
description: 資料庫套件
published: true
date: 2023-08-04T09:11:41.025Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-11-04T08:13:19.416Z
---

# 資料庫套件介紹 ( Database Dependencies )
- [ ] [仿Trello - Prisma 安裝與 Schema 建立](https://ithelp.ithome.com.tw/articles/10250424)
- [ ] [仿Trello - Prisma Migrate 與新增Seeder](https://ithelp.ithome.com.tw/articles/10250814)
- [ ] [仿Trello - Prisma client CRUD](https://ithelp.ithome.com.tw/articles/10251700)
- [ ] [Seeding your database](https://www.prisma.io/docs/guides/migrate/seed-database)

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

#### postgresql
- 會生成一個資料夾 prisma 與 .env(根目錄)

```shell
npx prisma init --datasource-provider postgresql
```

```shell
npx prisma init --datasource-provider sqlite
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

- 設定 .env 如下

```shell
# Environment variables declared in this file are automatically made available to Prisma.
# See the documentation for more detail: https://pris.ly/d/prisma-schema#accessing-environment-variables-from-the-schema

# Prisma supports the native connection string format for PostgreSQL, MySQL, SQLite, SQL Server, MongoDB and CockroachDB (Preview).
# See the documentation for all the connection string options: https://pris.ly/d/connection-strings

# DATABASE
# TEST
# sqlserver
DATABASE_URL=
# postgresql
DATABASE_URL=

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

#### sqlite
- 會生成一個資料夾 prisma 與 .env(根目錄)

```shell
npx prisma init --datasource-provider sqlite
```

- 設定 .env 如下：路徑為放置 sqlite 檔案的路徑

```
# DATABASE
# TEST
# sqlite
DATABASE_URL="file:../sqlite.db"
```

- 將現有 DB 中的 table 轉換成 Prisma schema，將 schema 寫入 schema.prisma 中

```shell
npx prisma db pull
```

- 設定 schema 抓取路徑
```json
// package.json
  "prisma": {
    "schema": "./prisma/schema.prisma"
  }
```

- 生成 Prisma Client：已經有 schema.prisma 時使用
- --schema 參數放 schema.prisma 的路徑

```shell
npx prisma generate --schema=./prisma/schema.prisma
```

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
npx prisma migrate dev --create-only --name Init
```

#### 執行 migration
```shell
npx prisma db push
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

#### 資料庫檢視與編輯工具 [prisma studio](https://www.prisma.io/studio)
- Prisma Studio 已經內建在 prisma 套件中，執行以下指令後，就會啟動一個 Web 服務，如 http://localhost:5555，我們就可以在網頁中查看資料庫內的資料
- 編譯成功後就會自動在瀏覽器上開啟，可以選擇要檢視的 Table

```shell
npx prisma studio
```

![npx prisma studio.png](http://192.168.25.60:8000/files/file_storage/cdffbb5f.png)

#### 對於已上線的專案，需執行 migration 新增資料表及新增初始化資料，開發流程與上線流程
1. **開發階段**
- 先將 docker 中 db 備份到本機端
- 執行 `npm run migration:generate AddWeatherCode`：產製 migration (此步驟會將所有資料 reset)
- 再次將 docker 中 db 備份到本機端
- 執行 `npm run migration:run` ：執行 migration，建立 table weather_code
- 產製初始化資料檔案並執行
  - 產製 seed 檔案 initWeatherCode.js 放置在資料夾 prisma 底下
  - 在 package.json 中的 prisma 新增 "seed": "node prisma/initWeatherCode.js"
```json
{
  "prisma": {
    "schema": "./prisma/schema.prisma",
    "seed": "node prisma/initWeatherCode.js"
  }
}
```
  - 執行 `npx prisma db seed` ：執行 seed 將初始化資料 insert 到 table 中

2. **上線階段**
- db 備份：先將 docker 中 db 備份到本機端 
- docker 部屬完成
- 進入到 container：`docker exec -it c8ca0553f3bb /bin/bash`
- 執行 `npm run migration:run`：執行 migration，建立 table weather_code
- 執行 `npx prisma db seed`：執行 seed 將初始化資料 insert 到 table 中

![Eboard System access container to execte migration and seed.png](http://192.168.25.60:8000/files/file_storage/c3e3e8c1.png)

## @fastify/redis
> - Fastify Redis connection plugin, with which you can share the same Redis connection across every part of your server.
> - @fastify/redis 是 Fastify 官方維護的與 Redis 進行互動的 Plugin

### Install
```shell
npm install @fastify/redis --save
```
> [reference](https://www.npmjs.com/package/@fastify/redis) 
{.is-info}

### Usage
> [reference](https://github.com/fastify/fastify-redis)
{.is-info}

- 新增一個 redis plugin 
```js
/**
 * @description redis plugin
 */

const fastifyPlugin = require('fastify-plugin');
const fastifyRedis = require('@fastify/redis');
const errorLogger = require('./logger');

async function redisConnector(fastify) {
  fastify.register(fastifyRedis, {
    host: '127.0.0.1',
    // 設為 true 表示在 app 關閉時中斷連線
    closeClient: true,
  });
  /**
   * @description 取得單筆 redis 資料路由( GET /redis )
   */
  fastify.get('/redis', async (request, reply) => {
    try {
      const { redis } = fastify;
      const { key } = request.query;
      // redis 取得資料
      await redis.get(key, (err, val) => {
        reply.send(err || JSON.parse(val));
      });
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  });

  /**
   * @description 新增單筆 redis 資料路由( POST /redis )
   */
  fastify.post('/redis', async (request, reply) => {
    try {
      const { redis } = fastify;
      const { key, value } = request.body;
      // redis 新增資料並回傳資料
      await redis.set(key, JSON.stringify(value), (err) => {
        if (err) {
          errorLogger.error(err);
        }
      });
      await redis.get(key, (err, val) => {
        reply.send(err || JSON.parse(val));
      });
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  });
}

module.exports = fastifyPlugin(redisConnector);

```

- 在 app.js 中 register redis plugin

```js
// import plugin
const fastifyRedisPlugin = require('./src/plugin/redis');
// init app
const app = fastify({
  // init logger
  logger: true,
});
// redis
app.register(fastifyRedisPlugin);
```

- Postman 測試：新增資料

![redis postman request post example.png](http://192.168.25.60:8000/files/file_storage/7902c73c.png)

- Postman 測試：取得資料

![redis postman request get example.png](http://192.168.25.60:8000/files/file_storage/b943dd06.png)

- GUI 查看 data

![redis gui key value example.png](http://192.168.25.60:8000/files/file_storage/e88e799f.png)

## Node-Redis
> node-redis is a modern, high performance Redis client for Node.js.
### Install
```shell
npm install redis --save
```
> [reference](https://www.npmjs.com/package/redis) 
{.is-info}

### Usage
> [reference](https://github.com/redis/node-redis)
{.is-info}