---
title: Fastify Dependencies ( Document )
description: 文件套件
published: true
date: 2023-08-17T06:29:10.212Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-08-01T06:35:10.677Z
---

# 文件套件介紹 ( Document Dependencies )

## @fastify/swagger
> - A Fastify plugin for serving a Swagger UI, using Swagger (OpenAPI v2) or OpenAPI v3 schemas automatically generated from your route schemas, or from an existing Swagger/OpenAPI schema.

> [reference](https://www.npmjs.com/package/@fastify/swagger)
{.is-info}

### Install
```shell
npm install @fastify/swagger@7.6.1 --save 
```

### Usage
#### 線上打卡系統
- app.js

```javascript
// app.js
// import Dependencies
const fastify = require('fastify');
const fs = require('fs');
const path = require('path');

// init logger
const app = fastify({
  logger: true,
  // 設定走 https 協定，部屬到 docker 時才會都走 https ，才能產製出 swagger API 文件
  https: {
    key: fs.readFileSync(
      path.join(__dirname, 'src/https', 'fastify.key')
    ),
    cert: fs.readFileSync(
      path.join(__dirname, 'src/https', 'fastify.cert')
    ),
  },
});

// import routes
// import swaggerRouter
const swaggerRouter = require('./src/routes/swagger/swagger');

// 初始化路由
app.register(swaggerRouter);

// 部屬到 docker 時要加 host: '0.0.0.0'
app.listen({ port: port, host: '0.0.0.0' }, function (err, address) {
  console.log(
    `Welcome to Web-Signin app listening at https://${process.env.IP_ADDRESS}:${process.env.PORT}`
  );
  if (err) {
    console.log(err);
    process.exit(1);
  }
});

module.exports = app;
```
- 參閱 [SSL](/軟體開發/學習心得/11542/Basic/WebDev/SSL) 產製 192.168.25.180-net、192.168.25.180-net.key
- fastify.cert：192.168.25.180-net
- fastify.key：192.168.25.180-net.key

- swagger router

```javascript
// swagger.js
'use strict';
async function router(fastify, opts) {
  // 配置 swagger
  fastify.register(require('@fastify/swagger'), {
    // swagger 文件路由
    routePrefix: '/swagger/WebSignin_API',
    mode: 'static',
    specification: {
      // yaml 或 json 檔案存放位置
      path: './src/routes/swagger/swagger.yaml',
    },
    exposeRoute: true,
  });
}

module.exports = router;
```
- swagger yaml

```yaml
// src/routes/swagger/swagger.yaml
swagger: '2.0'
info:
  description: WebSignin API
  version: 1.0.0
  title: 線上打卡系統 API
  contact:
    email: mandy.chen@hokia.com.tw
  # license:
  #   name: 
  #   url: 
# host: 
# basePath: /v2
tags:
  - name: Forestage
    description: 前台
  - name: Backstage
    description: 後台
schemes:
  - https
  - http  
# routes
paths:
  # 前台
  # 使用者登入
  /api/public/user/login:
    get:
      tags:
        - Forestage
      summary: User login
      description: Returns code and data with a user based on EmployeeName, LoginDay, LoginTime, Token
      operationId: userLogin
      consumes:
        - application/json
        - application/xml
      produces:
        - application/json
        - application/xml
      parameters:
        - name: EmployeeID
          description: Your EmployeeID
          in: query
          required: true
          type: string
          example: 11542
        - name: Password
          description: Your Password
          in: query
          required: true
          type: string
          example: mandy
      responses:
        200:
          description: successful operation
          schema:
            $ref: '#/definitions/successResponse'        
        400:
          description: error
  # 使用者打卡
  /api/public/user/signin:
    post:
      tags:
        - Forestage
      summary: User signin
      description: Returns code and data with a signin based on SignDay, SignTime
      operationId: userSignin
      consumes:
        - application/json
        - application/xml
      produces:
        - application/json
        - application/xml
      parameters:
        - name: signin
          description: signin object
          in: body
          required: true
          schema:
            $ref: '#/definitions/signin'
      responses:
        200:
          description: successful operation
          schema:
            $ref: '#/definitions/successResponse'
        400:
          description: error
        401:
          description: Unauthorized
      security:
        - Bearer: []
# Authorization
securityDefinitions:
  Bearer:
    type: apiKey
    name: Authorization
    in: header
    description: example:Bearer token (Bearer+space+token)
    #example: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJFbXBsb3llZUlEIjoiMTE1NDIiLCJpYXQiOjE2NTc3ODAyMzUsImV4cCI6MTY1Nzc4MDI5NX0.sqaBMb7UP7CtFC_utFmmUnW9Y3gIs3g_Ku4mz_thanY
# Models
definitions:
  # 使用者打卡
  signin:
    type: object
    required:
      - EmployeeID
      - EmployeeName
      - LoginDay
      - LoginTime
      - SignStatus
    properties:
      EmployeeID:
        type: string
        example: 11542
      EmployeeName:
        type: string
        example: 陳欣怡
      LoginDay:
        type: string
        example: 2022/06/14
      LoginTime:
        type: string
        example: 08:00:04
      SignStatus:
        type: string
        example: 上班
  # 成功回應      
  successResponse:
    type: object
    properties:
      code:
        type: string
      data:
        type: object
  # 失敗回應
  errorResponse:
    type: object
    properties:
      code:
        type: string
      message:
        type: string
```

- 查看 API 文件：https://192.168.25.180:10001/swagger/WebSignin_API

![swagger api document for WebSignin_API.png](http://192.168.25.60:8000/files/file_storage/85445df6.png)

#### 資訊看板系統
- 在 `src/routes/swagger` 新增一個 `swagger.js`
- 並在 plugin router 進行 register

```js
'use strict';
/**
 * @description swagger 路由( GET /swagger/e_board_API )
 */
async function router(fastify, opts) {
  // 配置 swagger
  fastify.register(require('@fastify/swagger'), {
    // swagger 文件路由
    routePrefix: '/swagger/e_board_API',
    mode: 'static',
    specification: {
      // yaml 或 json 檔案存放位置
      path: './src/routes/swagger/swagger.yaml',
    },
    exposeRoute: true,
  });
}

module.exports = router;
```

- 在 `src/routes/swagger` 新增一個 `swagger.yaml`
- 查看 API 文件：https://192.168.25.180:8115/swagger/e_board_API 
 
## AsyncAPI


> [reference](https://github.com/asyncapi/cli)
{.is-info}

### Install
- cli

```shell
npm install -g @asyncapi/cli
```

- generator

```shell
npm install -g @asyncapi/generator
```

### Usage
- 列出所有 cli

```
asyncapi 
```

```
All in one CLI for all AsyncAPI tools

VERSION
  @asyncapi/cli/0.52.5 win32-x64 node-v14.18.0

USAGE
  $ asyncapi [COMMAND]

TOPICS
  config    CLI config settings
  generate  Generate models and template
  new       Creates a new asyncapi file
  start     Start asyncapi studio

COMMANDS
  bundle    bundle one or multiple asyncapi documents and their references together.
  config    CLI config settings
  convert   Convert asyncapi documents older to newer versions
  diff      Find diff between two asyncapi files
  generate  Generate typed models or other things like clients, applications or docs using AsyncAPI        
            Generator templates.
  new       Creates a new asyncapi file
  optimize  optimize asyncapi specification file
  start     Start asyncapi studio
  validate  validate asyncapi file
```



