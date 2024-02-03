---
title: Koa Dependencies ( Log )
description: 日誌套件
published: true
date: 2023-05-18T00:38:14.345Z
tags: koa, framework
editor: markdown
dateCreated: 2022-07-26T03:34:36.925Z
---

# 日誌套件介紹 ( Log Dependencies )
## log4js
> This is a conversion of the log4js framework to work with node.

> [Log4j from attack to prevention](https://blog.bytebytego.com/p/log4j-from-attack-to-prevention?utm_source=profile&utm_medium=reader2)
{.is-warning}


### Install
> [reference](https://www.npmjs.com/package/log4js/v/6.4.7)
{.is-info}
```shell
npm install log4js --save
```
### Usage
> [reference](https://juejin.cn/post/6992080399698493454)
{.is-info}
- 新增 logger 中間件
```javascript
// middleware/logger.js
/**
 * @description logger 的中間件( 紀錄日誌 log )
 */

const log4js = require('log4js');

const config = {
  // appender：用於指定日誌的輸出方式（控制台、檔案等）
  appenders: {
    // layout：用於定義日誌的輸出格式
    // 控制台 console 輸出日誌
    out: {
      type: 'console',
    },
    default: {
      type: 'dateFile',
      filename: 'logs/default/default',
      pattern: '-yyyy-MM-dd.log',
      alwaysIncludePattern: true,
      numBackups: 30,
      maxLogSize: 10485760,
    },
    // 紀錄 http 相關的 log
    http: {
      type: 'dateFile', // 輸出按時間分文件的日誌
      filename: 'logs/http/http', // 生成文件位置路徑及文件名
      pattern: 'yyyy-MM-dd.log', // 生成文件的規則
      alwaysIncludePattern: true, // 設定文件名稱為 filename + pattern
      category: 'http',
      numBackups: 30, // 備份的文件數量，如果文件過多則會將最舊的刪除
      maxLogSize: 10485760, // 設置日誌文件的最大大小，文件體積超過時，自動分文件
    },
    // 紀錄 error 相關的 log
    error: {
      type: 'dateFile',
      filename: 'logs/error/error',
      pattern: 'yyyy-MM-dd.log',
      alwaysIncludePattern: true,
      numBackups: 30,
      maxLogSize: 10485760,
    },
  },
  // category：日誌主項目
  // log4js.getLogger() 直接取得的項目
  categories: {
    default: {
      appenders: ['out', 'default'],
      level: 'info',
    },
    http: { appenders: ['http'], level: 'info' },
    error: {
      appenders: ['error'],
      level: 'error',
    },
  },
};

log4js.configure(config);

exports.httpLogger = (content) => {
  const logger = log4js.getLogger('http');
  logger.level = log4js.levels.INFO;
  logger.info(content);
};
exports.errorLogger = log4js.getLogger('error');

```
- 在 app.js 引入使用
```javascript
// app.js
// import middlewares
const { errorLogger, httpLogger } = require('./middleware/logger');

// middlewares
// 配置日誌/控制台日誌中間件
app.use(async (ctx, next) => {
  const { ip, originalUrl, URL, method, host } = ctx.request;
  const userAgent = ctx.header['user-agent'];
  await next();
  const { status } = ctx.response;
  const { code } = ctx.body;
  const { message } = ctx.body.message === undefined ? ctx.response : ctx.body;
  httpLogger(
    `${ip} - - ${JSON.stringify(method)} ${JSON.stringify(
      originalUrl,
    )} - - ${status} - - ${JSON.stringify(
      URL,
    )} - - ${code} - - ${JSON.stringify(message)} - - ${JSON.stringify(
      host,
    )} - - ${JSON.stringify(userAgent)}`,
  );
});

// error-handling
app.on('error', (err, ctx) => {
  // 紀錄全局狀態下的 error 到日誌
  errorLogger.error(err);
});
```
- 於一般函式中抓取 error 紀錄error 到日誌
```javascript
// import
const { errorLogger } = require('../../../middleware/logger');
try {

} catch (error) {
  errorLogger.error(error);
}
```
- 會建立一個 logs 資料夾放置不同分類的日誌

![koa log4js generate logs.png](http://192.168.25.60:8000/files/file_storage/9f060767.png)



> 因 koa-log4 的版本已無再更新且有資安漏洞，故不使用
{.is-warning}
## koa-log4
> - A wrapper for log4js-node which support Koa logger middleware
> - log4js提供了多個日誌等級分類，同時也能替換 console.log 輸出，另外可以按照文件大小或者日期來生成本地日誌文件，也可以使用郵件等形式發送日誌

> [reference](https://www.npmjs.com/package/koa-log4)
{.is-info}
### Install
```shell
npm install koa-log4 --save
```
### Usage
- 新增 logger 中間件
```javascript
// middleware/logger.js
/**
 * @description logger 的中間件( 紀錄日誌 log )
 */

const log4js = require('koa-log4');

const config = {
  appenders: {
    // 控制台 console 輸出日誌
    out: {
      type: 'console',
    },
    default: {
      type: 'dateFile',
      filename: 'logs/default/default',
      pattern: '-yyyy-MM-dd.log',
      alwaysIncludePattern: true,
      numBackups: 30,
      maxLogSize: 10485760,
    },
    // 紀錄 http 相關的 log
    http: {
      type: 'dateFile', // 輸出按時間分文件的日誌
      filename: 'logs/http/http', // 生成文件位置路徑及文件名
      pattern: '-yyyy-MM-dd.log', // 生成文件的規則
      alwaysIncludePattern: true,
      numBackups: 30, // 備份的文件數量，如果文件過多則會將最舊的刪除
      maxLogSize: 10485760, // 設置日誌文件的最大大小，文件體積超過時，自動分文件
    },
    // 紀錄 error 相關的 log
    error: {
      type: 'dateFile',
      filename: 'logs/error/error',
      pattern: '-yyyy-MM-dd.log',
      alwaysIncludePattern: true,
      numBackups: 30,
      maxLogSize: 10485760,
    },
  },
  categories: {
    default: {
      appenders: ['out', 'default'],
      level: 'info',
    },
    http: { appenders: ['http'], level: 'info' },
    error: {
      appenders: ['error'],
      level: 'error',
    },
  },
};

log4js.configure(config);

exports.httpLogger = () => log4js.koaLogger(log4js.getLogger('http'));
exports.errorLogger = log4js.getLogger('error');

```
- 在 app.js 引入使用
```javascript
// app.js
// import middlewares
const { errorLogger, httpLogger } = require('./middleware/logger');

// middlewares
// 配置日誌/控制台日誌中間件
app.use(httpLogger());

// error-handling
app.on('error', (err, ctx) => {
  // 紀錄全局狀態下的 error 到日誌
  errorLogger.error(err);
});
```
- 於一般函式中抓取 error 紀錄error 到日誌
```javascript
// import
const { errorLogger } = require('../../../middleware/logger');
try {

} catch (error) {
  errorLogger.error(error);
}
```
- 會建立一個 logs 資料夾放置不同分類的日誌

![koa log4 generate logs.png](http://192.168.25.60:8000/files/file_storage/049a210c.png)






