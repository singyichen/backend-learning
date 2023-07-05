---
title: Fastify Dependencies ( Test )
description: 測試套件
published: true
date: 2023-05-24T09:13:07.097Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-08-15T03:17:32.668Z
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
- `runInBand`：所有測試會以逐步的方式，在目前程式中執行，這就能夠進行一連串的測試
- `forceExit`：執行完離開
- `colors`：呈顯顏色
- `watchAll`：自動重啟測試
- `coverage`：產生測試報告
- `verbose`：產生測試設置分類（describe）
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
  testEnvironment: 'jsdom',
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

## jest-environment-jsdom
> [reference](https://www.npmjs.com/package/jest-environment-jsdom) 
{.is-info}

> solve question：ReferenceError: You are trying to import a file after the Jest environment has been torn down

https://itsolutionpoints.com/question/referenceerror-you-are-trying-to-import-a-file-after-the-jest-environment-has-been-torn-down/

### Install
```shell
npm install jest-environment-jsdom --save
```

- 於 jest.config.js 中調整 testEnvironment 為 jsdom

```javascript
// jest.config.js
module.exports = {
  testEnvironment: 'jsdom',
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
  setupFilesAfterEnv: ['./test/config.js'],
};
```





