---
title: Fastify Dependencies ( Test )
description: 測試套件
published: true
date: 2023-12-21T01:18:22.421Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-08-15T03:17:32.668Z
---

# 測試套件介紹 ( Test Dependencies )
- [ ] [How to test Socket.io with Jest on backend (Node.js)?](https://medium.com/@tozwierz/testing-socket-io-with-jest-on-backend-node-js-f71f7ec7010f)
- [ ] [一起玩 Cypress(1) - 初探及快速建立](https://hackmd.io/@SuFrank/r1CwP5Dp5)
- [ ] [cypress](https://www.cypress.io/)
- [ ] [cypress chrome recorder](https://chromewebstore.google.com/detail/cypress-chrome-recorder/fellcphjglholofndfmmjmheedhomgin)
- [ ] [記錄、重播及評估使用者流程](https://developer.chrome.com/docs/devtools/recorder?hl=zh-tw)
- [ ] [【Day33】ChatGPT請教教我：E2E測試！Cypress！（中）- Cypress 完整語法](https://ithelp.ithome.com.tw/m/articles/10339976)
- [ ] [playwright](https://playwright.dev/)
- [ ] [【Testing】端到端的測試工具 Playwright](https://www.youtube.com/watch?v=YHw00K6Hg-o&loop=0)
- [ ] [現代化 End to End 測試工具 Playwright｜帶您讀源碼](https://www.youtube.com/watch?v=fqXLfgk6eJY&loop=0)
- [ ] [如何取消 Chrome 自動將 HTTP 轉成 HTTPS](https://rainmakerho.github.io/2021/08/30/Chrome-Auto-Http-to-Https/)

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
  setupFiles: ['dotenv/config'], // 此設定可讓 .env 在 jest 中使用
  testTimeout: 50000,
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
  setupFiles: ['dotenv/config'], // 此設定可讓 .env 在 jest 中使用
  testTimeout: 50000,
};
```

## Web 自動化測試框架比較表
| 特點 | Cypress | Playwright |
|---|:--:|:--:|
| 支援的瀏覽器 | Chromium 核心 (Chrome, Edge) | Chromium, Firefox, WebKit |
| 執行速度 | 一般較慢 | 通常較快，尤其適合複雜測試場景 |
| 偵錯 | 直觀的介面化偵錯程式 | 需要編寫程式碼，相對複雜 |
| 社區和資源 | 社區龐大，文件和學習資源豐富 | 社區相對較小，文件和學習資源相對少 |
| 適用場景 | 快速編寫和運行簡單測試，尤其適合 React 項目 | 跨瀏覽器測試、複雜測試場景、headless 測試 |
| 優點 | 易於使用，啟動快，偵錯方便 | 執行速度快，支援更多瀏覽器，功能更強大 |
| 缺點 | 瀏覽器支援有限 | 偵錯複雜，社區資源少 |
| 支援的程式語言 | JavaScript | JavaScript、Python、Java、C# |

## cypress
> - Cypress 是一個前端自動化測試工具，主要用於測試網頁應用程式
> - Fast, easy and reliable testing for anything that runs in a browser.

### Install
```shell
npx cypress install --force
```
> [reference](https://www.npmjs.com/package/cypress) 
{.is-info}

- 安裝成功

```
Cypress 13.6.1 is installed in C:\Users\11542\AppData\Local\Cypress\Cache\13.6.1

Installing Cypress (version: 13.6.1)

✔  Downloaded Cypress
✔  Unzipped Cypress
✔  Finished Installation C:\Users\11542\AppData\Local\Cypress\Cache\13.6.1

You can now open Cypress by running: node_modules\.bin\cypress open

https://on.cypress.io/installing-cypress
```

### Usage
> [reference](https://github.com/cypress-io/cypress)
{.is-info}

- 在 `package.json` 新增 `script`

```json
{
  "name": "lottery_cypress_test",
  "version": "1.0.0",
  "description": "This project is about cypress test.",
  "main": "index.js",
  "directories": {
    "test": "test"
  },
  "scripts": {
    "open": "cypress open"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "cypress": "^13.6.1"
  }
}
```

- 執行 `npm run open` 會出現一個 UI，接著選擇 `E2E Testing`

```
> lottery_cypress_test@1.0.0 open
> cypress open

It looks like this is your first time using Cypress: 13.6.1

✔  Verified Cypress! C:\Users\11542\AppData\Local\Cypress\Cache\13.6.1\Cypress

Opening Cypress...


DevTools listening on ws://127.0.0.1:52419/devtools/browser/0ca8fff8-3892-4587-b47f-53e09873ce10
```

![cypress UI.png](http://192.168.25.60:8000/files/file_storage/4cc41da5.png)


- 選擇 `E2E Testing` 點選後會得到以下畫面，此畫面主要是 Cypress 相關基本設定，未來還是可以透過實體檔調整，目前就直接選擇 `Continue`

![cypress UI configuration files choose continue.png](http://192.168.25.60:8000/files/file_storage/f57fc6d1.png)

- 選擇使用測試的瀏覽器，選擇完成後就繼續下一步

![cypress UI choose a browser.png](http://192.168.25.60:8000/files/file_storage/a711d265.png)

- 接著 Cypres 會透過瀏覽器開啟另一個視窗

![cypress test browser.png](http://192.168.25.60:8000/files/file_storage/5bf959e9.png)

- 選擇 `Create new spec`，建立一個空白的測試腳本 `cypress\e2e\lottery_login_spec.cy.js`

![cypress test browser create a new spec.png](http://192.168.25.60:8000/files/file_storage/6cba99b3.png)

- 後續會提供 UI 編輯程式，這邊直接忽略後續我們直接在 VsCode 寫，執行 `run the spec`

![cypress test browser create a new spec successfully.png](http://192.168.25.60:8000/files/file_storage/7721c73c.png)

- 執行測試腳本成功

![cypress test browser run a spec successfully.png](http://192.168.25.60:8000/files/file_storage/c40059ba.png)

- 在撰寫程式之前先回到 VsCode 來看一下專案結構

![cypress project structure.png](http://192.168.25.60:8000/files/file_storage/ffd22059.png)

```
│  cypress.config.js                # Cypress 的配置檔案，用於配置插件和其他相關設定。
│  package-lock.json                # 用於記錄安裝過程中確切的相依版本的檔案
│  package.json                     # 描述檔案、套件訊息、開發者訊息
│
│
└─cypress                           
    ├─e2e                           # e2e 測試腳本資料夾
    │      lottery_login_spec.cy.js # 自訂義測試腳本檔案 
    │
    ├─fixtures                      # 用於存放測試過程中使用的假資料檔案
    │      example.js               # 可以包含一些模擬的測試資料
    │
    └─support                       # 用於存放在執行測試之前會運行的共用代碼區域
           commands.js              # 可以包含自定義的 Cypress 命令
           e2e.js                   # 可以包含一些在測試過程中需要使用的輔助函數或共享的配置
```

- 可在 `commands.js` 中寫一些共用的函式，例如：登入

```js
// login 登入
Cypress.Commands.add('login', (user_id, password) => {
  cy.get('#user_id').click();
  cy.get('#user_id').type(user_id);
  cy.get('#password').click();
  cy.get('#password').type(password);
  cy.get('button').click();
});
```

- 調整測試腳本如下，此腳本是搭配使用 cypress chrome recorder 產製

```js
// lottery_login_spec.cy.js
describe('Test 後台尾牙抽獎系統', () => {
  describe('Test 後台超級管理員', () => {
    it('Test 後台超級管理員 使用正確帳號密碼 登入', () => {
      cy.viewport(1035, 931);
      cy.visit('http://eservice.hokia.com.tw:9040/lotmgr/');
      cy.wait(1000);
      cy.login('帳號', '密碼');
      cy.wait(2000);
      cy.get('a:nth-of-type(2) > span').click();
      cy.wait(1000);
      cy.get('a:nth-of-type(3) > span').click();
      cy.wait(1000);
      cy.get('a:nth-of-type(4) > span').click();
      cy.wait(2000);
    });
    it('Test 後台超級管理員 使用正確帳號密碼 登出', () => {
      cy.viewport(1035, 931);
      cy.visit('http://eservice.hokia.com.tw:9040/lotmgr/');
      cy.wait(1000);
      cy.login('帳號', '密碼');
      cy.wait(2000);
      cy.get('a:nth-of-type(2) > span').click();
      cy.wait(1000);
      cy.get('a:nth-of-type(3) > span').click();
      cy.wait(1000);
      cy.get('a:nth-of-type(4) > span').click();
      cy.wait(1000);
      cy.get('div.nav_link > span').click();
      cy.wait(2000);
    });
    it('Test 後台超級管理員 使用錯誤帳號密碼 登入', () => {
      cy.viewport(1035, 931);
      cy.visit('http://eservice.hokia.com.tw:9040/lotmgr/');
      cy.wait(1000);
      cy.login('帳號', '密碼');
      cy.wait(2000);
      cy.get('div.cu-danger.cu-fs-h4.cu-mt-16', {
        timeout: 10000,
      })
        .should('contain', '帳號或密碼錯誤，請重新嘗試。')
        .should('be.visible');
    });
  });
});

```

- 預設會使用 https 測試，若需使用 http 測試可手動調整 url 即可

![cypress test browser run some specs successfully.png](http://192.168.25.60:8000/files/file_storage/714caaab.png)

## playwright
> - Playwright 是一個跨平台、跨語言的開源瀏覽器自動化工具，由微軟開發。
> - Playwright is a framework for Web Testing and Automation.

### Install
```shell
npm i playwright --save
```
> [reference](https://www.npmjs.com/package/playwright) 
{.is-info}

### Usage
> [reference](https://github.com/microsoft/playwright)
{.is-info}


- 實作一個測試用專案

```shell
npm init playwright
```

```
Need to install the following packages:
create-playwright@1.17.131
Ok to proceed? (y) y
Getting started with writing end-to-end tests with Playwright:
Initializing project in '.'
√ Do you want to use TypeScript or JavaScript? · JavaScript
√ Where to put your end-to-end tests? · tests 
√ Add a GitHub Actions workflow? (y/N) · false
√ Install Playwright browsers (can be done manually via 'npx playwright install')? (Y/n) · true
```

- 建置成功

```
✔ Success! Created a Playwright Test project at D:\mandy\Project\lottery_playwright_test

Inside that directory, you can run several commands:

  npx playwright test
    Runs the end-to-end tests.

  npx playwright test --ui
    Starts the interactive UI mode.

  npx playwright test --project=chromium
    Runs the tests only on Desktop Chrome.

  npx playwright test example
    Runs the tests in a specific file.

  npx playwright test --debug
    Runs the tests in debug mode.

  npx playwright codegen
    Auto generate tests with Codegen.

We suggest that you begin by typing:

    npx playwright test

And check out the following files:
  - .\tests\example.spec.js - Example end-to-end test
  - .\tests-examples\demo-todo-app.spec.js - Demo Todo App end-to-end tests
  - .\playwright.config.js - Playwright Test configuration

Visit https://playwright.dev/docs/intro for more information. ✨

Happy hacking! 🎭
```

- 測試指令說明：
- `npx playwright test`：執行所有端到端測試(執行在背景)
- `npx playwright test --headed`：執行所有端到端測試(跳出瀏覽器)
- `npx playwright test --ui`：啟動互動式 UI 模式，可視覺化運行和偵錯測試
- `npx playwright test --project=chromium`：僅在桌面 Chrome 上運行測試
- `npx playwright test example`：運行指定檔案中的測試
- `npx playwright test --debug`：開啟偵錯模式運行測試
- `npx playwright codegen`：使用 Playwright Codegen 自動生成測試程式碼，後面再自行加入要測試的 url
- `npx playwright show-report`：開啟測試報告

- 將以上測試指令寫入到 `package` 的 `script` 中

```json
{
  "name": "lottery_playwright_test",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "npx playwright test --headed",
    "ui":"npx playwright test --ui",
    "test:desktop":"npx playwright test --project=chromium",
    "test:example":"npx playwright test example",
    "test:debug":"npx playwright test --debug",
    "codegen":"npx playwright codegen",
    "report":"npx playwright show-report"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@playwright/test": "^1.40.1",
    "@types/node": "^20.10.5"
  }
}
```

- 專案結構

```
│  playwright.config.js             # playwright 的配置檔案，用於配置插件和其他相關設定。
│  package-lock.json                # 用於記錄安裝過程中確切的相依版本的檔案
│  package.json                     # 描述檔案、套件訊息、開發者訊息
│  .gitignore
│
├─tests
│    example.spec.js
│
└─tests-examples              
     demo-todo-app.spec.js

```

- 執行 `npm run test` 成功

```
> lottery_playwright_test@1.0.0 test
> npx playwright test


Running 6 tests using 2 workers
  6 passed (1.2m)

To open last HTML report run:

  npx playwright show-report
```

- 產製腳本方式：
1. 執行 `npm run codegen http://eservice.hokia.com.tw:9040/lotmgr/`
2. 左側會出現一個瀏覽器，可執行預測試的詳細步驟
3. 測試要點：可搭配使用上方工具列執行
`Pick locator`：**是一個用於選擇元素定位器的功能**。在測試中，需要確定在網頁上選擇哪個元素來執行操作或斷言。Pick locator 可以幫助從網頁上選擇元素，並生成一個定位器（如 CSS 選擇器、XPath 等），以便在測試程式碼中使用。
`Assert Visibility`：**是一個用於確認元素是否可見的功能**。在測試中，您可能需要確保某個元素在頁面上可見，以便進行後續操作或驗證。Assert Visibility 可以檢查元素是否可見，如果元素不可見，則會引發錯誤。
`Assert text`：**是一個用於確認元素文本內容的功能**。在測試中，您可能需要檢查元素的文本是否符合預期，以確保頁面顯示正確的內容。Assert text 可以檢查元素的文本內容是否與預期值匹配，如果不匹配，則會引發錯誤。
`Assert value`：**是一個用於確認元素值的功能**。在測試中，您可能需要檢查表單元素（如輸入框、下拉選單等）的值是否符合預期，以確保使用者輸入或選擇的值正確。Assert value 可以檢查元素的值是否與預期值匹配，如果不匹配，則會引發錯誤。
4. 右側會同步紀錄執行步驟的腳本，執行完後要按下 `Record` 表示停止紀錄
5. 產製完成再複製貼上到測試腳本中
6. 可自行做細部調整：將不需要的步驟刪除、將隱密資料使用 .env 資料取代

![playwright npm run codegen generate script code.png](http://192.168.25.60:8000/files/file_storage/383f8e66.png)

- 常見自動重試斷言
| 斷言 | 描述 | 常用 |
|---|:--:|:--:|	
|`await expect(locator).toBeAttached()`| 元素已附加 | ✕ |
|`await expect(locator).toBeChecked()`| 複選框被選中 | ✓ |
|`await expect(locator).toBeDisabled()`| 元素被停用 | ✓ |
|`await expect(locator).toBeEditable()`| 元素是可編輯的 | ✓ |
|`await expect(locator).toBeEmpty()`| 容器是空的 | ✓ |
|`await expect(locator).toBeEnabled()`| 元素已啟用 | ✓ |
|`await expect(locator).toBeFocused()`| 元素已聚焦 | ✓ |
|`await expect(locator).toBeHidden()`| 元素不可見 | ✓ |
|`await expect(locator).toBeInViewport()`| 元素與視口相交 | ✕ |
|`await expect(locator).toBeVisible()`| 元素可見 | ✓ |
|`await expect(locator).toContainText()`| 元素包含文字 | ✓ |
|`await expect(locator).toHaveAttribute()`| 元素具有 DOM 屬性 | ✕ |
|`await expect(locator).toHaveClass()`| 元素具有類屬性 | ✕ |
|`await expect(locator).toHaveCount()`| 列表具有精確數量的子項 | ✓ |
|`await expect(locator).toHaveCSS()`| 元素具有 CSS 屬性 | ✕ |
|`await expect(locator).toHaveId()`| 元素有一個 ID | ✕ |
|`await expect(locator).toHaveJSProperty()`| 元素具有 JavaScript 屬性 | ✕ |
|`await expect(locator).toHaveScreenshot()`| 元素有截圖 | ✕ |
|`await expect(locator).toHaveText()`| 元素匹配文字 | ✓ |
|`await expect(locator).toHaveValue()`| 輸入有一個值 | ✓ |
|`await expect(locator).toHaveValues()`| 選擇選項已選中 | ✓ |
|`await expect(page).toHaveScreenshot()`| 頁面有截圖 | ✕ |
|`await expect(page).toHaveTitle()`| 頁面有標題 | ✓ |
|`await expect(page).toHaveURL()`| 頁面有一個 URL | ✓ |
|`await expect(response).toBeOK()`| 響應具有 OK 狀態 | ✓ |

- 新增一個測試用腳本 `lotmgr.basic.spec.js`

```js
import { test, expect } from '@playwright/test';
require('dotenv').config();

test.describe('Test 後台尾牙抽獎系統', () => {
  test.describe('Test 後台超級管理員', () => {
    test('Test 後台超級管理員 使用正確帳號密碼 登入', async ({
      page,
    }) => {
      await page.goto('http://eservice.hokia.com.tw:9040/lotmgr/');
      await page.getByText('請登入尾牙抽獎系統後台').click();
      await page.getByPlaceholder('輸入員工編號').click();
      await page
        .getByPlaceholder('輸入員工編號')
        .fill(process.env.LOTMGR_SUPER_ADMIN_USERNAME);
      await page.getByPlaceholder('輸入開機密碼').click();
      await page
        .getByPlaceholder('輸入開機密碼')
        .fill(process.env.LOTMGR_SUPER_ADMIN_CORRECT_PASSWORD);
      await page.getByRole('button', { name: '登入' }).click();
      await page.getByRole('link', { name: '中獎名單' }).click();
      await page.getByRole('link', { name: '獎品管理' }).click();
      await page.getByRole('link', { name: '抽獎人員管理' }).click();
      await page.getByRole('link', { name: '權限管理' }).click();
    });
    test('Test 後台超級管理員 使用正確帳號密碼 登出', async ({
      page,
    }) => {
      await page.goto('http://eservice.hokia.com.tw:9040/lotmgr/');
      await page.getByPlaceholder('輸入員工編號').click();
      await page
        .getByPlaceholder('輸入員工編號')
        .fill(process.env.LOTMGR_SUPER_ADMIN_USERNAME);
      await page.getByPlaceholder('輸入開機密碼').click();
      await page
        .getByPlaceholder('輸入開機密碼')
        .fill(process.env.LOTMGR_SUPER_ADMIN_CORRECT_PASSWORD);
      await page.getByRole('button', { name: '登入' }).click();
      await page.getByRole('link', { name: '中獎名單' }).click();
      await page.getByRole('link', { name: '獎品管理' }).click();
      await page.getByRole('link', { name: '抽獎人員管理' }).click();
      await page.getByRole('link', { name: '權限管理' }).click();
      await page.getByText('後台超級管理員 登出').click();
      await page.getByText('請登入尾牙抽獎系統後台').click();
    });
    test('Test 後台超級管理員 使用錯誤帳號密碼 登入', async ({
      page,
    }) => {
      await page.goto('http://eservice.hokia.com.tw:9040/lotmgr/');
      await page.getByPlaceholder('輸入員工編號').click();
      await page
        .getByPlaceholder('輸入員工編號')
        .fill(process.env.LOTMGR_SUPER_ADMIN_USERNAME);
      await page.getByPlaceholder('輸入開機密碼').click();
      await page
        .getByPlaceholder('輸入開機密碼')
        .fill(process.env.LOTMGR_SUPER_ADMIN_ERROR_PASSWORD);
      await page.getByRole('button', { name: '登入' }).click();
      await page.getByText('帳號或密碼錯誤，請重新嘗試。').click();
    });
  });
});

```

- 執行 `npm run test`：會跳出瀏覽器並顯示測試步驟，最後可在 `http://localhost:9323/` 查看測試報告

![playwright npm run report.png](http://192.168.25.60:8000/files/file_storage/baee89ac.png)

- 執行 `npm run ui`：可自行點選想測試的項目，並查看其測試步驟

![playwright npm run ui.png](http://192.168.25.60:8000/files/file_storage/57e19516.png)

- 可將程式碼進行重構

```js
import { test, expect } from '@playwright/test';
require('dotenv').config();

test.describe('Test 後台尾牙抽獎系統', () => {
  const url = 'http://eservice.hokia.com.tw:9040/lotmgr/';
  /**
   * @description 登入
   */
  async function login(page, userId, password) {
    await page.goto(url);
    await expect(page).toHaveTitle(/後台尾牙抽獎系統/);
    await page.getByText('請登入尾牙抽獎系統後台').click();
    await page.getByPlaceholder('輸入員工編號').click();
    await page.getByPlaceholder('輸入員工編號').fill(userId);
    await page.getByPlaceholder('輸入開機密碼').click();
    await page.getByPlaceholder('輸入開機密碼').fill(password);
    await page.getByRole('button', { name: '登入' }).click();
  }
  /**
   * @description 登出
   */
  async function logout(page) {
    await page.getByText('後台超級管理員 登出').click();
    await page.getByText('請登入尾牙抽獎系統後台').click();
  }
  test.describe('Test 後台超級管理員', () => {
    test('Test 後台超級管理員 使用正確帳號密碼 登入', async ({
      page,
    }) => {
      const userId = process.env.LOTMGR_SUPER_ADMIN_USERNAME;
      const password =
        process.env.LOTMGR_SUPER_ADMIN_CORRECT_PASSWORD;
      await login(page, userId, password);
      await page.getByRole('link', { name: '中獎名單' }).click();
      await page.getByRole('link', { name: '獎品管理' }).click();
      await page.getByRole('link', { name: '抽獎人員管理' }).click();
      await page.getByRole('link', { name: '權限管理' }).click();
    });
    test('Test 後台超級管理員 使用正確帳號密碼 登出', async ({
      page,
    }) => {
      const userId = process.env.LOTMGR_SUPER_ADMIN_USERNAME;
      const password =
        process.env.LOTMGR_SUPER_ADMIN_CORRECT_PASSWORD;
      await login(page, userId, password);
      await page.getByRole('link', { name: '中獎名單' }).click();
      await page.getByRole('link', { name: '獎品管理' }).click();
      await page.getByRole('link', { name: '抽獎人員管理' }).click();
      await page.getByRole('link', { name: '權限管理' }).click();
      await logout(page);
    });
    test('Test 後台超級管理員 使用錯誤帳號密碼 登入', async ({
      page,
    }) => {
      const userId = process.env.LOTMGR_SUPER_ADMIN_USERNAME;
      const password = process.env.LOTMGR_SUPER_ADMIN_ERROR_PASSWORD;
      await login(page, userId, password);
      await page.getByText('帳號或密碼錯誤，請重新嘗試。').click();
    });
  });
});
```




