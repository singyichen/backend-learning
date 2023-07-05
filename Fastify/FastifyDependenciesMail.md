---
title: Fastify Dependencies ( Mail )
description: 測試套件
published: true
date: 2023-06-01T07:06:02.747Z
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
      host: '192.168.25.27',
      port: '25',
      secure: false, // true for 465, false for other ports
    });
    // 信件參數
    const mailOptions = {
      from: 'mis@hokia.com.tw', // 寄件者
      to: toRecipient, // 收信者清單
      cc: ccRecipient, // 副本者清單
      subject: subject, // 主旨
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



