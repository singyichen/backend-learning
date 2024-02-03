---
title: Fastify Dependencies ( Others )
description: 其他套件
published: true
date: 2023-05-17T08:22:36.480Z
tags: fastify, framework
editor: markdown
dateCreated: 2022-12-27T02:55:16.572Z
---

# 其他套件介紹 ( Others Dependencies )
## user-agents
> User-Agents is a JavaScript package for generating random User Agents based on how frequently they're used in the wild

> [reference](https://www.npmjs.com/package/user-agents) 
{.is-info}

### Install
```shell
npm i user-agents --save
```

## ajv
> - JSON schema validator
> - The fastest JSON validator for Node.js and browser.

> [reference](https://www.npmjs.com/package/ajv) 
{.is-info}

### Install
```shell
npm i ajv --save
```

## moment
> A JavaScript date library for parsing, validating, manipulating, and formatting dates.

> [reference](https://www.npmjs.com/package/moment) 
{.is-info}

### Install
```shell
npm i moment --save
```

## moment-timezone
> IANA Time zone support for Moment.js

> [reference](https://www.npmjs.com/package/moment-timezone) 
{.is-info}

### Install
```shell
npm i moment-timezone --save
```

## ntp-time-sync
> Node.JS module to fetch the current time from NTP servers and returns offset information.

> [reference](https://www.npmjs.com/package/ntp-time-sync) 
{.is-info}

### Install
> [reference](https://github.com/buffcode/ntp-time-sync) 
{.is-info}

```shell
npm i ntp-time-sync --save
```
### Usage
- [ ] [台灣慣用公開時間伺服器(NTP.SERVER)](https://note.chiatse.com/tai-wan-guan-yong-gong-kai-shi-jian-si-fu-qi-ntp-server/)
- [ ] [中華電信研究所時間與頻率國家標準實驗室](https://www.stdtime.gov.tw/chinese/bulletin/NTP%20promo.txt)
- [ ] [NTP Server Taiwan 網路時間協定伺服器](https://vector.cool/ntp-server-taiwan/)
- [ ] [Building a more accurate time service at Facebook scale](https://engineering.fb.com/2020/03/18/production-engineering/ntp-service/)
- [ ] [網路時間標準NTP、PTP是什麼？Meta為何引進PTP？有助於元宇宙嗎？](https://www.bnext.com.tw/article/72776/is-ntp-retiring-meta-announced-that-it-will-introduce-a-new-network-time-standard-ptp-for-the-metaverse)
- [ ] [NTP Monitor Status](https://stats.uptimerobot.com/2GQWKtkkvE)

```js
const prismaClientService = require('./prismaClientService');
const errorLogger = require('../plugin/logger');
const { dayNamesZh } = require('../model/sharedObject');
const { NtpTimeSync } = require('ntp-time-sync');
const moment = require('moment');
/**
 * @description dateTime service
 */
class DateTimeService {
  /**
   * @description 從 NTP 取得台灣日期時間
   * @returns { Object } JSON include tw_date and tw_time and tw_day
   */
  async getTWDateTimeFromNtp() {
    try {
      const defaultOptions = {
        // list of NTP time servers, optionally including a port (defaults to 123)
        servers: [
          'tock.stdtime.gov.tw', // 台灣國家時間與頻率標準實驗室
          'watch.stdtime.gov.tw', // 台灣國家時間與頻率標準實驗室
          'time.stdtime.gov.tw', // 台灣國家時間與頻率標準實驗室
          'clock.stdtime.gov.tw', // 台灣國家時間與頻率標準實驗室
          'tick.stdtime.gov.tw', // 台灣國家時間與頻率標準實驗室
          'time.google.com', // Google
          'time1.facebook.com', // Facebook
          'time2.facebook.com', // Facebook
          'time.asia.apple.com', // Apple
          'time.windows.com', // Microsoft
        ],
        // required amount of valid samples in order to calculate the time
        sampleCount: 8,

        // amount of time in milliseconds to wait for a single NTP response
        replyTimeout: 3000,
      };
      const timeSync = NtpTimeSync.getInstance(defaultOptions);
      // request
      timeSync.getTime().then(function (result) {
        console.log('current system time', new Date());
        console.log('real time', result.now);
        console.log('offset in milliseconds', result.offset);
      });
      const result = await timeSync.getTime();
      const now = result.now;
      const date = moment(now).format('YYYY/MM/DD');
      const time = moment(now).format('HH:mm:ss');
      const day = new Date(now).getDay();
      return {
        tw_date: date,
        tw_time: time,
        tw_day: dayNamesZh[day],
      };
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
}

module.exports = DateTimeService;
```


