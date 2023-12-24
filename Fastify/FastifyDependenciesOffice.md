---
title: Fastify Dependencies ( Office )
description: 資料處理軟體套件
published: true
date: 2023-12-18T08:57:40.527Z
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
- 新增 `attachFieldsToBody` 設定：用來決定是否將字段附加到請求的主體中。當設置為 `true` 時，表示將多部分表單數據的字段附加到 `request.body` 中。這樣可以直接從 `body` 中取得字段值，而不需要從 `request.parts.fields` 中訪問它們。

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
    attachFieldsToBody: true, // determine whether or not to attach fields to the request body
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

- controller：取得資料使用以下寫法
- 上傳檔案：`request.body.file`
- 一般參數：`request.body.user_id.value`

```js
// controller.js
  /**
   * @description 新增多筆獎品
   * @param { Object } request 請求
   * @returns { Object } json
   */
  async uploads(request) {
    try {
      const data = request.body.file;
      const user_id = request.body.user_id.value;
      return await this.prizeService.uploads(data, user_id);
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
```

- router 設定及資料驗證格式
- 資料驗證格式：上傳檔案設定為 `binary`，一般參數也可以加入一起設定

```js
/**
 * @description uploadsPrize 資料格式驗證
 */
const uploadsPrizeSchema = {
  type: 'object',
  properties: {
    file: {
      format: 'binary',
    },
    user_id: {
      minLength: 0,
    },
  },
  required: ['file', 'user_id'],
};
module.exports = uploadsPrizeSchema;
```

- 路由 schema 的部份設定格式為：`multipart/form-data`

```js
// router.js
  /**
   * @description 新增多筆獎品路由 ( POST api/admin/prize/uploads )
   */
  fastify.post('/uploads', {
    // uploadsPrize 資料格式驗證
    schema: {
      consumes: ['multipart/form-data'],
      body: uploadsPrizeSchema,
    },
    // JWT 驗證
    preHandler: fastify.auth([fastify.verifyJWT]),
    handler: async function (request, reply) {
      try {
        const res = await prizeController.uploads(request);
        reply.send(res);
      } catch (error) {
        errorLogger.error(error);
        reply.send({ status: -1, message: error.message });
        console.log(error);
      }
    },
  });
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
   * @description 取得單筆上傳檔案資料
   * @param { Object } data excel 檔案
   * @returns { Object } JSON
   */
  async uploads(data) {
    try {
      if (!data.filename.endsWith('.xls') && !data.filename.endsWith('.xlsx')) {
        const twDateTimeData = (await dateTimeApi()).data;
        return new ErrorResponse(
          uploadsCreateFailedFileIsNotExcelInfo,
          twDateTimeData,
        );
      }
      const excelData = await this.getExcelData(data);
      if (excelData.statusCode !== StatusCodes.OK) {
        return excelData;
      }
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
      // 檔案中完全沒資料或僅有表頭
      if (jsonData.every((arr) => arr.length === 0) || jsonData.length === 2) {
        const twDateTimeData = (await dateTimeApi()).data;
        return new ErrorResponse(
          uploadsCreateFailedExcelFileNoDataInfo,
          twDateTimeData,
        );
      }
      const [title, header, ...dataRows] = jsonData.filter(
        (item) => item.length > 0,
      );
      const excelData = {
        title: title[0],
        header,
        data: dataRows,
      };
      return new SuccessResponse(StatusCodes.OK, excelData);
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
      const buffer = data._buf;
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
  // 以下函式可以不需要使用到
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

- service 處理業務邏輯

```js
// service.js
  /**
   * @description 新增多筆獎品
   * @param { Object } excel excel 檔案
   * @param { string } createdId 建立者 ID
   * @returns { Object } JSON
   */
  async uploads(excel, createdId) {
    try {
      const data = await this.uploadsService.uploads(excel);
      if (data.statusCode === StatusCodes.OK) {
        const columnMap = {
          獎品資料: 'prize',
          獎品類別: 'prize_category',
          獎品名稱: 'prize_name',
          獎品數量: 'number',
          抽獎別: 'raffle',
          // 建立者員編: 'created_id',
        };
        const excelData = data.data;
        const transformedData = {
          title: columnMap[excelData.title],
          header: excelData.header.map((key) => columnMap[key]),
          data: excelData.data.map((row) => {
            const obj = {};
            excelData.header.forEach((key, index) => {
              obj[columnMap[key]] = row[index];
            });
            return obj;
          }),
        };
        // 先刪除資料
        const verifyDeleteData = await this.deleteAll();
        if (verifyDeleteData.statusCode !== StatusCodes.OK) {
          return verifyDeleteData;
        }
        const addData = transformedData.data.map((item) => {
          return {
            ...item, // 保留其他欄位的值
            raffle: item.raffle.toString(),
            created_id: createdId,
          };
        });
        // 再新增資料
        await prismaClientService.prisma.prize.createMany({
          data: addData,
        });
        const findAllPrizeData = await this.findAll();
        return new SuccessResponse(StatusCodes.OK, findAllPrizeData.data);
      }
      return data;
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  }
```

## fast-xml-parser
> - 一個用於 Node.js 的輕量級 JavaScript 套件，可以解析、驗證和建構 XML 檔案。
> - Validate XML, Parse XML to JS Object, or Build XML from JS Object without C/C++ based libraries and no callback.

### Install
```shell
npm install fast-xml-parser --save
```
> [reference](https://www.npmjs.com/package/fast-xml-parser) 
{.is-info}

### Usage
> [reference](https://github.com/NaturalIntelligence/fast-xml-parser)
{.is-info}






## xml-formatter
> - 一個用於格式化 XML 的 JavaScript 套件
> - Converts XML into a human readable format (pretty print).

### Install
```shell
npm install xml-formatter --save
```
> [reference](https://www.npmjs.com/package/xml-formatter) 
{.is-info}

### Usage
> [reference](https://github.com/chrisbottin/xml-formatter)
{.is-info}





## html-entities
> - 一個可以用於編碼和解碼 HTML 實體的 JavaScript 套件，以其速度和輕量化著稱
> - Fastest HTML entities library.

### Install
```shell
npm install html-entities --save
```
> [reference](https://www.npmjs.com/package/html-entities) 
{.is-info}

### Usage
> [reference](https://github.com/mdevils/html-entities)
{.is-info}

