---
title: API Design
description: 
published: true
date: 2023-08-30T02:46:55.661Z
tags: api
editor: markdown
dateCreated: 2023-07-07T00:43:03.191Z
---

# API Design
- [ ] [EP56: System Design Blueprint: The Ultimate Guide](https://blog.bytebytego.com/p/ep56-system-design-blueprint-the?utm_source=profile&utm_medium=reader2)
- [ ] [Mastering the Art of API Design](https://blog.bytebytego.com/p/api-design?utm_source=profile&utm_medium=reader2)
- [ ] [API Design Practice](https://medium.com/api-center/api-design-practice-7fce69e6336c)
- [ ] [API redesign: shopping cart and Stripe payment](https://blog.bytebytego.com/p/api-redesign-shopping-cart-and-stripe?utm_source=profile&utm_medium=reader2)
- [ ] [Why Your Backend in Node.JS Needs an API Layer and How to Build It](https://semaphoreci.medium.com/why-your-backend-in-node-js-needs-an-api-layer-and-how-to-build-it-3e4e4c262a2a)
- [ ] [How to improve API performance](https://blog.bytebytego.com/p/ep64-how-to-improve-api-performance?utm_source=profile&utm_medium=reader2)
- [ ] [API Lifecycle Explained API](https://medium.com/@shubhadeepchat/api-lifecycle-explained-8c3d1aca7316)
- [ ] [10 Must-Follow Best Practices for API Creation](https://sandydev.medium.com/10-must-follow-best-practices-for-api-creation-3434cd5dc098)
- [ ] [The Complete Guide to Grokking the API Design Interview](https://grokkingtechinterview.com/the-complete-guide-to-grokking-the-api-design-interview-9219c0bec786)
- [ ] [Demystifying the Unusual Evolution of the Netflix API Architecture](https://www.youtube.com/watch?v=Uu32ggF-DWg&ab_channel=ByteByteGo&loop=0)
- [ ] [How to handle multiple API requests in your NodeJS Application](https://medium.com/@abhinavcv007/how-to-handle-multiple-api-requests-in-your-nodejs-application-cfa298e11b28)
- [ ] [How to Build World-Class Web APIs: 3 Lessons from the Postman Team](https://learningdaily.dev/how-to-build-world-class-web-apis-3-lessons-from-the-postman-team-d76de34011db)
- [ ] [Crafting Effective API Design with Node.js and Express](https://smit90.medium.com/crafting-effective-api-design-with-node-js-and-express-65c2bb16845c)
- [ ] [API Architecture - Synchronous, Asynchronous, and Parallel calls](https://abdulrwahab.medium.com/api-architecture-synchronous-asynchronous-and-parallel-calls-e85b9c08fe51)



![api slogan.png](http://192.168.25.60:8000/files/file_storage/dbe158de.png)

## Define Clear Endpoints and Resources 
> 在設計 API 時，清晰度是關鍵。明確定義您的 API 端點，使其直觀且易於理解。每個終結點應遵循一致的命名約定表示特定的資源或操作。

![api design endpoints and resources.png](http://192.168.25.60:8000/files/file_storage/5b5efb17.png)

```js
// GET all users
app.get('/users', (req, res) => {
  // ...retrieve and send user data
});

// POST to create a new user
app.post('/users', (req, res) => {
  const newUser = req.body;
  // ...create user and send response
});
```

## Use HTTP Methods Effectively 
> 根據其預期目的使用 HTTP 方法。

- GET ：從伺服器檢索資料。
- POST ：將資料傳送到伺服器以建立新資源。
- PUT ：更新伺服器上的現有資料。
- DELETE ：從伺服器中刪除資料。

```js
// Using HTTP methods effectively
app.get('/posts', (req, res) => {
  // ...retrieve and send post data
});

app.post('/posts', (req, res) => {
  const newPost = req.body;
  // ...create post and send response
});
```

## Embrace RESTful Principles 
> 遵循 RESTful 原則以實現一致性。利用適當的 HTTP 狀態程式碼來指示請求結果

- 200 OK ：成功的 GET 請求。
- 201 Created ：開機自檢請求成功。
- 204 No Content ：刪除請求成功。
- 400 Bad Request ：請求資料無效。
- 404 Not Found ：找不到資源。

```js
// Embracing RESTful principles
app.get('/products/:id', (req, res) => {
  const productId = req.params.id;
  // ...retrieve and send product data
});
```

## Validate Input Data
> 驗證輸入資料的資料完整性和安全性。快速中介軟體庫，如 express-validator 幫助清理和驗證傳入資料。

![api design validate input data.png](http://192.168.25.60:8000/files/file_storage/1235cc8d.png)

```js
// Using express-validator for input validation
const { body, validationResult } = require('express-validator');

app.post('/create', [
  body('username').trim().isLength({ min: 3 }),
  body('email').isEmail().normalizeEmail(),
], (req, res) => {
  const errors = validationResult(req);
  if (!errors.isEmpty()) {
    return res.status(400).json({ errors: errors.array() });
  }
  // ...create user and send response
});
```
## Version Your APIs
> 隨著 API 的發展，保持與版本化終結點的向後相容性

![api design rest api versioning.png](http://192.168.25.60:8000/files/file_storage/23edd4e8.png)

```js
// Versioning your APIs
app.get('/v1/users', (req, res) => {
  // ...retrieve and send user data (version 1)
});

app.get('/v2/users', (req, res) => {
  // ...retrieve and send updated user data (version 2)
});
```

## Folder Structure and Design Patterns
> 使用邏輯資料夾結構和設計模式（如 MVC（模型-檢視-控製器））來組織項目，以確保模組化和可伸縮性。

- 資料夾結構示例

```
- controllers/
  - userController.js
  - postController.js
- models/
  - User.js
  - Post.js
- routes/
  - userRoutes.js
  - postRoutes.js
- app.js
```
## Error Handling and Middleware
> 使用中介軟體實現集中式錯誤處理。建立自訂錯誤類並使用適當的狀態程式碼。

![api design error handling and middleware.png](http://192.168.25.60:8000/files/file_storage/6ed7239c.png)

- 錯誤處理中介軟體示例

```js
// Error handling middleware
app.use((err, req, res, next) => {
  console.error(err.stack);
  res.status(500).json({ message: 'Something went wrong!' });
});
```

## Implementing Logging Services
> 通過實施日誌記錄服務來增強 API 的監視和偵錯功能。利用庫（如 winston 或 morgan ）記錄重要事件、錯誤和請求詳細資訊。

- 用於 morgan 請求日誌記錄的示例

```js
const morgan = require('morgan');
app.use(morgan('combined')); // Logs detailed request information
```

## Handling Different Environments
> 通過使用組態檔案、環境變數和日誌記錄等級，使 API 適應不同的環境（開發、暫存、生產）。

```js
// Handling different environments using environment variables
const environment = process.env.NODE_ENV || 'development';

if (environment === 'development') {
  // Configure development-specific settings
} else if (environment === 'production') {
  // Configure production-specific settings
}
```

## Automated Testing for Reliability
> 將自動化測試納入您的 API 開發流程。編寫單元測試、整合測試和端到端測試，以驗證 API 在各個等級的功能。

```js
// Example of unit test using a testing library like Jest
test('GET /users should return a list of users', async () => {
  const response = await request(app).get('/users');
  expect(response.status).toBe(200);
  expect(response.body).toEqual(expect.arrayContaining([
    { id: 1, name: 'John' },
    { id: 2, name: 'Jane' },
  ]));
});
```

## Continuous Integration and Continuous Deployment (CI/CD)
> 將測試套件整合到 CI/CD 管道中。自動執行測試、建構 API 並將其部署到不同環境的過程，確保一致性並減少引入錯誤的機會。

![api design CICD.png](http://192.168.25.60:8000/files/file_storage/a3f5f6fe.png)

```
# Example configuration for a CI/CD pipeline
stages:
  - test
  - build
  - deploy

test:
  stage: test
  script:
    - npm install
    - npm test

build:
  stage: build
  script:
    - npm install
    - npm run build

deploy:
  stage: deploy
  script:
    - npm install
    - npm run deploy
```

## Monitoring and Reporting
> 實施監控工具來跟蹤 API 的性能、正常執行階段間和響應時間。使用 New Relic 或 Prometheus 等服務即時瞭解 API 的行為。

![api design monitoring and reporting.png](http://192.168.25.60:8000/files/file_storage/23106d9a.png)

```js
// Example of setting up New Relic monitoring
const newrelic = require('newrelic');
```

## Testing-Driven Development (TDD) Approach
> 考慮採用測試驅動的開發方法。在實現功能之前編寫測試，以確保 API 從一開始就按預期運行。

![api design TDD.png](http://192.168.25.60:8000/files/file_storage/87d42f99.png)

```js
// Example of a TDD approach
test('POST /users should create a new user', async () => {
  const response = await request(app)
    .post('/users')
    .send({ name: 'Alice' });
  expect(response.status).toBe(201);
  expect(response.body.name).toBe('Alice');
});
```

![producer api lifecycle map.png](http://192.168.25.60:8000/files/file_storage/3091066a.png)

![design effective and safe APIs.png](http://192.168.25.60:8000/files/file_storage/63a3df66.png)

![Code First vs API First Development.png](http://192.168.25.60:8000/files/file_storage/fc7c2199.png)

![How to improve API performance.png](http://192.168.25.60:8000/files/file_storage/c6f0c599.png)

![API-Driven Application Infrastructure Blueprint.gif](http://192.168.25.60:8000/files/file_storage/7759ab93.gif)

![Top 16 API Security Practices.gif](http://192.168.25.60:8000/files/file_storage/9458aead.gif)




