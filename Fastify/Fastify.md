---
title: Fastify
description: Fastify
published: true
date: 2023-05-17T07:01:03.671Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-07-29T08:12:59.156Z
---

# Fastify
- [ ] [fastify](https://www.fastify.io/)
- [ ] [fastify cn](https://www.fastify.cn/)
- [ ] [fastify github](https://github.com/fastify/docs-chinese/blob/HEAD/README.md)
- [ ] [fastify npm](https://www.npmjs.com/package/fastify)
- [ ] [fastify docs-chinese](https://github.com/fastify/docs-chinese)
- [ ] [fastify dependencies](https://www.fastify.io/ecosystem/)
- [ ] [Fastify 系列教程一 (路由和日志)](https://www.codeprj.com/zh/blog/75edbf1.html)
- [ ] [Fastify 系列教程二 (中間件、鈎子函數和裝飾器)](https://www.codeprj.com/zh/blog/760b351.html)
- [ ] [Fastify 系列教程三 (驗證、序列化和生命周期)](https://www.codeprj.com/zh/blog/7640691.html)
- [ ] [Fastify 系列教程四 (請求對象、響應對象和插件)](https://www.codeprj.com/zh/blog/764ff91.html)
- [ ] [Creating a Fastify REST API – Postgres, Swagger and Fastify CLI](https://progressivecoder.com/creating-a-fastify-rest-api/)
- [ ] [Fastify 101 系列](https://ithelp.ithome.com.tw/users/20151148/ironman/5108)
- [ ] [使用 Fastify 框架以及 TypeScript 語言來開發 Web 服務的起手式](https://magiclen.org/fastify-in-typescript/)

# Fastify 框架介紹
> **Fastify** 是一個 NodeJS 的 Web Framework，利用 Fastify 可以快速的讓開發者建立後端的伺服器應用

![fastify slogan.png](http://192.168.25.60:8000/files/file_storage/c98d4efa.png)

在 NodeJS 的 Web Framwork 工具中，知名度最高也最多人使用的是 ExpressJS。

在 Full Stack Solution 中，由 Mongodb、Express、React、NodeJS 所構成的 [MERN](https://www.mongodb.com/mern-stack) 也是非常熱門的開發技術。

# Fastify 優勢
## 高性能 ( Highly performant )
Fastify 是這一領域中最快的 web 框架之一，另外，取決於程式碼的複雜性，Fastify 最多可以處理每秒 3 萬次的請求。詳細測試結果可以參考這份 [benchmarks](https://www.fastify.io/benchmarks/)

## 可擴展 ( Extensible )
Fastify 通過其提供的**鉤子（hook）**、 **外掛 ( plugin )** 和 **裝飾器（decorator）** 提供完整的可擴展性

## 基於 Schema ( Schema based )
任何進來的資料都必須被經過驗證 ( Validation )，任何出去的資料都要經過序列化 ( Serialization )，Fastify 內部已經把這部分實作好，開發者只要經過簡單的定義即可完成這兩個重要工作

即使這不是強制性的，我們仍建議使用 [JSON Schema](http://json-schema.org/) 來做路由（route）驗證及輸出內容的序列化，Fastify 在內部將 schema 編譯為高效的函數並執行

## 日誌 ( Logging )
日誌是非常重要且代價高昂的，寫 log 很重要，Fastify 內建已經整合好 [Pino](https://github.com/pinojs/pino) 這個知名的 Logging 工具

## 支援 TypeScript ( TypeScript ready )
Fastify 原生支援 [TypeScript](https://www.typescriptlang.org/)，在強型別特性的支援下，開發過程更不容易出錯，且更舒服

## 對開發人員友好 ( Developer friendly )
框架的使用很友好，幫助開發人員處理日常工作，並且不犧牲性能和安全性

以上幾點對開發人員來說是非常友善的， Fastify 在不犧牲效能及安全性的前提下達到，做為一個後端開發工具，相當推薦大家使用這個 Web Framwork

# Fastify 特色
## 生命週期
下圖展示了 Fastify 的內部生命週期。
每個節點右邊的分支為生命週期的下一階段，左邊的則是上一個生命週期拋出錯誤時產生的錯誤碼 (請注意 Fastify 會自動處理所有的錯誤)。

```
Incoming Request
  │
  └─▶ Routing
        │
        └─▶ Instance Logger
             │
   4**/5** ◀─┴─▶ onRequest Hook
                  │
        4**/5** ◀─┴─▶ preParsing Hook
                        │
              4**/5** ◀─┴─▶ Parsing
                             │
                   4**/5** ◀─┴─▶ preValidation Hook
                                  │
                            400 ◀─┴─▶ Validation
                                        │
                              4**/5** ◀─┴─▶ preHandler Hook
                                              │
                                    4**/5** ◀─┴─▶ User Handler
                                                    │
                                                    └─▶ Reply
                                                          │
                                                4**/5** ◀─┴─▶ preSerialization Hook
                                                                │
                                                                └─▶ onSend Hook
                                                                      │
                                                            4**/5** ◀─┴─▶ Outgoing Response
                                                                            │
                                                                            └─▶ onResponse Hook
```
在 `使用者編寫的處理函數` 執行前或執行時，你可以呼叫 `reply.hijack()` 以使得 Fastify：

- 終止運行所有鉤子及使用者的處理函數
- 不再自動傳送響應

特別注意：假如使用了 `reply.raw` 來傳送響應，則 `onResponse` 依舊會執行。

## 響應生命週期
不管使用者如何處理請求，結果無非以下幾種：

- 非同步函數中返回 payload
- 非同步函數中拋出 Error
- 同步函數中傳送 payload
- 同步函數中傳送 Error 實例

當響應被劫持時 (即呼叫了 `reply.hijack()`) 會跳過之後的步驟，否則，響應被提交後的資料流向如下：

```
                        ★ schema validation Error
                                    │
                                    └─▶ schemaErrorFormatter
                                               │
                          reply sent ◀── JSON ─┴─ Error instance
                                                      │
                                                      │         ★ throw an Error
                     ★ send or return                 │                 │
                            │                         │                 │
                            │                         ▼                 │
       reply sent ◀── JSON ─┴─ Error instance ──▶ setErrorHandler ◀─────┘
                                                      │
                                 reply sent ◀── JSON ─┴─ Error instance ──▶ onError Hook
                                                                                │
                                                                                └─▶ reply sent
```

註：`reply sent` 意味著 `JSON payload` 將會如下被序列化：

- 通過 [響應序列化方法](https://github.com/fastify/docs-chinese/blob/master/docs/Server.md#setreplyserializer)  (如有設定)
- 或當為返回的 HTTP 狀態碼設定了 JSON schema 時，通過 [序列化函數生成器](https://github.com/fastify/docs-chinese/blob/master/docs/Server.md#setserializercompiler)
- 或通過默認的 `JSON.stringify` 函數

# 環境安裝
## 全域安装 fastify-cli ( Install Cli )
```shell
npm install fastify-cli --global
```
## 進入在建立專案的資料夾 ( cd to the Project )
```shell
cd WebSignin_Fastify_API
```
## 初始化 Fastify 專案 ( Init Fastify App )
```shell
npm init fastify
```

![npm init fastify.png](http://192.168.25.60:8000/files/file_storage/a64f5107.png)

## 程式基本結構
### 顯示目錄結構
```shell
tree /f
```

![顯示程式目錄結構.png](http://192.168.25.60:8000/files/file_storage/39fee340.png)

### 將目錄結構整理如下 (複製貼上即可)
```
│  .gitignore                       # git 忽略檔案
│  app.js                           # 專案程式啟動檔案
│  package.json                     # 描述檔案、套件訊息、開發者訊息
│  README.md
│
├─plugins                           # 插件
│      README.md
│      sensible.js
│      support.js
│
├─routes                            # 路由
│  │  README.md
│  │  root.js
│  │
│  └─example
│          index.js
│
└─test                              # 測試
    │  helper.js
    │
    ├─plugins
    │      support.test.js
    │
    └─routes
            example.test.js
            root.test.js
```
### 目錄
1. plugins：插件
2. routes：路由
3. test：測試
4. .gitignore：git忽略檔案
5. README.md：專案說明文件
6. app.js：專案程式啟動檔案
7. package-lock.json：套件詳細訊息
8. package.json：描述文件及開發者訊息

## 安裝套件 ( Install dependencies )
```shell
npm i
```
## 執行專案 ( Run the app )
```shell
npm run dev
```
```
> websignin_fastify_api@1.0.0 dev D:\mandy\Project\Fastify\WebSignin_Fastify_API
> fastify start -w -l info -P app.js

[1659315175504] INFO (9316 on SY0216006-MIS): Server listening at http://127.0.0.1:3000
[1659315175507] INFO (9316 on SY0216006-MIS): Server listening at http://[::1]:3000
```
## 連接 http://localhost:3000
頁面會出現如下
```
{"root":true}
```

## 在[版本控制系統](http://192.168.25.60:9001/)新增一個儲存庫

取得遠端連線網址：http://192.168.25.60:9001/11542/WebSignin_Fastify_API.git

## git 初始化
```shell
git init
```

## 設定遠端連線
```shell
git remote add origin http://192.168.25.60:9001/11542/WebSignin_Fastify_API.git
```

## 程式碼提交 ( commit code )
```shell
git add .
git commit -m "first commit"
```
- first commit 內容
```
feat：first commit

原因：初始化專案

新增項目：
1. plugins：插件
2. routes：路由
3. test：測試
4. .gitignore：git 忽略檔案
5. README.md：專案說明文件
6. app.js：專案程式啟動檔案
7. package-lock.json：套件詳細訊息
8. package.json：描述文件及開發者訊息
```

## 從命令行推送已經建立的儲存庫
```shell
git push -u origin master 
```

## 建立分支並切換到分支
```shell
git checkout -b dev 
git checkout -b featureCreateAPI 
```

## 從命令行推送至遠端分支
```shell
git push -u origin dev
git push -u origin featureCreateAPI
```

## 將 master 進行保護
- 專案 ➔ 設定 ➔ 分支 ➔ 分支保護選擇 master ➔ 啟用分支保護 ➔ 啟用推送

## 將 dev 設定成預設分支 ( 提交程式碼和合併請求的預設分支 )
- 專案 ➔ 設定 ➔ 分支 ➔ 預設分支選擇 dev

![git 分支預設分支及分支保護.png](http://192.168.25.60:8000/files/file_storage/97b91a16.png)

## 合併分支
```shell
git checkout dev
git merge featureCreateAPI --no-ff
```

## 從命令行推送已經建立的儲存庫 
```shell
git push -u origin dev
git push -u origin master
```

## Docker 部屬

https://github.com/fastify/fastify/issues/935
https://www.fastify.io/docs/latest/Guides/Getting-Started/
http://greens2314.blogspot.com/2018/12/local-postman-https-api.html

```javascript
// 部屬到 docker 時要加 host: '0.0.0.0'
Note
The above examples, and subsequent examples in this document, default to listening only on the localhost 127.0.0.1 interface. To listen on all available IPv4 interfaces the example should be modified to listen on 0.0.0.0 like so:

fastify.listen({ port: 3000, host: '0.0.0.0' }, function (err, address) {
  if (err) {
    fastify.log.error(err)
    process.exit(1)
  }
  fastify.log.info(`server listening on ${address}`)
})
Similarly, specify ::1 to accept only local connections via IPv6. Or specify :: to accept connections on all IPv6 addresses, and, if the operating system supports it, also on all IPv4 addresses.

When deploying to a Docker (or another type of) container using 0.0.0.0 or :: would be the easiest method for exposing the application.
```
```javascript
// 部屬到 docker 時要加 host: '0.0.0.0'
本檔案中的範例，預設情況下只監聽本地 127.0.0.1 連接埠。要監聽所有有效的 IPv4 連接埠，需要將程式碼修改為監聽 0.0.0.0，如下所示：

fastify.listen(3000, '0.0.0.0', function (err, address) {
  if (err) {
    fastify.log.error(err)
    process.exit(1)
  }
  fastify.log.info(`server listening on ${address}`)
})
類似地，::1 表示只允許本地的 IPv6 連接。而 :: 表示允許所有 IPv6 地址的接入，當作業系統支援時，所有的 IPv4 地址也會被允許。

當使用 Docker 或其他容器部署時，使用 0.0.0.0 或 :: 會是最簡單的暴露應用的方式。
```

### 到專案資料夾
```shell
cd WebSignin_Fastify_API
```

### 打包 Docker 映像檔
```shell
docker build -t websignin_fastify_api --no-cache .
```

### 執行映像檔
```shell
docker run -p 10001:10001 websignin_fastify_api
```
### 測試專案是否成功運作

連接 http://192.168.25.180:10001/

### 查看 log
#### 先查看 container 的 name
```shell
docker ps -a
```
#### 進入到 container 裡面
```shell
sudo docker exec -ti <container_name> bash
```
#### 到要查看的 log 資料夾中使用 cat 查看 log 檔案資料
```shell
root@a5b1ea5f7b86:/src/logs/info# cat 2022-08-26-info.log
```

![docker exec -ti container_name bash.png](http://192.168.25.60:8000/files/file_storage/df2bb25e.png)

## 將專案重構使用共用函式庫
參閱文件 [Separate Project Method](/軟體開發/學習心得/11542/Fastify/SeparateProjectMethod)