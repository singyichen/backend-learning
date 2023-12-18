---
title: Fastify Dependencies ( Office )
description: 資料處理軟體套件
published: true
date: 2023-12-15T08:27:05.042Z
tags: fastify, framework
editor: markdown
dateCreated: 2023-08-24T00:02:23.252Z
---

# 資料處理軟體套件介紹 ( Office Dependencies )
- [ ] [Efficient Large Excel to JSON Conversion in Node.js with Low Memory Usage](https://mobileappcircular.com/efficient-large-excel-to-json-conversion-in-node-js-with-low-memory-usage-8132ed237345)
- [ ] [[Fastify] Day23 - Upload File](https://ithelp.ithome.com.tw/articles/10305976)
- [ ] [Fastify - How to get non-file fields using fastify-multipart](https://stackoverflow.com/questions/70477141/fastify-how-to-get-non-file-fields-using-fastify-multipart)

## @fastify/multipart
> Fastify plugin to parse the multipart content-type.

### Install
```shell
npm install @fastify/multipart --save
```
> [reference](https://www.npmjs.com/package/@fastify/multipart) 
{.is-info}

### Usage
> [reference](https://github.com/fastify/fastify-multipart)
{.is-info}

- 如果沒有擴充對於 `multipart` 的支援，直接對伺服器上傳檔案的話，在 Fastify 生命週期中，進到 router 前，就會被 Fastify 回傳 `415` 的錯誤。

```json
{
  "statusCode":415,
  "code":"FST_ERR_CTP_INVALID_MEDIA_TYPE",
  "error":"Unsupported Media Type",
  "message":"Unsupported Media Type: multipart/form-data; boundary=--------------------------626027659265615707518419"
}
```

- 在 `src/plugin` 中新增一個 `multipart.js`

```js
/**
 * @description multipart plugin
 */

const fastifyPlugin = require('fastify-plugin');
const fastifyMultipart = require('@fastify/multipart');

async function multipartConnector(fastify, opts, done) {
  const multipartOptions = {
    limits: {
      fieldNameSize: 100, // Max field name size in bytes
      fieldSize: 100, // Max field value size in bytes
      fields: 10, // Max number of non-file fields
      fileSize: 1000000, // For multipart forms, the max file size in bytes
      files: 1, // Max number of file fields
      headerPairs: 2000, // Max number of header key=>value pairs
    },
  };
  fastify.register(fastifyMultipart, multipartOptions);

  done();
}

module.exports = fastifyPlugin(multipartConnector);
```

- 在 `app.js` 中 import 並 register
- 位置需在 **router** 之前

```javascript
// app.js
// import plugin
const fastifyMultipartPlugin = require('./src/plugin/multipart');
const fastifyRouterPlugin = require('./src/plugin/router');

function build() {
  // init app
  const app = fastify();

  /**
   * @description plugin
   */
  // plugin
  // multipart (需在 router 之前)
  app.register(fastifyMultipartPlugin);
  // router
  app.register(fastifyRouterPlugin);

  return app;
}

const app = build();
```

- Postman 測試檔案上傳功能
- 從 Body 選取傳送資料為 `form-data`，`key` 選擇 `file`，`value` 選擇要上傳的檔案

![Eboard System Postman file uploads.png](http://192.168.25.60:8000/files/file_storage/9e2b764f.png)

## xlsx
> xlsx 是一個用於處理 Excel 檔案的 JavaScript 套件。它可以讓您讀取、寫入和修改 Excel 檔案，並提供了豐富的功能和方法來處理 Excel 資料

### Install
```shell
npm install xlsx --save
```
> [reference](https://www.npmjs.com/package/xlsx) 
{.is-info}

### Usage
> [reference](https://github.com/SheetJS/sheetjs)
{.is-info}

- 解析的 `excel` 範例檔：[IT010盤點報表(保管人00000資訊部)](http://192.168.25.60:8000/files/file_storage/6cf6a7c4.xlsx)
- 在 `uploadsService.js` 中的新增相關函式

```js
const errorLogger = require('../../../../plugin/logger');
const xlsx = require('xlsx');
const {
  uploadsCreateFailedExcelFileNoDataInfo,
  uploadsCreateFailedFileIsNotExcelInfo,
} = require('../../../../utils/errorInfo');
const dateTimeApi = require('../../../../api/dateTimeApi');
const { ErrorResponse } = require('../../../../base/base.response');
/**
 * @description uploads service
 */
class UploadsService {
  /**
   * @description 新增單筆檔案上傳
   * @param { Object } data excel 檔案
   * @returns { Object } JSON
   */
  async create(data) {
    try {
      if (!data.filename.endsWith('.xls') && !data.filename.endsWith('.xlsx')) {
        const twDateTimeData = (await dateTimeApi()).data;
        return new ErrorResponse(
          uploadsCreateFailedFileIsNotExcelInfo,
          twDateTimeData,
        );
      }
      const excelData = await this.getExcelData(data);
      // TODO：insert data into db
      return excelData;
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
  /**
   * @description 取得 excel 資料
   * @param { Object } data excel 檔案
   * @returns { Object } JSON
   */
  async getExcelData(data) {
    try {
      const workbook = await this.getWorkbook(data);
      const sheetName = workbook.SheetNames[0];
      const worksheet = workbook.Sheets[sheetName];
      // header 選項可以控制生成的 JSON 中是否包含標題行的資料(0,1,false,其他數字)
      const jsonData = xlsx.utils.sheet_to_json(worksheet, { header: 1 });
      if (jsonData.length === 0) {
        const twDateTimeData = (await dateTimeApi()).data;
        return new ErrorResponse(
          uploadsCreateFailedExcelFileNoDataInfo,
          twDateTimeData,
        );
      }
      const [title, header, ...dataRows] = jsonData.filter(
        (item) => item.length > 0,
      );
      return {
        title: title[0],
        header,
        data: dataRows,
      };
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
  /**
   * @description 取得 workbook
   * @param { Object } data excel 檔案
   * @returns { Object } workbook
   */
  async getWorkbook(data) {
    try {
      const buffer = await this.readFile(data.file);
      const bookType = data.filename.endsWith('.xls') ? 'biff8' : 'xlsx';
      return xlsx.read(buffer, {
        type: 'buffer',
        bookType,
      });
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
  /**
   * @description 讀取檔案
   * @param { Object } fileStream 檔案
   * @returns { Object } buffer
   */
  async readFile(fileStream) {
    try {
      const chunks = [];
      for await (const chunk of fileStream) {
        chunks.push(chunk);
      }
      return Buffer.concat(chunks);
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
}

module.exports = UploadsService;
```


