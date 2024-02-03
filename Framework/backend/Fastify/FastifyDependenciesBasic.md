---
title: Fastify Dependencies ( Basic )
description: 基本套件
published: true
date: 2023-11-02T09:01:09.219Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-08-09T05:57:34.299Z
---

# 基本套件介紹 ( Basic Dependencies )
- [ ] [[NodeJS]使用 nodemon 來啟動 node server](https://medium.com/@hayato.chang/nodejs-%E4%BD%BF%E7%94%A8-nodemon-%E4%BE%86%E5%95%9F%E5%8B%95-node-server-c6293260278e)
- [ ] [後端基礎：資安細節隨手筆記](https://hugh-program-learning-diary-js.medium.com/%E5%BE%8C%E7%AB%AF%E5%9F%BA%E7%A4%8E-%E8%B3%87%E5%AE%89%E7%B4%B0%E7%AF%80%E9%9A%A8%E6%89%8B%E7%AD%86%E8%A8%98-deb1e6252944)

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
    "dev": "nodemon app.js",
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

## dotenv
> Dotenv is a zero-dependency module that loads environment variables from a .env file into process.env

> [reference](https://www.npmjs.com/package/dotenv) 
{.is-info}

### Install
```shell
npm install dotenv --save-dev
```
### Usage
在 fastify 專案中，已使用套件 `@fastify/env` 取得 `.env` 的資訊，這此使用 `dotenv` 是為了使用 jest 進行測試時能取得 `.env ` 的設定檔

- 在 `jest.config.js` 中進行設定
```js
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
  setupFiles: ['dotenv/config'], // 此設定可讓 .env 在 jest 中使用
};
```

- 參考資料：[Using .env files for unit testing with jest](https://stackoverflow.com/questions/50259025/using-env-files-for-unit-testing-with-jest)


## bcrypt
> 一種專門用於生成密碼雜湊的密碼雜湊函數。它使用鹽和雜湊演算法來產生一個唯一的雜湊值，該雜湊值很難被破解。它非常安全和高效，常用於密碼的加密和驗證。

> [reference](https://www.npmjs.com/package/bcrypt) 
{.is-info}

### Install
```shell
npm install bcrypt --save
```

### Usage
> - [reference](https://github.com/kelektiv/node.bcrypt.js)


## crypto-js
> 一個功能強大的 JavaScript 加密函式庫，提供了多種加密和解密算法。包括 AES、RSA、SHA-256 和 MD5。它支援各種加密演算法，但可能不如 Bcrypt 安全或高效。

> [reference](https://www.npmjs.com/package/crypto-js) 
{.is-info}

### Install
```shell
npm install crypto-js --save
```

### Usage
> - [reference](https://github.com/brix/crypto-js)

| 功能 | CryptoJS | Bcrypt | 
|--|:--:|:--:|
| 類型 | 加密庫 | 密碼雜湊函數 | 
| 支援的加密演算法 | AES、RSA、SHA-256、MD5 | 一種 |
| 用途 | 加密和解密數據、生成密碼雜湊 | 生成密碼雜湊 |
| 安全性 |	可能不如 Bcrypt 安全 | 非常安全 |
| 效率 | 可能不如 Bcrypt 高效 |	高效 |
| 易用性 | 提供了易於使用的 API | 易於使用 |

- 以下使用 Dropbox 的密碼加密流程實作

Dropbox 把密碼先經過 `SHA512` 之後，在經過每個使用者加不同的鹽之後，再用難度為 10 的 `bcrypt` 處理過。 而 `bcrypt` 被設計成一種很慢，很難以通過字定義硬體或 GPUs 的方式去加速運算。最後再使用 `AES256` 加密

![Multiple layers of protection for passwords.png](http://192.168.25.60:8000/files/file_storage/b93b3281.png)

```js
// verificationService.js
  /**
   * @description 使用 SHA512 對明文密碼進行雜湊並加鹽處理
   * @param { string } password 密碼
   * @returns { string } bcryptHash 加鹽過後的密碼
   */
  async encryptWithSHA512(password) {
    try {
      // 生成一個用於每個用戶的鹽
      // 成本因子可以根據你的需求進行調整，鹽的複雜度，數字越高，加密強度越高，但處理時間也越長
      const salt = bcrypt.genSaltSync(10);
      // 使用 SHA512 對明文密碼進行雜湊
      const sha512Hash = CryptoJS.SHA512(password).toString();
      // 將明文密碼的 SHA512 雜湊值和鹽值進行雜湊加密，得到加鹽過後的密碼
      const bcryptHash = await bcrypt.hash(sha512Hash, salt);
      return bcryptHash;
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }

  /**
   * @description 使用 AES256 和一個密鑰 對 bcryptHash 進行加密
   * @param { string } bcryptHash 加鹽過後的密碼
   * @param { string } pepper 密鑰
   * @returns { string } encryptedHash 加密過後的加鹽密碼
   */
  async encryptWithAES256(bcryptHash, pepper) {
    try {
      // 將 pepper 轉換為 key，key 用於加密過程中的金鑰
      const key = CryptoJS.enc.Utf8.parse(pepper);
      // 使用 key 對 bcryptHash 進行 AES256 加密，並指定加密模式為 ECB
      const encryptedHash = CryptoJS.AES.encrypt(bcryptHash, key, {
        mode: CryptoJS.mode.ECB,
      }).toString();
      return encryptedHash;
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }

  /**
   * @description 對密碼進行加密
   * @param { string } password 密碼
   * @returns { string } encryptedPassword 加密後的密碼
   */
  async encryptPassword(password) {
    try {
      const bcryptHash = await this.encryptWithSHA512(password);
      const encryptedPassword = await this.encryptWithAES256(
        bcryptHash,
        process.env.PEPPER,
      );
      return encryptedPassword;
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }

  /**
   * @description 使用密鑰對 bcryptHash 進行解密
   * @param { string } encryptedHash 加密過後的加鹽密碼
   * @param { string } pepper 密鑰
   * @returns { string } decryptedHash 解密過後的加鹽密碼
   */
  decryptWithAES256(encryptedHash, pepper) {
    try {
      // 將 pepper 轉換為 key，key 用於解密過程中的金鑰
      const key = CryptoJS.enc.Utf8.parse(pepper);
      // 使用 key 對 bcryptHash 進行 AES256 解密，並指定解密模式為 ECB
      const decrypted = CryptoJS.AES.decrypt(encryptedHash, key, {
        mode: CryptoJS.mode.ECB,
      });
      const decryptedHash = CryptoJS.enc.Utf8.stringify(decrypted);
      return decryptedHash;
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
  /**
   * @description 驗證密碼
   * @param { string } email 電郵
   * @param { string } password 密碼
   * @returns { Boolean } 密碼是否相符
   */
  async verifyPassword(email, password) {
    try {
      const userData = await prismaClientService.prisma.user.findUnique({
        where: {
          email: email,
        },
      });
      // 解密過後的雜湊值（從 DB 中取得的加密密碼）
      const decryptedHash = this.decryptWithAES256(
        userData.password,
        process.env.PEPPER,
      );
      // 將明文密碼轉換為 SHA512 雜湊值
      const sha512Hash = CryptoJS.SHA512(password).toString();
      // 將使用者輸入的雜湊值與解密後的雜湊值進行比較
      const isMatch = bcrypt.compareSync(sha512Hash, decryptedHash);
      return isMatch;
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
```
