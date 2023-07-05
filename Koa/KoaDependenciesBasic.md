---
title: Koa Dependencies ( Basic )
description: 基本套件
published: true
date: 2023-05-17T03:59:19.266Z
tags: koa, framework
editor: markdown
dateCreated: 2022-07-26T03:11:41.348Z
---

# 基本套件介紹 ( Basic Dependencies )
## koa-router
> [reference](https://www.npmjs.com/package/koa-router) 
{.is-info}

### Install
```shell
npm install koa-router --save
```
### Usage
- routes( )：啟用 router
- allowedMethods( )：允許的接口。一個提供數據的接口，就可以設置為 GET， 當客戶端 POST 請求時，就會接返回失敗
```javascript
// app.js
app.use(index.routes(), index.allowedMethods())
```

## koa-bodyparser
> - 解析來自網頁表單 form 所送出的資料
> - A body parser for koa, based on co-body. support json, form and text type body.

> [reference](https://www.npmjs.com/package/koa-bodyparser) 
{.is-info}

### Install
```shell
npm install koa-bodyparser --save
```
### Usage
- 支援可解析資料類型：json、form、text
```javascript
// app.js
app.use(bodyparser({
  enableTypes:['json', 'form', 'text']
}))
```
- 假設有一個 EJS 表單頁面如下
```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Document</title>
</head>
<body>
  <h1>Hello <%- title %></h1>
  <form action="/post" method="post">
    <input type="email" name="email" id="email">
    <input type="text" name="EmployeeID" id="EmployeeID">
    <input type="text" name="Password" id="Password">
    <input type="submit" value="送出">
  </form>
</body>
</html>
```
- 可以利用 ctx.request.body 獲取表單的資料
```javascript
// loginController.js
const { response, request } = ctx
const { EmployeeID, Password } = request.body
}))
```
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
    "dev": "cross-env NODE_ENV=dev ./node_modules/.bin/nodemon src/app",
},
```
> 開發階段可使用，當正式佈署到 docker 時須移除
{.is-warning}

## cross-env
> Run scripts that set and use environment variables across platforms

> [reference](https://www.npmjs.com/package/cross-env) 
{.is-info}

### Install
```shell
npm i cross-env -D --save
```
### Usage
在 scripts 新增 cross-env NODE_ENV=dev、cross-env NODE_ENV=production、cross-env NODE_ENV=test
```json
// package.json
"scripts": {
    "dev": "cross-env NODE_ENV=dev ./node_modules/.bin/nodemon src/app",
    "prd": "cross-env NODE_ENV=production pm2 start src/app",
  	"test": "cross-env NODE_ENV=test jest --runInBand --forceExit --colors --watchAll --coverage  --verbose",
},
```

## dotenv
> Dotenv is a zero-dependency module that loads environment variables from a .env file into process.env

> [reference](https://www.npmjs.com/package/dotenv) 
{.is-info}

### Install
```shell
npm install dotenv --save
```
### Usage
使用 **process.env** 取得 .env 中的環境變數
```javascript
// 取得.env中的環境變數
require('dotenv').config();
// 印出.env中的IP_ADDRESS
console.log('IP_ADDRESS = ' + process.env.IP_ADDRESS);
```

## @koa/cors
> Cross-Origin Resource Sharing(CORS) for koa

> [reference](https://www.npmjs.com/package/@koa/cors)
{.is-info}
### Install
```
npm install @koa/cors --save
```
### Usage
若沒有設定跨網域，前端會無法串接 API
> Referrer Policy: strict-origin-when-cross-origin
{.is-danger}

![後端無設定跨網域前端會無法串接API.png](http://192.168.25.60:8000/files/file_storage/9de90629.png)

- 允許所有網域

```javascript
// app.js
// import
const cors = require('@koa/cors');

// middlewares
// 解決跨網域存取問題
app.use(cors());
```

- 允許部分網域

```javascript
// app.js
// import
const cors = require('@koa/cors');

// middlewares
// 解決跨網域存取問題
const corsOptions = {
  // 設定授權使用 api 的網域
  origin: [
    // 'https://attend-srv.hokia.com.tw', // 公司 server
    // 'http://192.168.25.59:8000', // 正式機/測試機
    // 'http://192.168.25.60:8000', // 正式機/測試機
    // 'http://192.168.25.180:8080', // backend ip
    'http://192.168.25.180:8080', // backend ip + frontend port
    // 'http://192.168.25.140', // ip
    'http://192.168.25.100', // frontend ip
  ],
  // 設定所允許的 HTTP 請求方法
  methods: ['GET', 'POST', 'PUT', 'PATCH', 'DELETE'],
  // 設定本次請求的有效期，單位為秒。
  maxAge: 60,
  // 設定哪些 HTTP 標頭可以於實際請求中使用
  allowedHeaders: ['Content-Type', 'Authorization'],
};
app.use(cors(corsOptions));
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


