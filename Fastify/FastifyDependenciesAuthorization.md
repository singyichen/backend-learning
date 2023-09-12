---
title: Fastify Dependencies ( Authorization )
description: 權限套件
published: true
date: 2023-09-11T01:42:46.109Z
tags: fastify, authorization, framework
editor: markdown
dateCreated: 2023-09-05T00:06:22.698Z
---

# 權限套件介紹 ( Authorization Dependencies )
- [ ] [ACCESS CONTROL IN NODE.JS WITH FASTIFY AND CASBIN](https://www.nearform.com/blog/access-control-node-js-fastify-and-casbin/)

| 功能 | fastify-casbin | fastify-casbin-rest |
|---|:--:|:--:|
| 可用於任何類型的 Fastify 應用程式 | ✓ | ✓ |
| 提供簡單的 API 來檢查權限 | ✓ | ✓ |
| 支援路徑和操作來匹配請求 | ✕ | ✓ |
| 支援 HTTP 狀態碼來響應拒絕請求 | ✕ | ✓ |
| 支援自定義錯誤訊息 | ✕ | ✓ |

## fastify-casbin
> - A plugin for Fastify that adds support for Casbin.
> - casbin is a peer dependency and must be installed explicitly

> [reference](https://www.npmjs.com/package/fastify-casbin)
{.is-info}

### Install
```shell
npm i casbin fastify-casbin --save
```
### Usage
> [reference](https://github.com/nearform/fastify-casbin)
{.is-info}

- 資料夾結構
```
├─src       
│  ├─plugin
│  │    └─casbin.js
│  │
│  └─casbin
│       ├─abac
│       └─rbac
│           ├─rbac_model.conf
│           └─rbac_policy.csv
```

- 在 `src` 中放置資料夾 `casbin`，依照權限分類放置設定檔
- `rbac_model.conf`：設定 PERM 模型

```
[request_definition]
r = sub, obj, act

[policy_definition]
p = sub, obj, act

[role_definition]
g = _, _

[policy_effect]
e = some(where (p.eft == allow))

[matchers]
m = g(r.sub, p.sub) && r.obj == p.obj && r.act == p.act
```

- `rbac_policy.csv`：政策模型

```
p, alice, data1, read
p, bob, data2, write
p, data2_admin, data2, read
p, data2_admin, data2, write
g, alice, data2_admin
```

- 在 `plugin` 中新增 `casbin.js`

```js
/**
 * @description casbin plugin
 */

const fastifyPlugin = require('fastify-plugin');
const fastifyCasbin = require('fastify-casbin');
const { join } = require('path');

async function casbinConnector(fastify, opts, done) {
  const options = {
    model: join(__dirname, '../casbin/rbac', 'rbac_model.conf'), // the model configuration
    adapter: join(__dirname, '../casbin/rbac', 'rbac_policy.csv'), // the adapter
  };
  fastify.register(fastifyCasbin, options);

  done();
}

module.exports = fastifyPlugin(casbinConnector);
```

- 在 `app.js` 中 import 並 register

```js
// app.js
// import plugin
const fastifyCasbinPlugin = require('./src/plugin/casbin');
function build() {
  // init app
  const app = fastify();

  /**
   * @description plugin
   */
  // plugin
  // casbin
  app.register(fastifyCasbinPlugin);

  return app;
}
const app = build();
```

- 使用方式：`fastify.casbin.enforce('alice', 'data1', 'write')`

```js
'use strict';
/**
 * @description root API 路由
 */
const errorLogger = require('../plugin/logger');
async function router(fastify, opts) {
  fastify.get('/', async function (request, reply) {
    try {
      if (!(await fastify.casbin.enforce('alice', 'data1', 'write'))) {
        reply.send(new Error('Forbidden'));
      }
      console.log(request);
      reply.send({ hello: 'world' });
    } catch (error) {
      errorLogger.error(error);
      reply.send({ status: -1, message: error.message });
      console.log(error);
    }
  });
}

module.exports = router;
```

## casbin-sequelize-adapter
> - Casbin 的一個 Adapter 套件，提供了與 Sequelize ORM 庫集成的功能

> [reference](https://www.npmjs.com/package/casbin-sequelize-adapter)
{.is-info}

### Install
```shell
npm i casbin-sequelize-adapter sequelize sqlite3 --save
```

- `sequelize`：Node.js ORM 庫套件，提供了一種方便的方式來定義模型、執行資料庫查詢和操作資料庫
- `sqlite3`：SQLite 資料庫的 Node.js 驅動程式套件，提供了在 Node.js 中與 SQLite 資料庫進行交互的功能

### Usage
> [reference](https://github.com/node-casbin/sequelize-adapter)
{.is-info}






## fastify-casbin-rest
> - A plugin for Fastify that adds support for Casbin RESTful model.
> - fastify-casbin must be registered in the Fastify instance

> [reference](https://www.npmjs.com/package/fastify-casbin-rest)
{.is-info}

### Install
```shell
npm i fastify-casbin-rest --save
```
### Usage
> [reference](https://github.com/nearform/fastify-casbin-rest)
{.is-info}

