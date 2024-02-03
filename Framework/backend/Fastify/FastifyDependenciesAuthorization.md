---
title: Fastify Dependencies ( Authorization )
description: 權限套件
published: true
date: 2023-09-20T03:16:22.082Z
tags: fastify, authorization, framework
editor: markdown
dateCreated: 2023-09-05T00:06:22.698Z
---

# 權限套件介紹 ( Authorization Dependencies )
- [ ] [ACCESS CONTROL IN NODE.JS WITH FASTIFY AND CASBIN](https://www.nearform.com/blog/access-control-node-js-fastify-and-casbin/)
- [ ] [Supported Models Examples](https://github.com/casbin/casbin/tree/master/examples)

## 套件功能比較表
| 功能 | casbin | fastify-casbin | fastify-casbin-rest |
|---|:--:|:--:|:--:|
| 可用於任何類型的 Fastify 應用程式 | ✓ | ✓ | ✓ |
| 提供簡單的 API 來檢查權限 | ✓ | ✓ | ✓ |
| 支援路徑和操作來匹配請求 | ✕ | ✕ | ✓ |
| 支援 HTTP 狀態碼來響應拒絕請求 | ✕ | ✕ | ✓ |
| 支援自定義錯誤訊息 | ✕ | ✕ | ✓ |
| 權限 API 完整度 | ✓ | ✕ | ✕ |

## 不同權限使用套件建議表
| 權限 | casbin| fastify-casbin | fastify-casbin-rest |
| 功能 | 提供權限的 API | fastify-casbin-rest 相依套件 | 支援路徑和操作來匹配請求 |
|---|:--:|:--:|:--:|
| RBAC | ✓ | ✓ | ✓ |
| ABAC | ✓ | ✕ | ✕ |


## fastify-casbin
> - A plugin for Fastify that adds support for Casbin.

> [reference](https://www.npmjs.com/package/fastify-casbin)
{.is-info}

### Install
> 注意有相依套件
{.is-warning}

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
// root.js
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

## casbin-prisma-adapter
> - 一個用於將 Casbin 權限管理庫與 Prisma ORM 集成的適配器

> [reference](https://www.npmjs.com/package/casbin-prisma-adapter)
{.is-info}

### Install
```shell
npm i casbin-prisma-adapter --save
```

### Usage
> [reference](https://github.com/node-casbin/prisma-adapter)
{.is-info}

> 使用原因：將 `rbac_policy.csv` 中的 policy 資料放在資料庫管理


- 要使用 `casbin-prisma-adapter` 需做以下套件版本調整及 node 版本調整
1. 調整 @prisma/client 版本：v5.2.0
2. 調整 casbin 版本：v5.16.0
3. 調整 prisma 版本：v5.2.0
4. 調整 dev scripts：`nodemon app.js`
5. 調整 node version 到 v18.17.0，因 Prisma 5.2.0 only supports Node.js >= 16.13

- 在 `schema.prisma` 中新增一個 model `CasbinRule`

```
model CasbinRule {
  id         Int       @id @default(autoincrement())
  ptype      String
  v0         String?
  v1         String?
  v2         String?
  v3         String?
  v4         String?
  v5         String?
  created_at DateTime  @default(now())
  updated_at DateTime? @default(now())

  @@map("casbin_rule")
}
```

- 寫入測試資料

```
INSERT INTO casbin_rule (ptype, v0, v1, v2, v3, v4, v5) VALUES
('p', 'alice', 'data1', 'read', NULL, NULL, NULL),
('p', 'bob', 'data2', 'write', NULL, NULL, NULL),
('p', 'data2_admin', 'data2', 'read', NULL, NULL, NULL),
('p', 'data2_admin', 'data2', 'write', NULL, NULL, NULL),
('g', 'alice', 'data2_admin', NULL, NULL, NULL, NULL),
('p', 'admin', 'data1', 'read', NULL, NULL, NULL),
('p', 'admin', 'data1', 'write', NULL, NULL, NULL),
('p', 'user', 'data1', 'read', NULL, NULL, NULL),
('g', '11542', 'admin', NULL, NULL, NULL, NULL),
('g', '11548', 'user', NULL, NULL, NULL, NULL);
```

- 在 `casbin.js` 新增 `PrismaAdapter` 的一個適配器，配置 `prisma client`

```js
/**
 * @description casbin plugin
 */

const fastifyPlugin = require('fastify-plugin');
const fastifyCasbin = require('fastify-casbin');
const { join } = require('path');
const { PrismaAdapter } = require('casbin-prisma-adapter');
const prismaClientService = require('../ormService/prismaClientService');

async function casbinConnector(fastify, opts, done) {
  const model = join(__dirname, '../casbin/rbac', 'rbac_model.conf');
  const adapter = await PrismaAdapter.newAdapter(prismaClientService.prisma);
  const options = {
    model: model, // the model configuration
    adapter: adapter, // the adapter
  };
  fastify.register(fastifyCasbin, options);

  done();
}

module.exports = fastifyPlugin(casbinConnector);

```

- 使用方式：`fastify.casbin.enforce('alice', 'data1', 'write')`，並使用變數 `isAllowed` 做邏輯判斷

```js
// root.js
'use strict';
/**
 * @description root API 路由
 */
const errorLogger = require('../plugin/logger');
async function router(fastify, opts) {
  fastify.get('/', async function (request, reply) {
    try {
      const isAllowed = await fastify.casbin.enforce('11548', 'data1', 'read');
      if (!isAllowed) {
        reply.send(new Error('Forbidden'));
      }
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

## fastify-casbin-rest
> - A plugin for Fastify that adds support for Casbin RESTful model.
> - fastify-casbin must be registered in the Fastify instance

> [reference](https://www.npmjs.com/package/fastify-casbin-rest)
{.is-info}

### Install
```shell
npm i casbin fastify-casbin fastify-casbin-rest --save
```
### Usage
> [reference](https://github.com/nearform/fastify-casbin-rest)
{.is-info}

- 在 `casbin.js` 新增 `fastifyCasbinRest` 並 register

```js
/**
 * @description casbin plugin
 */

const fastifyPlugin = require('fastify-plugin');
const fastifyCasbin = require('fastify-casbin');
const fastifyCasbinRest = require('fastify-casbin-rest');
const { join } = require('path');
const { PrismaAdapter } = require('casbin-prisma-adapter');
const prismaClientService = require('../ormService/prismaClientService');

async function casbinConnector(fastify, opts, done) {
  const model = join(__dirname, '../casbin/rbac', 'rbac_model.conf');
  const adapter = await PrismaAdapter.newAdapter(prismaClientService.prisma);
  const options = {
    model: model, // the model configuration
    adapter: adapter, // the adapter
  };
  fastify.register(fastifyCasbin, options);
  fastify.register(fastifyCasbinRest);

  done();
}

module.exports = fastifyPlugin(casbinConnector);
```

- 可在 router 直接設定檢核權限邏輯

```js
// root.js
'use strict';
/**
 * @description root API 路由
 */
const errorLogger = require('../plugin/logger');
async function router(fastify, opts) {
  fastify.get('/', {
    casbin: {
      rest: {
        getSub: (request) => request.query.user_id,
        getObj: 'data1',
        getAct: 'read',
      },
    },
    handler: async (request, reply) => {
      try {
        reply.send({ hello: 'world' });
      } catch (error) {
        errorLogger.error(error);
        reply.send({ status: -1, message: error.message });
        console.log(error);
      }
    },
  });
}

module.exports = router;
```

- 無訪問權限會是固定的錯誤訊息

```
{
    "statusCode": 403,
    "error": "Forbidden",
    "message": "11540 not allowed to read data1"
}
```

- 若需客製化錯誤訊息需在 `casbin.js` 新增 `onDeny` 事件，回傳自定義的錯誤訊息並寫入日誌當中

```js
/**
 * @description casbin plugin
 */

const fastifyPlugin = require('fastify-plugin');
const fastifyCasbin = require('fastify-casbin');
const fastifyCasbinRest = require('fastify-casbin-rest');
const { join } = require('path');
const { PrismaAdapter } = require('casbin-prisma-adapter');
const prismaClientService = require('../ormService/prismaClientService');
const { permissionDeniedInfo } = require('../utils/errorInfo');
const errorLogger = require('../plugin/logger');
async function casbinConnector(fastify, opts, done) {
  const model = join(__dirname, '../casbin/rbac', 'rbac_model.conf');
  const adapter = await PrismaAdapter.newAdapter(prismaClientService.prisma);
  const options = {
    model: model, // the model configuration
    adapter: adapter, // the adapter
  };
  fastify.register(fastifyCasbin, options);
  fastify.register(fastifyCasbinRest, {
    onDeny: (reply, { sub, obj, act }) => {
      const message = `${sub} 無 ${act} ${obj} 權限`;
      reply.code(403).send({
        statusCode: permissionDeniedInfo.statusCode,
        code: permissionDeniedInfo.code,
        error: permissionDeniedInfo.error,
        message: message,
      });
      errorLogger.error(message);
    },
  });
  done();
}

module.exports = fastifyPlugin(casbinConnector);
```

- 自定義的錯誤訊息

```
{
    "statusCode": 403,
    "code": 40301,
    "error": "Forbidden",
    "message": "11540 無 read data1 權限"
}
```

- 若要有超級使用者 `admin` 的權限設定，需同時調整 `rbac_model.conf` 與 `casbin_rule` 中資料
- `rbac_model.conf`：調整 `matchers` 邏輯
- `keyMatch()` 主要是用來做路由資源的配對 ，可參考 [Functions in matchers](https://casbin.org/docs/function)
- `p.obj == '*'` 與 `p.act == '*'`，讓 `admin` 有所有資源的所有操作權限
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
m = g(r.sub, p.sub) && (keyMatch(r.obj, p.obj) || p.obj == '*') && (r.act == p.act || p.act == '*')
```

- 寫入測試資料

```
INSERT INTO casbin_rule (ptype, v0, v1, v2) VALUES
  ('p', 'admin', '*', '*'),
  ('p', 'user', '/', 'get'),
  ('p', 'manager', '/addUser', 'post'),
  ('g', 'manager', 'user', null),
  ('g', '11542', 'admin', null),
  ('g', '11548', 'user', null),
  ('g', '11284', 'manager', null);
```

## casbin
> - casbin is a peer dependency and must be installed explicitly

> [reference](https://www.npmjs.com/package/casbin)
{.is-info}

### Install
```shell
npm i fastify-casbin --save
```
### Usage
> [reference](https://github.com/casbin/node-casbin)
{.is-info}


- 在 `plugin` 中新增 `enforcer.js`

```js
// enforcer.js
/**
 * @description enforcer plugin
 */

const { newEnforcer } = require('casbin');
const createCasbinConfig = require('../../config/casbinConfig');

async function createEnforcer() {
  const options = await createCasbinConfig();
  const enforcer = await newEnforcer(options.model, options.adapter);
  return enforcer;
}

module.exports = createEnforcer;
```


#### ABAC 權限範例
- `abac_model.conf`：`sub_rule` 設定使用權限條件

```
[request_definition]
r = sub, obj, act

[policy_definition]
p = sub_rule, obj, act

[policy_effect]
e = some(where (p.eft == allow))

[matchers]
m = eval(p.sub_rule) && r.obj == p.obj && r.act == p.act
```

- 寫入測試資料

```
INSERT INTO casbin_rule (ptype, v0, v1, v2) VALUES
  ('p', 'r.sub.Age > 18', '/data1', 'read'),
  ('p', 'r.sub.Age < 60', '/data2', 'write');
``` 

- `casbinConfig.js`：使用 abac 的 model conf 檔

```js
// casbinConfig.js
/**
 * @description casbin config
 */

const { join } = require('path');
const { PrismaAdapter } = require('casbin-prisma-adapter');
const prismaClientService = require('../src/ormService/prismaClientService');
async function createCasbinConfig() {
  const model = join(__dirname, '../src/casbin/abac', 'abac_model.conf');
  const adapter = await PrismaAdapter.newAdapter(prismaClientService.prisma);
  // const adapter = join(__dirname, '../src/casbin/abac', 'abac_policy.csv');
  const options = {
    model: model, // the model configuration
    adapter: adapter, // the adapter
  };
  return options;
}

module.exports = createCasbinConfig;
```

- 使用 `enforcer.enforce(query, '/data1', 'read')` 做權限檢核

```js
// root.js
'use strict';
/**
 * @description root API 路由
 */
const errorLogger = require('../plugin/logger');
const enforcerCreator = require('../plugin/enforcer');
const { permissionDeniedInfo } = require('../utils/errorInfo');
async function router(fastify, opts) {
  fastify.get('/abac', {
    handler: async (request, reply) => {
      try {
        const enforcer = await enforcerCreator();
        const query = request.query;
        const p = await enforcer.enforce(query, '/data1', 'read');
        if (!p) {
          const age = query.Age;
          const message = `Age ${age} 無 /data1 read 權限`;
          reply.code(403).send({
            statusCode: permissionDeniedInfo.statusCode,
            code: permissionDeniedInfo.code,
            error: permissionDeniedInfo.error,
            message: message,
          });
        }
        reply.send({ hello: 'world' });
      } catch (error) {
        errorLogger.error(error);
        reply.send({ status: -1, message: error.message });
        console.log(error);
      }
    },
  });
}

module.exports = router;

```

- API 需在 `request.query` 傳入檢核參數

```
{{localUrl}}/?Age=20
```

#### RBAC 權限範例
- `casbinConfig.js`：使用 rbac 的 model conf 檔

```js
// casbinConfig.js
/**
 * @description casbin config
 */

const { join } = require('path');
const { PrismaAdapter } = require('casbin-prisma-adapter');
const prismaClientService = require('../src/ormService/prismaClientService');
async function createCasbinConfig() {
  const model = join(__dirname, '../src/casbin/rbac', 'rbac_model.conf');
  const adapter = await PrismaAdapter.newAdapter(prismaClientService.prisma);
  // const adapter = join(__dirname, '../src/casbin/rbac', 'rbac_policy.csv');
  const options = {
    model: model, // the model configuration
    adapter: adapter, // the adapter
  };
  return options;
}

module.exports = createCasbinConfig;
```

- 可在 router 直接設定檢核權限邏輯

```js
// root.js
'use strict';
/**
 * @description root API 路由
 */
const errorLogger = require('../plugin/logger');
async function router(fastify, opts) {
  fastify.get('/rbac', {
    casbin: {
      rest: {
        getSub: (request) => request.query.user_id,
        getObj: '/',
        getAct: 'get',
      },
    },
    handler: async (request, reply) => {
      try {
        reply.send({ hello: 'world' });
      } catch (error) {
        errorLogger.error(error);
        reply.send({ status: -1, message: error.message });
        console.log(error);
      }
    },
  });
}

module.exports = router;
```

- 可在 service 中使用 `enforcer` 提供的 API 做權限相關功能

```js
// permissionsService.js
const errorLogger = require('../../../../plugin/logger');
const enforcerCreator = require('../../../../plugin/enforcer');
/**
 * @description permissions service
 */
class PermissionsService {
  /**
   * @description 取得多筆角色權限
   * @param { string } role 角色
   * @returns { Boolean } Boolean
   */
  async getPermissionsForUser(role) {
    try {
      const enforcer = await enforcerCreator();
      return await enforcer.getPermissionsForUser(role);
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
  /**
   * @description 新增單筆角色權限
   * @param { string } role 角色
   * @param { array } permission 權限
   * @returns { Boolean } Boolean
   */
  async addPermissionForUser(role, permission) {
    try {
      const enforcer = await enforcerCreator();
      return await enforcer.addPermissionForUser(role, ...permission);
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
  /**
   * @description 刪除單筆角色權限
   * @param { string } role 角色
   * @param { array } permission 權限
   * @returns { Boolean } Boolean
   */
  async deletePermissionForUser(role, permission) {
    try {
      const enforcer = await enforcerCreator();
      return await enforcer.deletePermissionForUser(role, ...permission);
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
  /**
   * @description 取得多筆人員角色
   * @param { string } role 角色
   * @returns { Boolean } Boolean
   */
  async getUsersForRole(role) {
    try {
      const enforcer = await enforcerCreator();
      return await enforcer.getUsersForRole(role);
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
  /**
   * @description 新增單筆人員角色
   * @param { string } id id
   * @param { string } role 角色
   * @returns { Boolean } Boolean
   */
  async addRoleForUser(id, role) {
    try {
      const enforcer = await enforcerCreator();
      return await enforcer.addRoleForUser(id, role);
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
  /**
   * @description 刪除單筆人員角色
   * @param { string } id id
   * @param { string } role 角色
   * @returns { Boolean } Boolean
   */
  async deleteRoleForUser(id, role) {
    try {
      const enforcer = await enforcerCreator();
      return await enforcer.deleteRoleForUser(id, role);
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
}

module.exports = PermissionsService;

```

#### RBAC+ABAC 權限範例