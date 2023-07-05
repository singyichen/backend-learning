---
title: OpenAI Line Bot
description: 
published: true
date: 2022-12-15T09:12:41.393Z
tags: line bot
editor: markdown
dateCreated: 2022-12-12T01:47:11.556Z
---

# OpenAI Line Bot
> 使用 node.js ，framework fastify 實作一個 OpenAI LINE Bot 聊天機器人

- [ ] [用 Node.js 建立你的第一個 OpenAI LINE Bot 聊天機器人](https://israynotarray.com/nodejs/20221210/1224824056/?fbclid=IwAR3d8xR3Xj_31rOGVI5xEsLEBphqi0Jn8bIRz9aSN3w78zglvuOAYF7X8WA)
- [ ] [GPT AI Assistant](https://github.com/memochou1993/gpt-ai-assistant)
- [ ] [LINE Messaging API SDK for nodejs](https://github.com/line/line-bot-sdk-nodejs)
- [ ] [使用Node.js建置你的第一個LINE BOT](https://pyradise.com/%E4%BD%BF%E7%94%A8node-js%E5%BB%BA%E7%BD%AE%E4%BD%A0%E7%9A%84%E7%AC%AC%E4%B8%80%E5%80%8Bline-bot-590b7ba7a28a)
- [ ] [LINE Bot聊天機器人程式開發教學（三）：建立Node.js版的Echo Bot](https://swf.com.tw/?p=1098)
- [ ] [LINE bot 好好玩 30 天玩轉 LINE API 系列](https://ithelp.ithome.com.tw/users/20117701/ironman/2634)
- [ ] [openai models](https://beta.openai.com/docs/models/overview)
- [ ] [[Deploy to Render] 用免費方案部署 LINE Bot](https://ithelp.ithome.com.tw/articles/10283836)
- [ ] [[Deploy to Render] 什麼是 Render](https://ithelp.ithome.com.tw/articles/10255630?sc=rss.qu)
- [ ] [如何使用 Render 免費部署你的 Node.js 應用程式](https://www.freecodecamp.org/chinese/news/how-to-deploy-nodejs-application-with-render/)
- [ ] [關於從 Heroku 跳到 Render 這件事情](https://israynotarray.com/other/20221213/3036227586/)

# 申請 OpenAI API
## 登入 [OpenAI](https://beta.openai.com/) 平台，或註冊一個新的帳號
## 生成一個 OpenAI 的 API key
- 找到你的頭像點它，找到「View API keys」
- 看到「API keys」的申請畫面，點一下下方的「Create New Secret key」，這樣你就拿到串接 OpenAI 的 key，請先把它記下來，後面會用到。主要是稍後後面會串接 OpenAI 所提供的 API 來使用 ChatGPT。

> API key generated 是非常重要的，因此請不要隨意提供給他人，以免造成不必要的損失。
{.is-warning}

# 申請 Line Developer
## 登入 [LINE](https://developers.line.biz/en/) 平台，或註冊一個新的帳號
- 新增一個提供者（Provider），例如「Hokia」。
- 在「Hokia」新增一個類型為「Messaging API」的頻道（Channel），例如「AI Assistant」。
- 在「AI Assistant」點選「Messaging API」頁籤，生成一個頻道的 channel access token。
## 取得 Channel ID、Channel Secret、Channel Access Token
- 在 Channel Settings 畫面裡總共有三個重要的資料要找到：
	1. `Channel ID`：頻道識別碼，在 **Basic settings** 頁籤
	2. `Channel Secret`：頻道密鑰，在 **Basic settings** 頁籤
	3. `Channel Access Token`：頻道存取代碼，在 **Messaging API** 頁籤
## 加入 Line Bot 好友
- 使用 QR code 加入 Line Bot 好友：在 **Messaging API** 頁籤
## 調整 Line Bot 關掉預設回饋、加入好友通知
- Auto-reply messages：在 **Messaging API** 頁籤，調整成 Disabled
- Greeting messages：在 **Messaging API** 頁籤，調整成 Disabled

# 建立一個 fastify 專案
## 依照 [Fastify 專案初始化步驟](/軟體開發/學習心得/11542/Fastify/FastifyInit) 建立
## 在 .env 檔案中新增三個環境變數
```
# 頻道識別碼
CHANNEL_ID='Channel ID'
# 頻道密鑰
CHANNEL_SECRET='Channel Secret'
# 頻道存取代碼
CHANNEL_ACCESS_TOKEN='Channel Access Token'
```
## 此專案 port 預設為 10007

# 撰寫 Line Bot
## 安裝相關套件 [@line/bot-sdk](https://github.com/line/line-bot-sdk-nodejs/tree/master)
```
npm i @line/bot-sdk --save
```
```
feat：add dependency @line/bot-sdk

原因：新增套件 @line/bot-sdk

調整項目：
1. package.json
    - 新增 @line/bot-sdk：LINE Messaging API SDK for nodejs
```
## 新增一個 line bot plugin，並於 app.js 中 register
- post url：http://127.0.0.1:10007/callback
```js
/**
 * @description line bot plugin
 */
const fastifyPlugin = require('fastify-plugin');
const errorLogger = require('./logger');
// import linebot SDK
const line = require('@line/bot-sdk');

async function lineBotConnector(fastify, opts, done) {
  // create LINE SDK config from env variables
  const config = {
    channelId: process.env.CHANNEL_ID,
    channelSecret: process.env.CHANNEL_SECRET,
    channelAccessToken: process.env.CHANNEL_ACCESS_TOKEN,
  };

  // create LINE SDK client
  const client = new line.Client(config);
  // listen webhook url 
  fastify.post('/callback', async function (request, reply) {
    try {
      Promise.all(request.body.events.map(handleEvent))
        .then((result) => console.error(result))
        .catch((err) => {
          console.error(err);
          reply.status(500).end();
        });
      done();
    } catch (error) {
      errorLogger.error(error);
      reply.send({ status: -1, message: error.message });
      console.log(error);
    }
  });

  // event handler
  function handleEvent(event) {
    if (event.type !== 'message' || event.message.type !== 'text') {
      // ignore non-text-message event
      return Promise.resolve(null);
    }
    // create a echoing text message
    const echo = { type: 'text', text: event.message.text };

    // use reply API
    return client.replyMessage(event.replyToken, echo);
  }
  done();
}

module.exports = fastifyPlugin(lineBotConnector);

```
# 使用 ngrok export Line Bot
## 開啟 ngrok.exe
## 將本機此專案的 port 對外公開
```
ngrok http 10007
```
## 取得隨機產生的一個 HTTPS 的網址
https://cc93-122-147-204-116.jp.ngrok.io

# 設定 Line Webhook URL
## 在 Webhook settings 中設定 Webhook URL
- 將 https://cc93-122-147-204-116.jp.ngrok.io/callback 填入 url
- 按下 **Verify** 來驗證網址是否正確以及專案是否正常
- 呈現 **Success**：表示網址以及專案是正確的

![line bot webhook url verify success.png](http://192.168.25.60:8000/files/file_storage/ab81fdf3.png)

## 開啟 Use webhook 功能
![line bot webhook use webhook with ngrok.png](http://192.168.25.60:8000/files/file_storage/289be571.png)

# 測試 Line Bot echo 功能 
- Line Bot 會自動回覆使用者所傳送的訊息

![line bot echo feature.png](http://192.168.25.60:8000/files/file_storage/7c8a421f.png)

# 串接 OpenAI API
## 安裝相關套件 [openai](https://www.npmjs.com/package/openai)
```
npm i openai --save
```
```
feat：add dependency openai

原因：新增套件 openai

調整項目：
1. package.json
    - 新增 openai：OpenAI API for nodejs
```
## 在 .env 檔案中新增一個環境變數
```
OPENAI_API_KEY='OpenAI API key'
```
## 引入 OpenAI 的套件
```js
// import openai
const { Configuration, OpenAIApi } = require('openai');
```
## 實例化 OpenAI
```js
// initializing the library with the API key loaded from an environment variable
const configuration = new Configuration({
  apiKey: process.env.OPENAI_API_KEY,
});
const openai = new OpenAIApi(configuration);
```

## 建立一個 completion 並取得其 res
```js
// creating a completion
const completion = await openai.createCompletion({
  // Most capable GPT-3 model
  model: 'text-davinci-003',
  prompt: event.message.text,
  // The maximum number of tokens to generate in the completion
  max_tokens: 4000,
});
// completion res
console.log(completion.data.choices[0].text.trim())
```
## 完整版 line bot plugin
```js
/**
 * @description line bot plugin
 */
const fastifyPlugin = require('fastify-plugin');
const errorLogger = require('./logger');
// import linebot SDK
const line = require('@line/bot-sdk');
// import openai
const { Configuration, OpenAIApi } = require('openai');

async function lineBotConnector(fastify, opts, done) {
  // create LINE SDK config from env variables
  const config = {
    channelId: process.env.CHANNEL_ID,
    channelSecret: process.env.CHANNEL_SECRET,
    channelAccessToken: process.env.CHANNEL_ACCESS_TOKEN,
  };

  // create LINE SDK client
  const client = new line.Client(config);
  // listen webhook url
  fastify.post('/callback', async function (request, reply) {
    try {
      Promise.all(request.body.events.map(handleEvent))
        .then((result) => console.error(result))
        .catch((err) => {
          console.error(err);
          reply.status(500).end();
        });
      done();
    } catch (error) {
      errorLogger.error(error);
      reply.send({ status: -1, message: error.message });
      console.log(error);
    }
  });
  // initializing the library with the API key loaded from an environment variable
  const configuration = new Configuration({
    apiKey: process.env.OPENAI_API_KEY,
  });
  const openai = new OpenAIApi(configuration);

  // event handler
  async function handleEvent(event) {
    try {
      if (event.type !== 'message' || event.message.type !== 'text') {
        // ignore non-text-message event
        return Promise.resolve(null);
      }
      // creating a completion
      const completion = await openai.createCompletion({
        // Most capable GPT-3 model
        model: 'text-davinci-003',
        prompt: event.message.text,
        // The maximum number of tokens to generate in the completion
        max_tokens: 4000,
      });
      // create a echoing text message with completion res
      const echo = {
        type: 'text',
        text: completion.data.choices[0].text.trim(),
      };

      // use reply API
      return client.replyMessage(event.replyToken, echo);
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
  done();
}

module.exports = fastifyPlugin(lineBotConnector);
```
# 測試 OpenAI Line Bot 功能 
- OpenAI Line Bot 會回答使用者的問題

![line bot ai echo feature.png](http://192.168.25.60:8000/files/file_storage/6987f0fd.png)

# 部署到 Render
## 登入 [Render](https://render.com/) 平台，或註冊一個新的帳號

## 新增一個 Web Service 並 Connect a repository
- 需先將專案 upload 到自己的 github，再連接到 render

## 輸入部屬資料
- `Name` **主機名稱**：Fastify_OpenAI_Line_Bot
- `Build Command` **部屬指令**：`npm install && npx prisma generate --schema=./src/prisma/schema.prisma`
- `Start Command` **運行指令**：`npm start`
- `Auto-Deploy` **自動部屬**：yes，表示 repository 有更新時會自動部屬
- `Environment` **環境變數**：按下 `Advanced` 選擇 `Add Secret File`，將 .env 檔案中的資料匯入

## 部屬成功會顯示 live 並取得 https url

https://fastify-openai-line-bot.onrender.com/

![render deploy success.png](http://192.168.25.60:8000/files/file_storage/00ad1354.png)

# 重新設定 Line Webhook URL
## 在 Webhook settings 中設定 Webhook URL
- 將 https://fastify-openai-line-bot.onrender.com/callback 填入 url
- 按下 **Verify** 來驗證網址是否正確以及專案是否正常
- 呈現 **Success**：表示網址以及專案是正確的

![line bot webhook use webhook with render.png](http://192.168.25.60:8000/files/file_storage/691347f6.png)

# 測試 OpenAI Line Bot 功能 
- OpenAI Line Bot 會回答使用者的問題

![line bot ai echo feature.png](http://192.168.25.60:8000/files/file_storage/6987f0fd.png)