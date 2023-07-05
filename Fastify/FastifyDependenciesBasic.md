---
title: Fastify Dependencies ( Basic )
description: 基本套件
published: true
date: 2023-06-01T07:21:47.736Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-08-09T05:57:34.299Z
---

# 基本套件介紹 ( Basic Dependencies )
## nodemon
> 檢測你的程式碼是否有任何更改並自動重新啟動服務，這時只要重整你的瀏覽器就能看到更動
- 自動重啟應用程式
- 持續偵測你的預設程式
- 預設支援 node ＆ coffeescript，可以輕易執行任何可執行檔案（比如 python，make 等）
- 可以忽略特定檔案或目錄
- 觀察指定的目錄資料夾
- 與服務器應用程式或一次運行公用程式和 REPLs 一起使用
- 可在 node 中被存取使用
- 有完整的開源碼分享在 GitHub 上

> [reference](https://andy6804tw.github.io/2017/12/24/nodemon-tutorial/#%E4%BB%80%E9%BA%BC%E6%98%AF-nodemon) 
{.is-info}
### Install
```shell
npm install nodemon --save-dev
```
> [reference](https://www.npmjs.com/package/nodemon) 
{.is-info}
### Usage
在 scripts 中寫入 **./node_modules/.bin/nodemon**
```json
// package.json
"scripts": {
    "dev": "./node_modules/.bin/nodemon src/app",
},
```
> 開發階段可使用，當正式佈署到 docker 時須移除
{.is-warning}


## @fastify/cors
> @fastify/cors enables the use of CORS in a Fastify application.

> [reference](https://www.npmjs.com/package/@fastify/cors)
{.is-info}
### Install
```
npm install @fastify/cors --save
```
### Usage
```javascript
const cors = require('@fastify/cors');

// 配置跨網域
const corsOptions = {
  // 設定授權使用 api 的網域
  origin: [process.env.BACKEND_IP, process.env.FRONTEND_IP],
  // 設定所允許的 HTTP 請求方法
  methods: ['GET', 'POST', 'PUT', 'PATCH', 'DELETE'],
  // 設定本次請求的有效期，單位為秒
  maxAge: 60,
  // 設定哪些 HTTP 標頭可以於實際請求中使用
  allowedHeaders: ['Content-Type', 'Authorization'],
  hideOptionsRoute: false,
};

app.register(cors, corsOptions);
```

## ramda
> - for function programming
> - A practical functional library for JavaScript programmers
> - Ramda 強調函式都是 pure 的。Immutability 跟不造成 side-effect 是他的設計理念，也就是說我們可以寫出更簡單優雅的程式碼
> - 任何 API 都會自動 curry 化，這特性提供了程式碼相當大的彈性
> - "Function first，Data last"

> [reference](https://www.npmjs.com/package/ramda) 
{.is-info}

### Install
```shell
npm i ramda --save
```
### Usage
- import as R
```javascript
const R = require('ramda');
```
- use functions of R with R.
```javascript
const authenticate = R.pipe(logIn, toUser);
```

## @fastify/env
> Fastify plugin to check environment variables

### Install
> [reference](https://www.npmjs.com/package/@fastify/env) 
{.is-info}

```shell
npm i @fastify/env --save
```
### Usage
> [reference](https://stackoverflow.com/questions/70591401/fastify-is-it-possible-to-access-envs-registered-in-one-file-via-fastify-env-in) 
{.is-info}

- env.js：將 .env 以 schema 方式讀取

```javascript
// src/plugin/env.js
const fastifyPlugin = require('fastify-plugin');
const fastifyEnv = require('@fastify/env');

const schema = {
  type: 'object',
};

const options = {
  schema: schema,
  dotenv: true, // will read .env in root folder
};

async function envConnector(fastify) {
  fastify.register(fastifyEnv, options);
}

module.exports = fastifyPlugin(envConnector);
```

- app.js：於 app.js 中 register

```javascript
// app.js
// import Dependencies
const fastify = require('fastify');
// import plugin
const fastifyEnvPlugin = require('./src/plugin/env');

// init app
const app = fastify({
  // init logger
  logger: true,
});

// plugin
// 取得 .env 中的環境變數
app.register(fastifyEnvPlugin).after((err) => {
  if (err) console.log(err);
});
const port = process.env.PORT || '10001';

```

- 可使用 `process.env.環境變數` 抓取 `.env` 中的環境變數

## http-status-codes
> Constants enumerating the HTTP status codes. Based on the Java Apache HttpStatus API.

### Install
> [reference](https://www.npmjs.com/package/http-status-codes) 
{.is-info}

```shell
npm i http-status-codes --save
```
### Usage
> [reference](https://github.com/prettymuchbryce/http-status-codes) 
{.is-info}

## axios
> Promise based HTTP client for the browser and node.js

### Install
> [reference](https://www.npmjs.com/package/axios) 
{.is-info}

```shell
npm i axios --save
```
### Usage
> - [reference](https://github.com/axios/axios) 
> - [使用Axios你的API都怎麼管理？](https://medium.com/i-am-mike/%E4%BD%BF%E7%94%A8axios%E6%99%82%E4%BD%A0%E7%9A%84api%E9%83%BD%E6%80%8E%E9%BA%BC%E7%AE%A1%E7%90%86-557d88365619)
{.is-info}


## config
> - Node-config organizes hierarchical configurations for your app deployments.
> - Node-config 允許你在你的 Node 應用程式中為不同的部署環境建立組態檔案
> - 敏感資訊放到環境變數 `.env` 裡，不敏感的組態資訊放到 `config.json` 裡

### Install
> [reference](https://www.npmjs.com/package/config) 
{.is-info}

```shell
npm i config --save
```
### Usage
> - [基礎—Node.js 項目的組態檔案](https://shouliang.github.io/2016/06/23/Node.js/%E5%9F%BA%E7%A1%80%E2%80%94Node.js%E9%A1%B9%E7%9B%AE%E7%9A%84%E9%85%8D%E7%BD%AE%E6%96%87%E4%BB%B6/) 
> - [使用 node-config 在 Node.js 中建立組態檔案](https://zhuanlan.zhihu.com/p/426514122)
{.is-info}

- 在專案根目錄建立一個資料夾 `config`，放置 2 個設定檔，於不同環境使用
	- `development.json`：開發(配合 `npm run dev`)與部屬 docker 階段使用，部屬階段時可使用 docker volumn mount 修改設定檔
  - `test.json`：測試階段使用 (配合 jest `npm run test`)

```json
// development.json
{
    "mailer": {
        "email_service":{
            "email_service_ip":"25",
            "email_service_port":"192.168.25.27",
            "email_service_url":"mis@hokia.com.tw"
        },
        "schedule": {
            "schedule": "30 7 * * *"
        },
        "recipient": {
            "mis": [
                "SY-MIS-MG@hokia.com.tw"
            ],
            "hr": [
                "fion.chien@hokia.com.tw",
                "julia.chiao@hokia.com.tw"
            ],
            "backend": [
                "mandy.chen@hokia.com.tw"
            ]
        }
    }
}
```

```json
// test.json
{
    "mailer": {
        "email_service":{
            "email_service_ip":"25",
            "email_service_port":"192.168.25.27",
            "email_service_url":"mis@hokia.com.tw"
        },
        "schedule": {
            "schedule": "30 7 * * *"
        },
        "recipient": {
            "mis": [
                "SY-MIS-MG@hokia.com.tw"
            ],
            "hr": [
                "fion.chien@hokia.com.tw",
                "julia.chiao@hokia.com.tw"
            ],
            "backend": [
                "mandy.chen@hokia.com.tw"
            ]              
        }
    }
}
```

- 可使用 `config.get('mailer.schedule.schedule')` 取得 config 中的組態資訊

```js
const config = require('config');

const schedule = config.get('mailer.schedule.schedule')
```


