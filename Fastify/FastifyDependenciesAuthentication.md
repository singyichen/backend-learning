---
title: Fastify Dependencies ( Authentication )
description: 驗證套件
published: true
date: 2023-05-17T07:03:47.000Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-08-11T00:39:46.068Z
---

# 驗證套件介紹 ( Authentication Dependencies )
## ldapjs
> LDAPjs makes the LDAP protocol a first class citizen in Node.js.

> [reference](http://ldapjs.org/s) 
{.is-info}

### Install
> [reference](https://www.npmjs.com/package/ldapjs) 
{.is-info}

```shell
npm i ldapjs --save
```
### Usage
> [reference](https://github.com/ldapjs/node-ldapjs) 
{.is-info}


## fast-jwt
> - Fast JSON Web Token implementation.
> - 產生 JWT
> - [JWT Concept](/軟體開發/學習心得/11542/Basic/WebDev/JWT)

### Install
> [reference](https://www.npmjs.com/package/fast-jwt) 
{.is-info}

```shell
npm i fast-jwt --save
```
### Usage
> [reference](https://github.com/nearform/fast-jwt) 
{.is-info}

- 產生 JWT

```javascript
// verificationService.js
// import
const { createSigner } = require('fast-jwt');

// 產生 JWT
const payload = { EmployeeID: employeeID };
const secret = process.env.JWT_PASSPORT_SECRET;
// expiresIn 過期時間(單位為毫秒)
const expiresIn = +process.env.JWT_EXPIRESIN;
const signSync = createSigner({ key: secret, expiresIn: expiresIn });
const token = signSync(payload);
```

## @fastify/jwt
> - JWT utils for Fastify, internally it uses fast-jwt.
> - 產生 JWT
> - [JWT Concept](/軟體開發/學習心得/11542/Basic/WebDev/JWT)

> @fastify/jwt 需在 router 才能取得 JWT 較不方便，故暫時不使用
{.is-warning}

### Install
> [reference](https://www.npmjs.com/package/@fastify/jwt) 
{.is-info}

```shell
npm i @fastify/jwt --save
```
### Usage
> [reference](https://github.com/fastify/fastify-jwt) 
{.is-info}

## @fastify/passport
> - @fastify/passport is a port of passport for the Fastify ecosystem. It lets you use Passport strategies to authenticate requests and protect Fastify routes!

### Install
> [reference](https://www.npmjs.com/package/@fastify/passport) 
{.is-info}

```shell
npm i @fastify/passport --save
```
### Usage
> [reference](https://github.com/fastify/fastify-passport) 
{.is-info}


## @fastify/bearer-auth
> @fastify/bearer-auth provides a simple request hook for the Fastify web framework.

### Install
> [reference](https://www.npmjs.com/package/@fastify/bearer-auth) 
{.is-info}

```shell
npm i @fastify/bearer-auth --save
```
### Usage
> [reference](https://github.com/fastify/fastify-bearer-auth) 
{.is-info}





## @fastify/auth
> This module does not provide an authentication strategy, but it provides a very fast utility to handle authentication (and multiple strategies) in your routes, without adding overhead.

### Install
> [reference](https://www.npmjs.com/package/@fastify/auth) 
{.is-info}

```shell
npm i @fastify/auth --save
```
### Usage
> [reference](https://github.com/fastify/fastify-auth) 
{.is-info}

- 在 app.js 中 register auth

```javascript
// app.js
// import
const fastifyAuth = require('@fastify/auth');
// init app
const app = fastify({
  // init logger
  logger: true,
});

// auth
app.register(fastifyAuth);
// jwt
app.register(fastifyJwtPlugin);
```

- 設定 JWT 驗證策略

```javascript
// plugin/jwt.js
/**
 * @description jwt plugin
 */

const fastifyPlugin = require('fastify-plugin');
const { createVerifier } = require('fast-jwt');
const VerificationService = require('../service/public/verificationService');
const verificationService = new VerificationService();

async function jwtConnector(fastify, opts, done) {
  // 註冊驗證策略 'verifyJWT'
  fastify.decorate('verifyJWT', async function (request, reply, done) {
    try {
      const verifySync = createVerifier({
        key: process.env.JWT_PASSPORT_SECRET,
      });

      if (request.headers.authorization) {
        const headers = request.headers.authorization.toString().split(' ');
        const authorizationToken = headers[1];
        // JWT 驗證 (會檢查 Signature 是否合法 與 Token 是否過期)
        const payload = verifySync(authorizationToken);
        const userInfo = await verificationService.systemVerification(
          payload.EmployeeID,
        );
        const token = userInfo.Token;
        // 與 DB Token 比對
        if (authorizationToken !== token) {
          return done(Error('The token is invalid.'));
        }
      } else {
        return done(Error('The token is required.'));
      }
    } catch (err) {
      reply.send(err);
    }
  });
  done();
}

module.exports = fastifyPlugin(jwtConnector);

```

- 設定 bearerAuth 驗證

```javascript
/**
 * @description bearerAuth plugin
 */
const fastifyPlugin = require('fastify-plugin');
const fastifyBearerAuth = require('@fastify/bearer-auth');
const key = process.env.JWT_PASSPORT_SECRET;
const keys = { keys: new Set([key]) };

async function bearerAuthConnector(fastify, opts, done) {
  fastify.register(fastifyBearerAuth, {
    addHook: false,
    keys,
    verifyErrorLogLevel: 'debug',
  });
  done();
}

module.exports = fastifyPlugin(bearerAuthConnector);
```

- 在 router 中進行 JWT 驗證：使用 fastify.auth([fastify.verifyJWT])

```javascript
// signin.js
/**
 * @description signin API 路由
 */

const SigninController = require('../../../controller/public/signinController');
const signinSchema = require('../../../validator/public/signin');
const signinController = new SigninController();

/**
 * @description 使用者打卡路由( POST api/public/user/signin )
 */
async function router(fastify, opts) {
  fastify.post('/signin', {
    // signin 資料格式驗證
    schema: {
      body: signinSchema,
    },
    // JWT 驗證
    preHandler: fastify.auth([
      fastify.verifyBearerAuth,
      fastify.verifyJWT,
    ]),
    handler: async function (request, reply) {
      try {
        const res = await signinController.signin(request);
        reply.send(res);
      } catch (error) {
        reply.send({ status: -1, message: error.message });
        console.log(error);
      }
    },
  });
}

module.exports = router;

```

