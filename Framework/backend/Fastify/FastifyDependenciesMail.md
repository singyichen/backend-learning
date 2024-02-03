---
title: Fastify Dependencies ( Mail )
description: 郵件套件
published: true
date: 2023-08-18T08:20:44.780Z
tags: fastify, framework
editor: markdown
dateCreated: 2023-05-26T08:42:30.505Z
---

# 郵件套件介紹 ( Mail Dependencies )
## nodemailer
> - 郵件
> - Send emails from Node.js – easy as cake

> [reference](https://nodemailer.com/about/)
{.is-info}

### Install
```shell
npm i nodemailer --save
```
> [reference](https://www.npmjs.com/package/nodemailer)
{.is-info}
### Usage
> - [Node.js 系列學習日誌 #21 - 使用 nodemailer 套件透過 gmail 發送電子信箱](https://ithelp.ithome.com.tw/articles/10160766)
{.is-info}

- 在 `src/plugin` 中新增一個 `mailer.js`

```js
/**
 * @description mailer plugin
 */
const nodemailer = require('nodemailer');
const errorLogger = require('./logger');
const config = require('config');
/**
 * @description 發送電郵
 * @param { Array } toRecipient 收信者清單
 * @param { Array } ccRecipient 副本者清單
 * @param {string} subject 主旨
 * @param {string} html 嵌入 html 的內文
 * @returns { Object } JSON include from and to
 */
async function mailer(toRecipient, ccRecipient, subject, html) {
  try {
    // 發信物件
    const transporter = nodemailer.createTransport({
      host: config.get('mailer.email_service.email_service_ip'),
      port: config.get('mailer.email_service.email_service_port'),
      secure: false, // true for 465, false for other ports
    });
    // 信件參數
    const mailOptions = {
      from: config.get('mailer.email_service.email_service_url'), // 寄件者
      to: toRecipient, // 收信者清單
      cc: ccRecipient, // 副本者清單
      subject: subject + config.get('mailer.subject.subject'), // 主旨
      html: html, // 嵌入 html 的內文
    };
    // 發送信件
    return (await transporter.sendMail(mailOptions)).envelope;
  } catch (error) {
    errorLogger.error(error);
  }
}

module.exports = mailer;
```


## email-templates
> - 電子郵件模板
> - Create, preview (browser/iOS Simulator), and send custom email templates for Node.js. Made for Forward Email and Lad

> [reference](https://github.com/forwardemail/email-templates)
{.is-info}

### Install
```shell
npm i email-templates --save
```
> [reference](https://www.npmjs.com/package/email-templates)
{.is-info}
### Usage
- 在 `src/plugin` 中新增一個 `html.js`

```js
/**
 * @description email template plugin
 */
const Email = require('email-templates');
const path = require('path');
const errorLogger = require('./logger');
const email = new Email({
  views: {
    // 樣板路徑
    root: path.join(__dirname, '../views'),
  },
  juiceResources: {
    webResources: {
      // 取得 css
      relativeTo: path.join(__dirname, '../../public/stylesheets', 'style.css'),
      images: true,
    },
  },
});
/**
 * @description 生成電郵樣板
 * @param { String } pug 樣板檔名
 * @param { Array } data 資料
 * @returns { Object } html
 */
async function html(pug, data) {
  try {
    // 生成樣板
    return await email.render(pug, { data });
  } catch (error) {
    errorLogger.error(error);
  }
}
module.exports = html;
```

## pug
> - 一款 Node.js 的 HTML 樣版引擎
> - Pug is a high performance template engine heavily influenced by Haml and implemented with JavaScript for Node.js and browsers. 

> [reference](
https://pugjs.org/api/getting-started.html)
{.is-info}

### Install
```shell
npm i pug --save
```
> [reference](https://www.npmjs.com/package/pug)
{.is-info}
### Usage
> - [中文官方文件](https://pugjs.org/zh-cn/api/getting-started.html)
> - [精通 Pug 樣版語言（一）語法基礎篇](https://www.shirohana.me/blog/articles/2020-mastery-pug-template-engine/)
> - [認識 pug 範本語法](https://medium.com/unalai/%E8%AA%8D%E8%AD%98-pug-%E6%A8%A1%E6%9D%BF%E8%AA%9E%E6%B3%95-74adeee56468)
> - [HTML TO PUG - Online Converter](https://html-to-pug.com/)
{.is-info}

- 在 `src/views` 中新增一個 `email.pug`

```pug
//- 請假通知
doctype html
html
    head
        link(rel="stylesheet", type="text/css", href="../../stylesheets/style.css")
    body
        div.cu_font Dear all,
        br
        each item in data  ? data : []
            - const format_part_type =  item.part_type==='一日'? `休假${item.part_type}` : item.part_type   
            div.cu_font #{item.user_id}/#{item.user_name} #{item.holiday} #{format_part_type}
        br
        div.cu_font 請各位知悉
        br
        div.cu_font (此為系統自動發送訊息，詳細資訊請以紙本假單為準。)  
```

- 在 `public/stylesheets` 中新增一個 `style.css`

```css
.cu_font {
    font-size:12.0pt;
    font-family:"微軟正黑體",
    sans-serif;
}
```
# 郵件功能使用

- 以下用寄信排程為例
- 因郵件相關設定可能會時常需要進行修改，故使用 config 方式進行管理
- 在 `config` 中新增兩個設定檔 
	- `development.json`：部屬到 docker 時使用
  - `test.json`：unit test 時使用

```json
// development.json
{
    "mailer": {
        "email_service":{
            "email_service_ip":"25",
            "email_service_port":"192.168.25.27",
            "email_service_url":"mis@hokia.com.tw"
        },
        "schedule": {
            "schedule": "30 7 * * *"
        },
        "recipient": {
            "mis": [
                "SY-MIS-MG@hokia.com.tw"
            ],
            "hr": [
                "fion.chien@hokia.com.tw",
                "julia.chiao@hokia.com.tw"
            ],
            "backend": [
                "mandy.chen@hokia.com.tw"
            ]
        }
    }
}
```

```json
// test.json
{
    "mailer": {
        "email_service":{
            "email_service_ip":"25",
            "email_service_port":"192.168.25.27",
            "email_service_url":"mis@hokia.com.tw"
        },
        "schedule": {
            "schedule": "30 7 * * *"
        },
        "recipient": {
            "mis": [
                "mandy.chen@hokia.com.tw"
            ],
            "hr": [
                "mandy.chen@hokia.com.tw"
            ],
            "backend": [
                "mandy.chen@hokia.com.tw"
            ]
        }
    }
}
```

- 寄信排程

```js
/**
 * @description task and job
 */
const { Task, CronJob } = require('toad-scheduler');
const logger = require('../plugin/logger');
const moment = require('moment');
const HolidayAgentService = require('../modules/admin/holidayAgent/service/holidayAgentService');
const holidayAgentService = new HolidayAgentService();
const mailer = require('../plugin/mailer');
const config = require('config');
const { dayNamesZh } = require('../utils/sharedObject');
const html = require('../plugin/html');
// task 任務
const task = new Task('Send Email task', async () => {
  try {
    const taskStart = '寄信排程開始執行';
    const taskEnd = '寄信排程結束執行';
    const findAllHolidayAgentData = (await holidayAgentService.findAll()).data;
    if (findAllHolidayAgentData.length !== 0) {
      const twDate = moment(new Date()).format('YYYY/MM/DD');
      const twDay = new Date(twDate).getDay();
      // 檢核為當天資料且不為公出
      const mailerData = findAllHolidayAgentData.filter(
        (element) =>
          element.holiday === twDate + dayNamesZh[twDay] &&
          element.part_type !== '公出',
      );
      mailerData.forEach((element) => {
        element.holiday =
          moment(new Date()).format('YYYY-MM-DD') + dayNamesZh[twDay];
      });
      const mailerDataLength = mailerData.length;
      if (mailerDataLength !== 0) {
        console.log(taskStart);
        logger.info({
          schedule: {
            task_start: `${taskStart}共${mailerDataLength}筆資料`,
          },
        });
        const subject = '[請假通知] 今日資訊部請假人員列表';
        const pug = 'email.pug';
        const template = await html(pug, mailerData);
        const mailerInfo = await mailer(
          config.get('mailer.recipient.hr'),
          config.get('mailer.recipient.mis'),
          subject,
          template,
        );
        console.log(taskEnd);
        logger.info({
          schedule: {
            task_end: `${taskEnd}共${mailerDataLength}筆資料，from：${mailerInfo.from}`,
          },
        });
      }
    }
  } catch (error) {
    logger.error(error);
    console.log(error);
  }
});

// job 排程設定
const job = new CronJob(
  {
    // 每天 07:30
    cronExpression: config.get('mailer.schedule.schedule'),
  },
  task,
  {
    preventOverrun: true,
  },
);

module.exports = job;

```

