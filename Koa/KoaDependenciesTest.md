---
title: Koa Dependencies ( Test )
description: 測試套件
published: true
date: 2023-05-17T06:24:38.533Z
tags: koa, framework
editor: markdown
dateCreated: 2022-07-26T03:21:30.299Z
---

# 測試套件介紹 ( Test Dependencies )
## jest
> 寫測試的工具，為一個測試框架

> [reference](https://www.npmjs.com/package/jest)
{.is-info}

### Install
```shell
npm i jest --save
```
> [reference](https://jestjs.io/docs/expect)
{.is-info}
### Usage
scripts params meaning
- runInBand：所有測試會以逐步的方式，在目前程式中執行，這就能夠進行一連串的測試
- forceExit：執行完離開
- colors：呈顯顏色
- watchAll：自動重啟測試
- coverage：產生測試報告
- verbose：產生測試設置分類（describe）
```json
// package.json
"scripts": {
    "test": "cross-env NODE_ENV=test jest --runInBand --forceExit --colors --watchAll --coverage --verbose"
},
```
```shell
npm run test
```

![jest test pass and report.png](http://192.168.25.60:8000/files/file_storage/01100dd4.png)

- 設定跑測試時不出現 console.log 的訊息，需設定以下兩個設定檔

```javascript
// test/config.js
/**
 * @description config for console
 */

global.console = {
  log: jest.fn(), // console.log are ignored in tests
  // log: console.log, // console.log are shown in tests

  // Keep native behaviour for other methods, use those to print out things in your own tests, not `console.log`
  error: console.error,
  warn: console.warn,
  info: console.info,
  debug: console.debug,
};
```

```javascript
// jest.config.js
module.exports = {
  testEnvironment: 'node',
  moduleFileExtensions: ['js', 'jsx', 'json', 'ts', 'tsx'],
  collectCoverage: true,
  collectCoverageFrom: [
    '**/*.{ts,js}',
    '!**/node_modules/**',
    '!**/build/**',
    '!**/coverage/**',
  ],
  coverageThreshold: {
    global: {
      branches: 100,
      functions: 100,
      lines: 100,
      statements: 100,
    },
  },
  coverageReporters: ['text', 'text-summary'],
  testRegex: '(/__tests__/.*|(\\.|/)(test|spec))\\.(js|ts)x?$',
  testPathIgnorePatterns: ['/node_modules/', '/build/', '/coverage/'],
  setupFilesAfterEnv: ['./test/config.js'], // 放置 config.js 的路徑
};
```

## supertest
> - Supertest is a way to call on routes, with a simple syntax.
> - Server is our application. (Note: This will not work until we properly export our application, which we will do in a minute)
> - 封裝服務 request，是用來請求接口
> - 如果要對一个服務的 API 接口，進行單元測試，要用 supertest 加載服務的入口文件

> [reference](https://www.npmjs.com/package/supertest) 
{.is-info}
```shell
npm install supertest --save
```

## mocha
> - Mocha 是一個 JavaScript 的測試框架，目的是用來管理測試的程式碼
> - 測試的發動機

> [reference](https://www.npmjs.com/package/mocha) 
{.is-info}

### Install
```shell
npm install mocha --save
```
### Usage
```json
// package.json
"scripts": {
    "mocha": "mocha",
  	"mocha-with-coverage": "nyc --reporter=text mocha"
},
```

## chai
> - Chai 提供 BDD 語法測試用的斷言庫（Assertion Library）。斷言庫是一種判斷工具，驗證執行結果是否符合預期，若實際結果和預測不同，就是測到 bug 了。
> - Chai is a BDD / TDD assertion library for node and the browser that can be delightfully paired with any javascript testing framework.

> [reference](https://www.npmjs.com/package/chai) 
{.is-info}

### Install
```shell
npm install chai --save  
```

## node-mocks-http
> Mock 'http' objects for testing Express and Koa routing functions, but could be used for testing any Node.js web server applications that have code that requires mockups of the request and response objects.

> [reference](https://www.npmjs.com/package/node-mocks-http) 
{.is-info}
### Install
```shell
npm install node-mocks-http --save-dev
```






