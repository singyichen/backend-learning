---
title: Koa Dependencies ( Document )
description: 文件套件
published: true
date: 2023-05-17T04:00:08.890Z
tags: koa, framework
editor: markdown
dateCreated: 2022-07-26T03:32:28.455Z
---

# 文件套件介紹 ( Document Dependencies )
## koa2-swagger-ui
> - UI 視圖組件
> - Host swagger ui at a given directory from your koa v2 app

> [reference](https://www.npmjs.com/package/koa2-swagger-ui)
{.is-info}
### Install
```shell
npm install koa2-swagger-ui --save
```
### Usage
```javascript
// import Dependencies
const { koaSwagger } = require('koa2-swagger-ui');

// import swaggerRouter
const swaggerRouter = require('./routes/swagger/swagger');

// middlewares
// 配置 swagger
const swaggerOption = {
  // swagger 文件路由
  routePrefix: '/swagger/WebSignin_API',
  // swagger-jsdoc 生成的文件路由
  swaggerOptions: {
    url: '/swagger/swagger.json',
  },
};
app.use(koaSwagger(swaggerOption));

// routes
// swagger
app.use(swaggerRouter.routes()).use(swaggerRouter.allowedMethods());
```

## swagger-jsdoc
> - 識別寫的 /**/  轉成 JSON
> - This library reads your JSDoc-annotated source code and generates an OpenAPI (Swagger) specification

> [reference](https://www.npmjs.com/package/swagger-jsdoc)
{.is-info}
### Install
```shell
npm install swagger-jsdoc --save
```
### Usage
> [reference](https://github.com/rsl140/FE-demo)
{.is-info}

> [reference](https://www.twblogs.net/a/5eeb6c684b16c91a2848f689)
{.is-info}

> [reference](https://mherman.org/blog/swagger-and-nodejs/)
{.is-info}

> [reference](https://editor.swagger.io/?_ga=2.132017415.928281264.1596593063-247237175.1594971087#)
{.is-info}

- swagger route
	- 想要產生的 api 文件檔案 或 yaml 檔案存放位置，位置需精準到檔案的上一層
		- path.join(__dirname, '../api/admin/*.js') ➔ 搭配在 route 寫 api 文件的註解
      - path.join(__dirname, '../swagger/swagger.yaml') ➔ 搭配 yaml 檔案
      
> path.join(__dirname, '../api/*.js') ➔ 位置無精準到檔案的上一層    
{.is-danger}

  
```javascript
// src/routes/swagger/swagger.js
const Router = require('koa-router');
const path = require('path');
const swaggerJSDoc = require('swagger-jsdoc');
const router = new Router({
  // 路由前綴
  prefix: '/swagger',
});
const swaggerDefinition = {
  // api 文件網頁描述
  info: {
    title: '線上打卡系統 API',
    version: '1.0.0',
    description: 'WebSignin API',
  },
};
const options = {
  swaggerDefinition,
  // 想要產生的 api 文件檔案 或 yaml 檔案存放位置，位置需精準到檔案的上一層
  apis: [
    // path.join(__dirname, '../api/admin/*.js'),
    // path.join(__dirname, '../api/public/*.js'),
    path.join(__dirname, '../swagger/swagger.yaml'),
  ],
};
const swaggerSpec = swaggerJSDoc(options);

// 透過路由獲取生成的註解文件
router.get('/swagger.json', async function (ctx) {
  ctx.set('Content-Type', 'application/json');
  ctx.body = swaggerSpec;
});

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

- login router

```javascript
// src/routes/api/public/login.js
/**
 * @description login API 路由
 */

const router = require('koa-router')();
const LoginController = require('../../../controller/public/loginController');
const { errorLogger } = require('../../../middleware/logger');
const { getValidator } = require('../../../middleware/validator');
const { usersValidate } = require('../../../validator/users');
const loginController = new LoginController();

// 路由前綴
router.prefix('/api/public/user');

/**
 * @description 使用者登入路由( GET api/public/user/login )
 * @swagger
 * # Models
 * definitions:
 * # 成功回應
 *   successResponse:
 *    type: object
 *      properties:
 *       code:
 *         type: string
 *       data:
 *        type: object
 * /api/public/user/login:
 *   get:
 *     tags:
 *       - Forestage
 *     summary: User login
 *     description: Returns code and data with a user based on EmployeeName, LoginDay, LoginTime, Token
 *     operationId: userLogin
 *     consumes:
 *       - application/json
 *       - application/xml
 *     produces:
 *       - application/json
 *       - application/xml
 *     parameters:
 *       - name: EmployeeID
 *         description: Your EmployeeID
 *         in: query
 *         required: true
 *         type: string
 *         example: 11542
 *       - name: Password
 *         description: Your Password
 *         in: query
 *         required: true
 *         type: string
 *         example: mandy
 *     responses:
 *       200:
 *         description: successful operation
 *         schema:
 *           $ref: '#/definitions/successResponse'
 *       400:
 *         description: error
 */
router.get(
  '/login',
  // 資料校驗
  getValidator(usersValidate),
  // routes handler
  async (ctx, next) => {
    try {
      ctx.body = await loginController.login(ctx, next);
    } catch (error) {
      errorLogger.error(error);
      ctx.body = { status: -1, message: error.message };
      console.log(error);
    }
  },
);

module.exports = router;

```

### 查看 API 文件
http://192.168.25.180:10000/swagger/WebSignin_API

![swagger api document for WebSignin_API.png](http://192.168.25.60:8000/files/file_storage/85445df6.png)






