---
title: Hapi
description: Hapi
published: true
date: 2022-08-05T00:28:17.662Z
tags: hapi
editor: markdown
dateCreated: 2022-07-29T08:14:00.719Z
---

# Hapi
- [ ] [hapi](https://hapi.dev/)
- [ ] [hapi github](https://github.com/hapijs/hapi)
- [ ] [hapi npm](https://www.npmjs.com/package/@hapi/hapi)
- [ ] [hapi npm](https://www.npmjs.com/package/@hapi/hapi)
- [ ] [hapi Modules](https://hapi.dev/module/?sort=name)
- [ ] [hapi Plugins](https://hapi.dev/plugins/)

# 1. Hapi 框架介紹
# 3. 環境安裝

## 進入在建立專案的資料夾 ( cd to the Project )
```shell
cd WebSignin_Hapi_API
```
## 初始化 npm 建立 package.json ( Init Npm with package.json )
```shell
npm init
```
## 安裝 Hapi ( Install Hapi )
```shell
npm install @hapi/hapi
```
## 新增一個 server.js
```javascript
'use strict';

const Hapi = require('@hapi/hapi');

const init = async () => {
  const server = Hapi.server({
    port: 3000,
    host: 'localhost',
  });

  await server.start();
  console.log('Server running on %s', server.info.uri);
};

process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});

init();
```
## 新增一個 index.js
```javascript
'use strict';

const Hapi = require('@hapi/hapi');

const init = async () => {
  const server = Hapi.server({
    port: 3000,
    host: 'localhost',
  });

  server.route({
    method: 'GET',
    path: '/',
    handler: (request, h) => {
      return 'Hello World!';
    },
  });

  await server.start();
  console.log('Server running on %s', server.info.uri);
};

process.on('unhandledRejection', (err) => {
  console.log(err);
  process.exit(1);
});

init();
```
## 執行專案 ( Run the app )
```shell
node index.js
```
## 連接 http://localhost:3000
頁面會出現如下
Hello World!

## 在[版本控制系統](http://192.168.25.60:9001/)新增一個儲存庫

取得遠端連線網址：http://192.168.25.60:9001/11542/WebSignin_Hapi_API.git

## git 初始化
```shell
git init
```

## 設定遠端連線
```shell
git remote add origin http://192.168.25.60:9001/11542/WebSignin_Hapi_API.git
```

## 程式碼提交 ( commit code )
```shell
git add .
git commit -m "first commit"
```

## 從命令行推送已經建立的儲存庫
```shell
git push -u origin master 
```

## Docker 部屬

### 到專案資料夾
```shell
cd WebSignin_Hapi_API
```

### 打包 Docker 映像檔
```shell
docker build -t websignin_hapi_api --no-cache .
```

### 執行映像檔
```shell
docker run -p 10002:10002 websignin_hapi_api
```
### 測試專案是否成功運作

連接 http://192.168.25.180:10002/