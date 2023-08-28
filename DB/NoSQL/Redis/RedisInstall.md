---
title: Redis Install
description: Redis 安裝
published: true
date: 2023-08-25T01:17:13.341Z
tags: db, redis
editor: markdown
dateCreated: 2023-08-25T01:17:13.341Z
---

# Redis Install

# 安裝 Redis for Windows
## 下載 Redis-x64-3.0.504.msi 並安裝
Redis 主要是運行在 Linux 環境，因此在官網下載區是找不到 Windows 版安裝程式，需要到 [MicrosoftArchive/redis](https://github.com/MicrosoftArchive/redis) 提供的 github 頁面中點擊 [release page](https://github.com/microsoftarchive/redis/releases) 連結才有 Windows for Redis 安裝檔

# 安裝 圖形化使用者管理介面 GUI 
## 下載 Redis Desktop Manager：redis-desktop-manager-0.8.8.384.exe 並安裝
官網安裝最新版目前是要收費的，可以先使用它之前的版本(免費)，到官方 [github](https://github.com/uglide/RedisDesktopManager/releases/tag/0.8.8) 有提供舊的版本檔案下載

## 測試本機連線
點選 **Connect to Redis Server**，並輸入以下資訊，再按下 **Test Connection** ，連線成功後可以在左邊看到預設建立的 16 個 DB
- **Name**：本機 localhost
- **Host**：127.0.0.1
- **Port**：6349 (default)

![redis connect to redis server test connection.png](http://192.168.25.60:8000/files/file_storage/2fb7eb04.png)

# 安裝 Redis Client for Node.js
## 安裝套件 [@fastify/redis](https://github.com/redis/@fastify/redis)
> Fastify Redis connection plugin

```
npm install @fastify/redis --save
```

```
feat：add dependency @fastify/redis

原因：新增套件 @fastify/redis

調整項目：
1. package.json：
    - 新增套件 @fastify/redis ：Fastify Redis connection plugin
```

## 新增一個 redis plugin 
```js
/**
 * @description redis plugin
 */

const fastifyPlugin = require('fastify-plugin');
const fastifyRedis = require('@fastify/redis');
const errorLogger = require('./logger');

async function redisConnector(fastify) {
  fastify.register(fastifyRedis, {
    host: '127.0.0.1',
    // 設為 true 表示在 app 關閉時中斷連線
    closeClient: true,
  });
  /**
   * @description 取得單筆 redis 資料路由( GET /redis )
   */
  fastify.get('/redis', async (request, reply) => {
    try {
      const { redis } = fastify;
      const { key } = request.query;
      // redis 取得資料
      await redis.get(key, (err, val) => {
        reply.send(err || JSON.parse(val));
      });
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  });

  /**
   * @description 新增單筆 redis 資料路由( POST /redis )
   */
  fastify.post('/redis', async (request, reply) => {
    try {
      const { redis } = fastify;
      const { key, value } = request.body;
      // redis 新增資料
      await redis.set(key, JSON.stringify(value), (err) => {
        if (err) {
          errorLogger.error(err);
        }
      });
      // 回傳資料
      await redis.get(key, (err, val) => {
        reply.send(err || JSON.parse(val));
      });
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  });

  /**
   * @description 刪除單筆 redis 資料路由( DELETE /redis )
   */
  fastify.delete('/redis', async (request, reply) => {
    try {
      const { redis } = fastify;
      const { key } = request.query;
      // redis 刪除資料
      await redis.del(key, (err) => {
        reply.send(err || { status: 'ok' });
      });
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  });
}

module.exports = fastifyPlugin(redisConnector);

```
## 在 app.js 中 register redis plugin
```js
// import plugin
const fastifyRedisPlugin = require('./src/plugin/redis');
// init app
const app = fastify({
  // init logger
  logger: true,
});
// redis
app.register(fastifyRedisPlugin);
```
## Postman 測試
- 新增資料

```json
{
    "key":"Test",
    "value":"hello world"
}
```

```json
{
    "key":"Test Json",
    "value":
    {
        "Test":"hello world"
    }
}
```

![redis postman request post example.png](http://192.168.25.60:8000/files/file_storage/7902c73c.png)

- 取得資料

![redis postman request get example.png](http://192.168.25.60:8000/files/file_storage/b943dd06.png)

- 刪除資料

![redis postman request delete example.png](http://192.168.25.60:8000/files/file_storage/636910b7.png)

## GUI 查看 data
![redis gui key value example.png](http://192.168.25.60:8000/files/file_storage/e88e799f.png)

# Docker 部屬 Redis
## 在 Docker 起一個 container
-  在 docker-compose.yml 中新增 redis

```yaml
  redis:
    restart: always
    container_name: redis
    image: redis:alpine
    ports:
      - "6379:6379"
    volumes:
      - "/data/redis_data:/data"
```

- 將 redis_data 資料夾讀寫權限設定 777，原本會是 755 ( owner 可讀/寫/執行, 群組用戶及其他用戶可讀/執行)
```
cd /data/redis_data
```
```
sudo chmod 777 redis_data
```
## 操作 Redis
- 進入到 container 裡面
```
docker exec -it redis sh
```

- 使用 redis-cli
```
redis-cli
```

- 測試是否連線成功
```
ping
```

- 列出所有 key 
```
keys *
```

- 新增一個 key
```
setex mykey 60 "hello world"
```

- 取得一個 key
```
mget mykey
```

- 離開 container 
```
exit
```

