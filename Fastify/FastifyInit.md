---
title: Fastify Init
description: Fastify 專案初始化步驟
published: true
date: 2023-06-13T03:09:37.173Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-09-20T01:12:00.943Z
---

# Fastify Init
## 初始化專案
依照初始化步驟初始化專案

[Fastify](/軟體開發/學習心得/11542/Fastify/Fastify)

## 調整專案名稱為小寫英文
```
feat：alter project name as websignin_fastify_admin_api

原因：調整專案名稱為 websignin_fastify_admin_api ，原本為 WebSignin_Fastify_Admin_API

調整項目：
1. package.json
2. package-lock.json
```

## 將目前專案現有檔案的 code 進行格式化
```
style：code formatting

原因：將 code 進行格式化

調整項目：
1. app.js
2. sensible.js
3. support.js
4. root.js
5. index.js
6. helper.js
7. support.test.js
8. example.test.js
9. root.test.js
```

## 新增 .env 到專案中，並新增 .env 到 .gitignore
```
feat：add .env into .gitignore

原因：因在專案中新增 .env，故新增 .env 到 .gitignore

調整項目：
1. .gitignore：新增忽略 .env 檔案
```

## 新增 prettier 設定檔及忽略檔案
```
feat：add .prettierignore and .prettierrc.json

原因：新增 prettier 設定檔及忽略檔案

新增項目：
1. .prettierignore：prettier 忽略檔案
2. .prettierrc.json：prettier 設定檔
```

## 新增 eslintrc 設定檔
```
feat：add .eslintrc.json

原因：新增 eslintrc 設定檔

新增項目：
1. .eslintrc.json：eslintrc 設定檔
```

## 新增套件 ( Basic、Pattern )
```shell
npm install --save-dev --save-exact prettier
```
```shell
npm install eslint eslint-config-prettier eslint-plugin-prettier nodemon prettier-eslint-cli --save-dev
```

- 在 `package.json` 中新增 format script
```json
// package.json
  "scripts": {
    "format": "prettier-eslint --write \"src/**/*.js\""
  },
```
```
feat：add dependencies and add format script

原因：新增套件 eslint 、 eslint-config-prettier 、 eslint-plugin-prettier 、 nodemon 、 prettier 、 prettier-eslint-cli

調整項目：
1. package.json
    - 新增 format script ：程式格式化 script
    - 新增 eslint ：程式碼靜態檢查工具
    - 新增 eslint-config-prettier ：可以自動判斷並關閉 ESLint 沒有必要的規則
    - 新增 eslint-plugin-prettier ：將 prettier 整合進 eslint，作為 eslint 一條規則來執行 prettier
    - 新增 nodemon ：檢測程式碼是否有任何更改並自動重新啟動服務
    - 新增 prettier ：格式化套件
    - 新增 prettier-eslint-cli ：格式化套件
```

## 調整 start 與 dev script
- 調整 `package.json` 中 start 與 dev script
```json
// package.json
  "scripts": {
    "start": "node app.js",
    "dev": "./node_modules/.bin/nodemon app.js",
  },
```
```
feat：alter start and dev script

原因：調整 start 與 dev script

調整項目：
1. package.json：
    - start script：使用 node 執行 app.js
    - dev script：使用 nodemon 執行 app.js
```

## 移除不需要的資料夾及檔案：plugins、routes、test
```
feat：remove useless folder and files

原因：移除不需要的資料夾及檔案

調整項目：
1. plugins 資料夾
2. routes 資料夾
3. test 資料夾
```

## 新增套件 ( Basic、Database、Others、Log )
```shell
npm i @fastify/env @fastify/cors @prisma/client@4.0.0 prisma@4.1.0 moment pino --save
```
```
feat：add dependencies

原因：新增套件 @fastify/env、@fastify/cors、@prisma/client、prisma、moment、pino

調整項目：
1. package.json
    - 新增 @fastify/env：抓取 .env 檔案
    - 新增 @fastify/cors：配置跨網域設定
    - 新增 @prisma/client@4.0.0：DB ORM prisma client
    - 新增 prisma@4.1.0：DB ORM prisma
    - 新增 moment：時間套件
    - 新增 pino：日誌套件
```

## 新增 prisma 並將現有 DB 的 table 轉成 prisma 的 schema
- 生成一個資料夾 `prisma`
```shell
npx prisma init --datasource-provider postgresql
```
- 將現有 DB 中的 table 轉換成 Prisma schema，將 schema 寫入 `schema.prisma` 中
```shell
npx prisma db pull
```
- 新增一個資料夾 `src` ，將資料夾 `prisma` 放置在 `src` 中
```
feat：add prisma and export the schema from DB

原因：新增 prisma 並將現有 DB 的 table 轉成 prisma 的 schema

新增項目
1. prisima/schema.prisma：將現有 DB 的 table 轉成 prisima 的 schema
```

## 生成 Prisma Client 並設置 prisma schema 檔案路徑
- 在 `package.json` 中設定 schema 抓取路徑
```json
// package.json
  "prisma": {
    "schema": "./src/prisma/schema.prisma"
  }
```
- 生成 Prisma Client：已經有 `schema.prisma` 時使用
```shell
npx prisma generate --schema=./src/prisma/schema.prisma
```
```
feat：add prisma schema path in package.json

原因：設置 prisma schema 檔案路徑

調整項目：
1. package.json
    - 設置 prisma schema 檔案路徑："./src/prisma/schema.prisma"
```

## 新增 prismaClientService.js 到資料夾 ormService 中，放置 Prisma Client
- 在 src 底下新增一個資料夾 `ormService`，新增 `prismaClientService.js`

```
feat：add prismaClientService.js

原因：新增 prismaClientService.js

新增項目：
1. prismaClientService.js
    - 新增 PrismaClient
```

## 新增基礎模型 (controller、response、result)
```
feat：add base module

原因：新增基礎模型 (controller、response、result)

新增項目：
1. base.controller.js：controller 的基礎模型
2. base.response.js：res 的數據模型
3. base.result.js：result 的數據模型
```

## 新增 dockerfile 與 .dockerignore
```
feat：add dockerfile and .dockerignore

原因：新增 dockerfile 與 .dockerignore

新增項目：
1. dockerfile
2. .dockerignore
```

## 新增 env 插件
```
feat：add env plugin to get .env

原因：新增插件 env.js，以抓取 .env

新增項目：
1. env.js：
    - 新增使用套件 @fastify/env
    - 設定讀取 .env，為一個插件 plugin
```

## 新增 logger 插件
```
feat：add logger plugin

原因：新增插件 logger，新增日誌功能

新增項目：
1. logger.js：
    - 設定日誌分類：info、error
    - 設定日誌資料夾路徑
    - 日誌資料夾的存在檢核
    - 日誌檔案依照類型及日期時間分類
    - 設置循環寫入的時間間隔為一天
    - 設定保留最近 30 天的日誌文件
    - 日誌中的日期時間格式化
    - console log 顯示設定，目前無法跟寫入 log 同時運作
```

## 新增 cors 插件
```
feat：add cors plugin

原因：新增插件 cors，新增跨網域設定

新增項目：
1. cors.js：
    - import @fastify/cors
    - corsOptions：設定授權使用 api 的網域
    - register corsOptions
    - export corsConnector
```

## 新增 router 插件
```
feat：add router plugin

原因：新增插件 router，import 與 register 路由

新增項目：
1. router.js：
    - import 各 api 路由
    - register 各 api 路由
    - export routerConnector
```

## 新增 root router
- 新增一個資料夾 `routes`
```javascript
// routes/root.js
'use strict';
/**
 * @description root API 路由
 */

const errorLogger = require('../plugin/logger');
async function router(fastify, opts) {
  fastify.get('/', async function (request, reply) {
    try {
      console.log(request);
      reply.send({ hello: 'world' });
    } catch (error) {
      errorLogger.error(error);
      reply.send({ status: -1, message: error.message });
      console.log(error);
    }
  });
}

module.exports = router;
```
```
feat：add root.js

原因：新增 root router

新增項目：
1. routes/root.js：新增根路由
```
## 重構 app.js
```javascript
'use strict';
/**
 * @description import
 */
// import Dependencies
const fastify = require('fastify');
// import plugin
const fastifyCorsPlugin = require('./src/plugin/cors');
const fastifyRouterPlugin = require('./src/plugin/router');
const fastifyEnvPlugin = require('./src/plugin/env');
const fastifyLoggerPlugin = require('./src/plugin/logger');

function build() {
  // init app
  const app = fastify({
    // 使用 logger plugin
    logger: fastifyLoggerPlugin,
  });
  /**
   * @description plugin
   */
  // plugin
  // 取得 .env 中的環境變數
  app.register(fastifyEnvPlugin).after((err) => {
    if (err) console.log(err);
  });
  app.register(fastifyCorsPlugin);
  // 將 http 類型 log 寫入 info 日誌
  app.addHook('onResponse', (request, reply, done) => {
    fastifyLoggerPlugin.info({
      request: {
        method: request.method,
        url: request.url,
        user_agent: request.headers['user-agent'],
        hostname: request.hostname,
        remoteAddress: request.ip,
        remotePort: request.socket.remotePort,
      },
      reply: {
        statusCode: reply.statusCode,
        statusMessage: reply.raw.statusMessage,
      },
    });
    done();
  });
  // router
  app.register(fastifyRouterPlugin);

  return app;
}
const app = build();
app.listen({ port: process.env.PORT, host: '0.0.0.0' }, function (err) {
  console.log(
    `Welcome to e_board_API app listening at http://${process.env.IP_ADDRESS}:${process.env.PORT}`,
  );
  if (err) {
    // 紀錄全域狀態下的 error 到日誌
    fastifyLoggerPlugin.error(err);
    console.log(err);
  }
});
module.exports = build;
```
```
refactor：refactor app.js

原因：重構 app.js

調整項目：
1. app.js：
    - import fastify、cors、fs、path
    - import fastifyCorsPlugin
    - import fastifyRouterPlugin
    - import fastifyEnvPlugin
    - import fastifyLoggerPlugin
    - init app
    - 使用 logger plugin
    - 將 http 類型 log 寫入 info 日誌
    - 取得 .env 中的環境變數
    - 組態跨網域
    - 初始化路由
    - app.listen
    - 紀錄全域狀態下的 error 到日誌
```

## 新增套件 ( Test )
```
npm i jest --save
```
- 設定跑測試時不出現 console.log 的訊息，需設定以下兩個設定檔
- `test/config.js`：設定 console.log 不出現在測試結果中
- `jest.config.js`：基礎設定與設定放置 config.js 的路徑
```
feat：add jest config.js and jest.config.js

原因：新增 jest 測試 config.js 與 jest.config.js，並讓 console.log 不出現在測試結果中

新增項目：
1. jest.config.js：基礎設定與設定放置 config.js 的路徑
2. config.js：設定 console.log 不出現在測試結果中
```

- 調整 test script

```json
// package.json
  "scripts": {
    "test": "jest --runInBand --forceExit --colors --watchAll --coverage --verbose",
  },
```
```
feat：add dependency jest and alter test script

原因：新增套件 jest 與調整 test script

調整項目：
1. package.json
    - 調整 test script：用 jest 進行測試
    - 新增 jest：測試框架
```

## 新增 root router test
```
feat：add root.test.js

原因：新增 root router unit test

新增項目：
1. root.test.js
```

## 新增套件 ( Basic )
```
npm i axios --save
```
- 使用 axios 呼叫 core_API 中的 dateTime api
- 新增一個資料夾 `api` 放置 `dateTimeApi.js`
```javascript
// src/api/datetimeApi.js
const axios = require('axios');
/**
 * @description dateTime api
 */
const request = axios.create({
  // 設定 api baseURL
  baseURL: 'http://eservice.hokia.com.tw:8110', // 部屬到 docker 的 url
});

/**
 * @description 取得台灣日期時間 api
 * @returns { Object } JSON include tw_date and tw_time and tw_day
 */
const dateTimeApi = async () => {
  return request.get('/api/public/dateTimeFromNtp');
};

module.exports = dateTimeApi;
```
```
feat：add dependency axios

原因：新增套件 axios

調整項目：
1. package.json
    - 新增 axios： http 請求套件
```
```
feat：add dateTimeApi.js and unit test dateTimeApi.test.js

原因：新增 取得台灣日期時間 api 與 單元測試

新增項目：
1. dateTimeApi.js：
    - import axios
    - 設定 api baseURL
    - dateTimeApi：取得台灣日期時間 api，使用 get 方法
    - export dateTimeApi
2. dateTimeApi.test.js：
    - 測試 dateTimeApi
```
## 新增 migration 相關 script
- 新增 migration 相關 script

```json
// package.json
  "scripts": {
    "migration:generate": "npx prisma migrate dev --create-only --name",
    "migration:run": "npx prisma db push",
  },
```

```
feat：add migration:generate and migration:run script package.json

原因：新增 生成 migration 與 執行 migration 的 script

調整項目：
1. package.json：
    - 新增 migration:generate：生成 migration
    - 新增 migration:run：執行 migration
```

- 在 `README.md` 中新增 migration 相關 script

```
### `npm run migration:generate YourMigration`

Generate the migration.\
ex：npm run migration:generate Init\
ex：npm run migration:generate AddGpsToSignin

### `npm run migration:run`

Execute the migration.
```

```
feat：add npm run migration:generate and npm run migration:run script in README.md

原因：新增 生成 migration 與 執行 migration 的 script

調整項目：
1. README.md：
    - 新增 npm run migration:generate：生成 migration
    - 新增 npm run migration:run：執行 migration
```

## 新增套件 ( Authentication )
```
npm i @fastify/auth fast-jwt ldapjs ramda --save
```
```
feat：add dependencies @fastify/auth 、 fast-jwt 、 ldapjs 、 ramda

原因：新增套件 @fastify/auth 、 fast-jwt 、 ldapjs 、 ramda

調整項目：
1. package.json：
    - 新增 @fastify/auth：JWT 驗證使用
    - 新增 fast-jwt：JWT 生成及驗證使用
    - 新增 ldapjs：AD 驗證使用
    - 新增 ramda：FP 函式庫
```

## 新增套件 ( Basic )
```
npm i http-status-codes --save
```
- 新增 `errorInfo.js` 在資料夾 `utils` 中

```
feat：add dependency http-status-codes

原因：新增套件 http-status-codes

調整項目：
1. package.json：
    - 新增套件 http-status-codes：取得 HTTP status code 轉換
```