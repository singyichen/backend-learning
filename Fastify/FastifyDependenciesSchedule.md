---
title: Fastify Dependencies ( Schedule )
description: 排程套件
published: true
date: 2023-05-17T08:22:52.951Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-09-15T07:57:46.312Z
---

# 排程套件介紹 ( Schedule Dependencies )
## @fastify/schedule
> Fastify plugin for scheduling periodic jobs. Provides an instance of toad-scheduler on fastify instance.

> [reference](https://www.npmjs.com/package/@fastify/schedule)
{.is-info}

### Install
```shell
npm i @fastify/schedule --save
```
### Usage
> [reference](https://github.com/fastify/fastify-schedule)
{.is-info}


## toad-scheduler
> In-memory Node.js job scheduler that repeatedly executes given tasks within specified intervals of time

> [toad-scheduler reference](https://www.npmjs.com/package/toad-scheduler)
{.is-info}

> [croner reference](https://www.npmjs.com/package/croner)
{.is-info}

### Install
```shell
npm i toad-scheduler croner --save
```
### Usage
> [reference](https://github.com/kibertoad/toad-scheduler)
{.is-info}



- 在 src 底下新建一個資料夾 schedules 放置 task 檔案 
- 排程在定時內執行：使用 **SimpleIntervalJob**
```javascript
// src/schedules/job1.task.js
/**
 * @description task and job
 */
const { SimpleIntervalJob, Task } = require('toad-scheduler');

// task 任務
const task1 = new Task('simple task', () => {
  console.log('Task triggered 1');
});

// job 排程設定
const job1 = new SimpleIntervalJob(
  { seconds: 20, runImmediately: true },
  task1,
  'id_1',
);

module.exports = job1;
```

- 排程在固定時間執行：使用 **CronJob**

> [toad-scheduler#cron-support](https://github.com/kibertoad/toad-scheduler#cron-support)

> [排程工作（Cron Job）](http://kejyun.github.io/Laravel-4-Learning-Notes-Books/laravel-cronjob/laravel-cronjob-README.html)

> [Cron Format](http://www.nncron.ru/help/EN/working/cron-format.htm)

> [CronJob ](https://kubernetes.io/docs/concepts/workloads/controllers/cron-jobs/)

```javascript
// src/schedules/job1.task.js
/**
 * @description task and job
 */
const { Task, CronJob } = require('toad-scheduler');

// task 任務
const task1 = new Task('simple task', () => {
  console.log('Task triggered 1');
});

// job 排程設定
const job = new CronJob(
  {
    // 每週一早上 8 點
    cronExpression: '0 8 * * 1',
  },
  task,
  {
    preventOverrun: true,
  },
);

module.exports = job1;
```
- `cronExpression: '* * * * *'` 使用方式如下

```
* * * * * *
| | | | | | 
| | | | | +-- Year              (range: 1900-3000)
| | | | +---- Day of the Week   (range: 1-7, 1 standing for Monday)
| | | +------ Month of the Year (range: 1-12)
| | +-------- Day of the Month  (range: 1-31)
| +---------- Hour              (range: 0-23)
+------------ Minute            (range: 0-59)
```
```
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │                                   OR sun, mon, tue, wed, thu, fri, sat
# │ │ │ │ │
# * * * * *
```

- 分鐘 (0-59)
- 小時 (0-23)
- 每個月第幾天 (1-31)
- 月份 (1-12)
- 每週的第幾天 (0-6)
	- 0：星期日
	- 1：星期一
	- 2：星期二
	- 3：星期三
	- 4：星期四
	- 5：星期五
	- 6：星期六


- 在 plugin 底下新建一個 schedule.js
```javascript
// schedule.js
/**
 * @description scheduler plugin
 */
const fastifyPlugin = require('fastify-plugin');
const { fastifySchedulePlugin } = require('@fastify/schedule');
// import jobs
const job1 = require('../schedules/job1.task');

async function scheduleConnector(fastify) {
  fastify.register(fastifySchedulePlugin);
  // `fastify.scheduler` becomes available after initialization.
  // Therefore, you need to call `ready` method.
  fastify.ready().then(() => {
    // create and start jobs
    fastify.scheduler.addSimpleIntervalJob(job1);

    // check status of jobs
    console.log(fastify.scheduler.getById('id_1').getStatus());
  });
}

module.exports = fastifyPlugin(scheduleConnector);
```

- 在 app.js 中 import 並 register

```javascript
// app.js
// import plugin
const fastifySchedulePlugin = require('./src/plugin/schedule');
function build() {
  // init app
  const app = fastify();

  /**
   * @description plugin
   */
  // plugin
  // schedule
  app.register(fastifySchedulePlugin);  

  return app;
}

const app = build();
```
