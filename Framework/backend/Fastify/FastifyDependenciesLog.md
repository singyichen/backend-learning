---
title: Fastify Dependencies ( Log )
description: 日誌套件
published: true
date: 2023-06-19T00:08:12.878Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-08-25T01:26:35.600Z
---

# 日誌套件介紹 ( Log Dependencies )
## pino
> - 日誌套件
> - Very low overhead Node.js logger.

> [reference](https://github.com/pinojs/pino)
{.is-info}
### Install
> [reference](https://www.npmjs.com/package/pino)
{.is-info}
```shell
npm install pino --save
```
## rotating-file-stream
> - 建立可自動輪轉的日誌文件流套件
> - Creates a stream.Writable to a file which is rotated. Rotation behaviour can be deeply customized; optionally, classical UNIX logrotate behaviour can be used.

> [reference](https://github.com/iccicci/rotating-file-stream)
{.is-info}
### Install
> [reference](https://www.npmjs.com/package/rotating-file-stream)
{.is-info}
```shell
npm install rotating-file-stream --save
```

### Usage

- logger 插件
```javascript
// plugin/logger.js
/**
 * @description logger plugin
 */

const pino = require('pino');
const path = require('path');
const fs = require('fs');
const moment = require('moment');
const rotatingFileStream = require('rotating-file-stream');

// 設定日誌分類
const levels = {
  info: 30,
  error: 50,
};
// 設定日誌資料夾路徑
const logDirectory = path.join(__dirname, '../../logs');
// 確認日誌資料夾是否存在
fs.existsSync(logDirectory) || fs.mkdirSync(logDirectory);
const stream = Object.keys(levels).map((level) => {
  return {
    level: level,
    stream: rotatingFileStream.createStream(`${level}.log`, {
      interval: '1d', // 設置循環寫入的時間間隔
      path: `${logDirectory}/${level}`, // 設置日誌文件的目錄路徑
      maxFiles: 30, // 保留最近 30 天的日誌文件
    }),
  };
});
const logger = pino(
  {
    level: 'info', // must be the lowest level of all streams
    customLevels: levels,
    useOnlyCustomLevels: true,
    // 日誌中的日期時間格式化
    timestamp: () =>
      `,"time":"${moment(new Date()).format('YYYY-MM-DD HH:mm:ss')}"`,
    formatters: {
      // 將 level 從數字轉成文字
      level: (label) => {
        return { level: label };
      },
    },
    // 不記錄 pid，hostname
    base: undefined,
    // console log 顯示設定，目前無法跟寫入 log 同時運作
    // transport: {
    //   target: 'pino-pretty',
    //   options: {
    //     colorize: true,
    //     translateTime: 'SYS:yyyy-mm-dd hh:MM:ss', // 格式化日期時間
    //     ignore: 'pid',
    //   },
    // },
  },
  pino.multistream(stream, {
    levels,
    dedupe: true, // Set this to true to send logs only to the stream with the higher level
  }),
);

module.exports = logger;
```

- 會在專案根目錄自動建立放置 log 的資料夾 logs 
1. logs/error
2. logs/info

![fastify project add logs directory.png](http://192.168.25.60:8000/files/file_storage/f7626d82.png)

- 於 app.js 監控 onResponse，將 http 類型 log 寫入 info 日誌

```javascript
// app.js
// import plugin
const fastifyLoggerPlugin = require('./src/plugin/logger');
// init app
  const app = fastify({
    // 使用 logger plugin
    logger: fastifyLoggerPlugin,    
  });
  // 將 http 類型 log 寫入 info 日誌
  app.addHook('onResponse', (request, reply, done) => {
    fastifyLoggerPlugin.info({
      request: {
        method: request.method,
        url: request.url,
        user_agent: request.headers['user-agent'],
        hostname: request.hostname,
        remoteAddress: request.ip,
        remotePort: request.socket.remotePort,
      },
      reply: {
        statusCode: reply.statusCode,
        statusMessage: reply.raw.statusMessage,
      },
    });
    done();
  });
```

- 使用方式

```javascript
// import
const logger = require('../plugin/logger');

// 寫入 info log
logger.info({
  schedule: {
    task_end: `${taskEnd}共${mailerDataLength}筆資料，from：${mailerInfo.from}`,
  },
});

// 寫入 error log
logger.error(error);
```

- 會依照日誌的分類及日期生成檔案，並寫入 log

![fastify project logs.png](http://192.168.25.60:8000/files/file_storage/6083c729.png)

- 於 docker 部屬時，需將建立 logs 的指令加入到 dockerfile 中

```dockerfile
# 建立 log 資料夾
RUN mkdir -p /src/logs/error && mkdir -p /src/logs/info
```

```dockerfile
# Stage 1: Decide base image
# 決定基底映像檔
FROM node:14

# Stage 2: Set Timezone
# 設定容器時區
ENV TZ=Asia/Taipei
RUN cp /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Stage 3: Set the working directory
# 設置目前工作目錄
WORKDIR /src

# Stage 4: Install requirements
# 複製檔案至映像檔內
COPY . .
# 安裝 Dependencies
RUN npm install && npm cache clean --force
# 生成 Prisma Client
RUN npx prisma generate --schema=./src/prisma/schema.prisma
# 建立 log 資料夾
RUN mkdir -p /src/logs/error && mkdir -p /src/logs/info

# Stage 5: Set container listen Port
# 設定 Docker 要開放的 Port
EXPOSE 10001

# Stage 6: Run app
# 啟動 Container 時要執行的指令
CMD [ "node", "app" ]
```

### 於 docker 中查看 log
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

## pino-pretty
> This module provides a basic ndjson formatter to be used in development.

### Install
> [reference](https://www.npmjs.com/package/pino-pretty)
{.is-info}
```shell
npm install pino-pretty --save
```
### Usage








