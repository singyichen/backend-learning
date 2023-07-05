---
title: JWT
description: 
published: true
date: 2023-05-24T01:16:24.381Z
tags: jwt, 驗證, authentication
editor: markdown
dateCreated: 2022-06-01T07:27:37.054Z
---

# JWT ( Json Web Token )
- [ ] [jwt.io](https://jwt.io/)
- [ ] [[note] JWT](https://pjchender.dev/webdev/note-jwt)
- [ ] [以 JSON Web Token 替代傳統 Token](https://yami.io/jwt/)
- [ ] [Express 簡易 passport + jwt 認證](https://medium.com/karinsu/express-%E7%B0%A1%E6%98%93-passport-jwt-%E8%AA%8D%E8%AD%89-9472e35b5d43)
- [ ] [Introduction to JSON Web Tokens](https://jwt.io/introduction/)
- [ ] [Cookie、Session、Token、JWT 看一篇就夠了](https://blog.51cto.com/u_15300443/3167467)
- [ ] [Day 19 - 二週目 - 帳密認證與JWT (JSON Web Token)傳遞](https://ithelp.ithome.com.tw/articles/10203292)
- [ ] [傻傻分不清之 Cookie、Session、Token、JWT](https://juejin.cn/post/6844904034181070861)
- [ ] [JWT 的運作原理是什麼？](https://www.explainthis.io/zh-hant/interview-guides/backend/jwt)
- [ ] [Password, Session, Cookie, Token, JWT, SSO, OAuth - Authentication Explained - Part 2](https://blog.bytebytego.com/p/password-session-cookie-token-jwt-ec1)
- [ ] [打造一個 GraphQL API Server 應用：部落格社交軟體 - 2 (Authentication & Authorization)](https://ithelp.ithome.com.tw/articles/10205426)
- [ ] [使用 JWT 來驗證請求者身份](https://ken00535.medium.com/%E4%BD%BF%E7%94%A8-jwt-%E4%BE%86%E9%A9%97%E8%AD%89%E8%AB%8B%E6%B1%82%E8%80%85%E8%BA%AB%E5%88%86-285c74f4dc5c)
- [ ] [更好的選擇？用 JWT 取代 Session 的風險](https://ken00535.medium.com/jwt-vs-session-44175f2f2656)
- [ ] [JSON web token | JWT](https://www.geeksforgeeks.org/json-web-token-jwt/?ref=gcse)
- [ ] [JWT Authentication With Refresh Tokens](https://www.geeksforgeeks.org/jwt-authentication-with-refresh-tokens/?ref=gcse)
- [ ] [How to implement JWT authentication in Express.js app ?](https://www.geeksforgeeks.org/how-to-implement-jwt-authentication-in-express-js-app/?ref=gcse)
- [ ] [How to use JSON web tokens with Node.js ?](https://www.geeksforgeeks.org/how-to-use-json-web-tokens-with-node-js/?ref=gcse)
- [ ] [Secure User Authentication in Node.js using JSON Web Tokens (JWT)](https://singh-sandeep.medium.com/secure-user-authentication-in-node-js-using-json-web-tokens-jwt-271cd0599df)
- [ ] [How to Use JWT and Node.js for Better App Security](https://www.toptal.com/json/jwt-nodejs-security)

# 源起

![JWT slogan.png](http://192.168.25.60:8000/files/file_storage/9b0855d6.png)

我們在網路的世界中可以透過 API (Application Programming Interface) 與各種應用程式互相交流、溝通、傳遞各種訊息，而如何驗證身份就變成一件重要的事情，即我們不應該將所有的資料都裸奔，讓任何人都可以存取，所以在設計 Web API 的時候常常會需要考慮到授權或驗證的部分，那我們要如何確認誰有權限呢？要如何安全地傳遞訊息呢？

JSON Web Token (JWT) 是由 Auth0 所提構出的一個新 Token 想法，這並不是一套軟體、也不是一個技術，如果你在做網站時有用 Token 驗證使用者身份的習慣，那麼這個方法你應該很快就能上手。接下來將解說為什麼 JWT 會比起傳統 Token 要來得好

通常利用 JWT 來對使用者進行驗證，也就是使用者會先請求身分提供的伺服器給予該 JWT，而後，只要使用者帶著這個 JWT 向資源伺服器請求資源，如果這個 JWT 是有效的，那麼就能獲取資源

在每一次請求 API 都要帶上 token 讓後端來驗證這個請求是否是有經過認證的，若沒有驗證過就不能讓它去訪問 API 取得或是更改資料

過去在專案中大多透過 Session 和 Cookie 實作驗證機制（例如：透過 Passport.js 實作驗證機制），在使用者成功登入後，會在伺服器端建立 Session，並提供客戶端相對應的 Session ID。往後來自客戶端的請求，伺服器以 Cookie 中 Session ID 尋找 Session 確認使用者驗證狀態。然而，使用這個方法，意味著每位用戶經過驗證後，都需要為其建立和存取 Session（可能在資料庫中）當用戶量增加，資料庫的花費成本將不斷提高；每當用戶拜訪需經驗證的路徑時，也需要額外到資料庫執行資料查詢，花費額外的效能

![透過 Session 和 Cookie 實作驗證機制.png](http://192.168.25.60:8000/files/file_storage/58838d32.png)

它更符合設計 RESTful API 時「Stateless 無狀態」原則：意味著每一次從客戶端向伺服器端發出的請求都是獨立的，使用者經驗證後，在伺服器端不會將用戶驗證狀態透過 Session 儲存起來，因此每次客戶端發出的請求都將帶有伺服器端需要的所有資訊 — 從客戶端發出給伺服器端的請求將帶有 JWT 字串表明身份

![透過 JWT 實作驗證機制.png](http://192.168.25.60:8000/files/file_storage/8c284744.png)


# 概念

![JWT 運作原理.png](http://192.168.25.60:8000/files/file_storage/fd60f3db.png)

![jwt-authentication.png](http://192.168.25.60:8000/files/file_storage/390c0d1d.png)

# JWT 何時可以使用
> - **授權 ( Authorization )**
這是很常見 JWT 的使用方式，例如使用者從 Client 端登入後，該使用者再次對 Server 端發送請求的時候，會夾帶著 JWT，允許使用者存取該 token 有權限的資源。單一登錄 ( Single Sign On ) 是當今廣泛使用 JWT 的功能之一，因為它的成本較小並且可以在不同的網域 ( domain ) 中輕鬆使用。

This is the most common scenario for using JWT. Once the user is logged in, each subsequent request will include the JWT, allowing the user to access routes, services, and resources that are permitted with that token. Single Sign On is a feature that widely uses JWT nowadays, because of its small overhead and its ability to be easily used across different domains.

> - **訊息交換 ( Information Exchange )**
JWT 可以透過公鑰/私鑰來做簽章，讓我們可以知道是誰發送這個 JWT，此外，由於簽章是使用 header 和 payload 計算的，因此還可以驗證內容是否遭到篡改。

JSON Web Tokens are a good way of securely transmitting information between parties. Because JWTs can be signed—for example, using public/private key pairs—you can be sure the senders are who they say they are. Additionally, as the signature is calculated using the header and the payload, you can also verify that the content hasn't been tampered with.

# 認證( Authentication ) 、 授權( Authorization ) 、 憑證（Credentials）
> **認證 ( Authentication )**：能不能做，你是誰，驗證使用者的身份
> **授權 ( Authorization )**：能做到什麼程度，你有沒有權利進行那項操作，例如給予第三方應用程式可以用哪些服務
> **憑證 ( Credentials )**：要實行驗證和授權之前，需要有東西可以證明這些事情，而這個東西就叫憑證，可以想像是你的身份證。例如你要在凌晨的時候進去網咖或是酒吧，必須要有東西證明你是成年的。

## 認證 Authentication
網頁應用上，伺服器常常需要知道發送請求的人是誰，用戶端發送 Request 要帶上足夠的資訊證明自己的身份，伺服器收到後，藉著那些資訊去判斷這個 Request 是否真的是那個人，這是認證 ( Authentication ) 的動作

如果該 Request 沒有帶上足夠的資訊或認證失敗，伺服器應該回應 `401 Unauthorized` 的狀態碼

### 網際網路中的認證：
- 使用者名稱密碼登錄
- 信箱傳送登錄連結
- 手機號接收驗證碼
- 信箱/驗證碼，你就是帳號的主人

## 授權 Authorization
認證動作完成，拿到 Request 發送方的身份後，因為有些動作不是每位使用者都有權限操作，判斷該身份是否有權限進行某個動作，這個是授權 ( Authorization )。常見的作法是利用角色 ( Role ) 來劃分權限

如果該 Request 代表的身份沒有權限進行當前的操作，伺服器應該回應 `403 Forbidden`

### 網際網路中的授權：
- 比如手機安裝 APP 的時候，會詢問是否允許授予權限（訪問相簿、位置等權限）
- 比如在訪問微信小程序時，當登錄時，小程序會詢問是否允許授予權限（獲取暱稱、頭像、地區、性別等個人資訊）
- 比如網站登錄的時候，可以使用 QQ ，微信進行登錄。（訪問你的個人資料，好友資訊等）

實現授權的方式有：cookie、session、token、OAuth 等

> 現實上，很多伺服器的實作不論認證或授權，只要失敗都會回應 403 Forbidden，但我們在開發後端應用程式的時候，提供足夠精確的狀態碼，更能讓存取你的資源的人明白發生了什麼事。
{.is-info}

## 憑證 Credentials
實現認證和授權的前提是需要一種媒介（證書） 來標記訪問者的身份

- 在戰國時期，商鞅變法，發明了照身帖。照身帖由官府發放，是一塊打磨光滑細密的竹板，上面刻有持有人的頭像和籍貫資訊。國人必須持有，如若沒有就被認為是黑戶，或者間諜之類的。
- 在現實生活中，每個人都會有一張專屬的居民身份證，是用於證明持有人身份的一種法定證件。通過身份證，我們可以辦理手機卡/銀行卡/個人貸款/交通出行等等，這就是**認證的憑證**。
- 在網際網路應用中，一般網站（如掘金）會有兩種模式，遊客模式和登錄模式。遊客模式下，可以正常瀏覽網站上面的文章，一旦想要點贊/收藏/分享文章，就需要登錄或者註冊帳號。當使用者登錄成功後，伺服器會給該使用者使用的瀏覽器頒發一個令牌（token），這個令牌用來表明你的身份，每次瀏覽器傳送請求時會帶上這個令牌，就可以使用遊客模式下無法使用的功能。
- 比如生活中，每個人都會有一張專屬的居民身份證，是用於證明持有人身份的一種法定證件。
- 通過身份證，我們可以辦理手機卡/銀行卡/個人貸款等等，這就是認證的憑證。

### 認證機制種類
最常見的認證機制為以下三種：

- Cookie
- Session
- JSON Web Token

# JWT 驗證流程

![JWT concept.png](http://192.168.25.60:8000/files/file_storage/60f13434.png)

## 1. POST /users/login with username and password
### 使用者發登入請求，帶入驗證所需資料( username 、password )
## 2. Create a JWT with a serect after authentication
### 伺服器端驗證使用者成功後，產生一組帶有資訊，且僅能在伺服器端被驗證的 Token
## 3. Returns the JWT to the Browser 
### 伺服器回傳 JWT 給使用者，存取在客戶端（大多存在瀏覽器的 Storage 當中）
## 4. Sends the JWT on the Authentication Header
### 使用者向伺服器端發送請求時，皆在 Authentication Header 附帶此 Token
## 5. Check JWT signature. Get user information from the JWT
### 伺服器端從 JWT 取得使用者資料與簽章，進行驗證
## 6. Sends response to Client
### 驗證 JWT token 是否有效，有效則回傳資源給客戶端

![JWT concept with Local Storage.png](http://192.168.25.60:8000/files/file_storage/ac6cfc08.png)


# 使用 Postman 設定 Authentication 與 Pre-request Script
## 實作 4. 使用者向伺服器端發送請求時，皆在 Authentication Header 附帶此 Token
### 實際情境
#### ※ 用戶登入
- 第一次用戶登入時會取得 token

![Postman post api user login.png](http://192.168.25.60:8000/files/file_storage/4112be5c.png)

```json
{
    "code": "00",
    "data": {
        "EmployeeName": "陳欣怡",
        "LoginDay": "2022/06/07",
        "LoginTime": "09:29:16",
        "Token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJFbXBsb3llZUlEIjoiMTE1NDIiLCJpYXQiOjE2NTQ1NjUzNTYsImV4cCI6MTY1NDU2NTQxNn0.u3yoBrkROPNNVvIKsMn5372t1z3a-NCAnb8ccOvDnQ8"
    }
}
```
#### ※ 用戶打卡：需進行 JWT 驗證才能打卡

```javascript
// signin.js
router.post(
  '/signin',
  // passport as middleware
  // 呼叫策略 'jwt' : JWT 驗證
  passport.authenticate('jwt', { session: false }),
  // 資料校驗
  getValidator(signinValidate),
  // routes handler
  async (ctx, next) => {
    ctx.body = await signinController.signin(ctx, next);
  },
);
```

如果使用了 passport.authenticate('jwt')，但是在 request Header 中沒有給 JWT Token ，會直接回傳 **Unauthorized**

![Postman post api user signin unauthorized.png](http://192.168.25.60:8000/files/file_storage/8e1bf32b.png)

以下模擬前端在 request Header 帶著 JWT Token
- 在 Pre-request Script 中發送一個 Post api/user/login ( EmployeeID 、 Password )
- 在 data.token 中將取得的 token 設定在 Postman 中的 environment 中，名稱為 my_token

![Postman Pre-requst Script.png](http://192.168.25.60:8000/files/file_storage/3a654cca.png)

- 若 request 用 body 方式則使用以下 Pre-request Script

```
// Pre-request Script
pm.sendRequest({
    url: 'http://192.168.25.180:10000/api/public/user/login',
    method: 'GET',
    header: {
        'Content-Type':'application/json'
    },
    body: {
        mode: 'raw',
        raw: JSON.stringify({ EmployeeID: "11542",  Password:"qwe123"})
    }
}, function (err, response) {
    const jsonParse = response.json();
    pm.environment.set("my_token", jsonParse.data.Token);
});
```

- 若 request 用 query 方式則使用以下 Pre-request Script

```
// Pre-request Script
pm.sendRequest({
    url: 'http://127.0.0.1:10001/api/public/user/login?EmployeeID=11542&Password=abc123',
    method: 'GET',
}, function (err, response) {
    const jsonParse = response.json();
    pm.environment.set("my_token", jsonParse.data.Token);
});
```

> [Pre-request Scripts info](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/)
{.is-info}

以下兩種方式皆可設定在 request Header 帶著 JWT Token

![Postman Two ways to set Authorization Bearer Token.png](http://192.168.25.60:8000/files/file_storage/71665d97.png)

- 方法1：在 Authorization 中設定 Type 為 Bearer Token，Token 為 {{my_token}}

![Postman Authorization Bearer Token.png](http://192.168.25.60:8000/files/file_storage/16f5a76d.png)

- 方法2：在 Headers 中設定一個 KEY 為 Authorization，VALUE 為 Bearer {{my_token}}

```
Authorization: Bearer <token>
```

> Bearer 跟 {{my_token}} 中間要有一個空格
{.is-warning}

![Postman Headers Authorization Bearer Token.png](http://192.168.25.60:8000/files/file_storage/79f862f0.png)

Post api/user/public/signin 打卡成功會 res ( SignDay、SignTime )

# Token 類型
> 一般來說通常包含兩類的 token - access token 和 refresh token。

當使用者第一次向伺服器要求登入時，伺服器通常會同時回應 access token 和 refresh token
```
# 例如向伺服器發送登入請求
curl -X POST -H -d 'username=webgoat&password=webgoat' localhost:8080/WebGoat/login
```

```
// 伺服器同時回應 access_token 和 refresh_token
{
  "token_type": "bearer",
  "access_token": "XXXX.YYYY.ZZZZ",
  "expires_in": 10,
  "refresh_token": "4a9a0b1eac1a34201b3c5659944e8b7"
}
```

接著所有使用 API 向伺服器進行操作時，都需要附上 access token 才可以存取資料。但一般來說 access token 會有使用期限，當過了效期之後，透過 refresh token 可以向伺服器核發新的 access token。refresh token 也可能會過期，但一般來說效期比 access token 來得長。

透過 refresh token 省去了使用者需要重新再向伺服器進行驗證（或授權）的動作，但因為有了 refresh token 等同於可以取得新的 access token ，因此當 client 在使用 refresh token 時，伺服器這邊應該要更留意安全性的考量，也許要記錄使用者的 IP、地點、裝置資訊、使用 refresh token 的次數，若有異常的情況，則要請使用者重新進行一次驗證。例如，每當使用不同裝置登入 google 或 facebook 時，都需要再重複進行一次驗證。

# Token 和 JWT 的區別
## 相同
- 都是訪問資源的令牌
- 都可以記錄使用者的資訊
- 都是使伺服器端無狀態化
- 都是只有驗證成功後，客戶端才能訪問伺服器端上受保護的資源
## 區別
- Token：伺服器端驗證客戶端傳送過來的 Token 時，還需要查詢資料庫獲取使用者資訊，然後驗證 Token 是否有效。
- JWT：將 Token 和 Payload 加密後儲存於客戶端，伺服器端只需要使用金鑰解密進行校驗（校驗也是 JWT 自己實現的）即可，不需要查詢或者減少查詢資料庫，因為 JWT 自包含了使用者資訊和加密的資料。

# JWT Token 的組成
JWT 包含三個 JSON object，並以（.）區隔，而這三個部分會各自進行 Base64 編碼，組成一個 JWT 字串

- **標頭 Header**：含 Token 的種類及產生簽章（signature）要使用的雜湊演算法
- **內容 Payload**：帶有欲存放的資訊（例如用戶資訊）
- **簽章 Signature**：編譯後的 Header、Payload 與密鑰透過雜湊演算法所產生
- **Base64**：是透過64個字符來表示二進制數據的一種方法，編碼的方式是固定的而且是可以逆向解碼的，並不是那種安全的加密演算法
```
// JWT example
// base64(Header) + base64(Payload) + alg(base64(Header).base64(Payload),secret)
// xxxxx.yyyyy.zzzzz
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJFbXBsb3llZUlEIjoiMTE1NDIiLCJpYXQiOjE2NTQwNzE0NDksImV4cCI6MTY1NDA3MTUwOX0.QAw6yqgZciRGxyEUNSWaeHMml8Q9haj6QKeJHvE0bvk
```

可以使用 [JWT.io](https://jwt.io/) 來做驗證

![JWT example with comment.png](http://192.168.25.60:8000/files/file_storage/49d1cac6.png)

![JWT example with encode.png](http://192.168.25.60:8000/files/file_storage/1038a1c1.png)

## 標頭 Header
> Header 是一個包含定義 Token 種類（type）及雜湊演算法（alg）資訊的 JSON
- `alg` ( Hashing Algorithm )：雜湊演算法 
- `typ` ( Type of Token )：Token 種類 

在此設定 Token 種類為 JWT、產生簽章（signature）要使用的雜湊演算法為 HS256
此 JSON 將被轉換成 Base64 編碼，成為第一個部分

```json
{
  "alg": "HS256", // Hashing Algorithm
  "typ": "JWT"    // Type of Token
}
```
```
// base64(Header)
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9
```


## 內容 Payload（Claims）
> - Payload 是一個包含使用者和相關的資訊的 JSON
> - Payload 又可以被稱為 claims，能想成使用者是透過附帶 JWT 通過認證，因此能說這筆資訊是屬於他（她）的

通常包含三種
### 1). **Registered claims**：註冊所有權資訊
#### 可以想成是標準公認的一些訊息建議你可以放，但並不強迫

- `iss` ( Issuer )：JWT 簽發者
- `exp` ( Expiration Time )：JWT 的過期時間，過期時間必須大於簽發 JWT 時間
- `sub` ( Subject )：JWT 所面向的用戶
- `aud` ( Audience )：接收 JWT 的一方
- `nbf` ( Not Before )：也就是定義擬發放 JWT 之後，某段時間點前該 JWT 仍舊是不可用的
- `iat` ( Issued At )：JWT 簽發時間
- `jti` ( JWT Id )：JWT 的身分標示，每個 JWT 的 Id 都應該是不重複的，避免重複發放

### 2). Public claims：公開所有權資訊
#### 可以想成是傳遞的欄位必須是跟上面Registered claims欄位不能衝突，然後可以向官方申請定義公開聲明，會進行審核等步驟，實務上在開發上是不太會用這部分的

### 3). Private claims：私人所有權資訊
#### 發放 JWT 伺服器可以自定義的欄位的部分，實務上不會放使用者的密碼等敏感數據，因為該 Payload 傳遞的訊息最後也是透過 Base64 進行編碼，所以是可以被破解的，因此放使用者密碼會有安全性的問題

- User Account：使用者帳號
- User Name：使用者名字
- User Role：使用者角色，可以用來看該使用者是否有權限取得後端 API 的內容

在此設定 EmployeeID 為自定義的欄位，設定 Token 簽發時間，設定 Token 到期的時間
此 JSON 將被轉換成 Base64 編碼，成為第二個部分
```json
{
  "EmployeeID": "11542", // Private claims
  "iat": 1654071449,     // Issued At
  "exp": 1654071509      // Expiration Time
}
```
```
// base64(Payload)
eyJFbXBsb3llZUlEIjoiMTE1NDIiLCJpYXQiOjE2NTQwNzE0NDksImV4cCI6MTY1NDA3MTUwOX0
```
> 不要將隱私資訊存放在 Payload 當中
> Payload 和 Header 被轉換成 Base64 編碼後，能夠被輕易的轉換回來
> 因此不應該把如用戶密碼等重要資料存取在 Payload 當中
{.is-warning}

## 簽章 Signature
> - Signature 是將被轉換成 Base64 編碼的 Header、Payload 與自己定義的密鑰 Secret，透過在 Header 設定的雜湊演算法方式所產生的
> - 每一個 JWT token 都應該在送出給 client 前進行簽章（sign），如果一個 token 沒有簽章，那麼 client 即可自由修改 token 中的內容

- **secret**：密鑰，由於密鑰並非公開，因此伺服器端在拿到 Token 後，能透過解碼，確認資料內容正確，且未被變更，以驗證對方身份

> secret 是要保存在伺服器端的，這個 secret 一旦外洩給客戶端，客戶端就可以自己產生 JWT ，並且透過該 JWT 存取資源，因此 secret 是永遠不該外流的
{.is-warning}

在此將 header 與 payload 用 Base64 編碼，設定 secret 為 THISISAREALLYNICESCRETKRYs
用 HS256 進行加密成為第三個部分
```
HMACSHA256(
  base64UrlEncode(header) + "." + base64UrlEncode(payload),
	THISISAREALLYNICESCRETKRYs
)
```
```
// alg(base64(Header).base64(Payload),secret)
QAw6yqgZciRGxyEUNSWaeHMml8Q9haj6QKeJHvE0bvk
```

# JWT 的優缺點
## 優點
- 採用 JSON object 的形式，大部分的程式語言皆支援
- 可存放一些使用者資訊，但並非是敏感的資訊
- 整個 JWT ，只要 Payload 不要放過多的資訊，其實 Size 是相當小的
- 不用在 Server 的資料庫存放 Session ，特別適合多台 Server 的情境下，使得擴展性容易，因為多台 Server 要使用 Session 的話，會有共享 Session 的問題產生，
- 對於現在手機上的 APP 的應用特別好，使用者不用每次打開 APP 都要重新輸入帳號與密碼
- 支持跨域請求，不會有傳統用 Cookie 進行跨域請求等問題
## 缺點
- JWT 沒辦法主動被中止，也就是說不能像 Session 一樣被強制無效，但是個人覺得這有很多方式可以避免
- JWT 一旦洩漏會有很大的安全性的問題，但是洩漏通常會透過兩種方式：
	1. 駭客使用你的電腦，並得知 JWT
	2. 使用中間人攻擊的方式，擷取客戶端傳送伺服器端的封包，並獲取 JWT ，但使用 HTTPS 傳輸可以大幅度降低該攻擊，只要定期更換 SSL 證書就可以了

# 使用 JWT 時需要考慮的問題
- 因為 JWT 並不依賴 Cookie 的，所以你可以使用任何域名提供你的 API 服務而不需要擔心跨域資源共享問題（CORS）
- JWT 默認是不加密，但也是可以加密的。生成原始 Token 以後，可以用金鑰再加密一次。
- JWT 不加密的情況下，不能將秘密資料寫入 JWT 。
- JWT 不僅可以用於認證，也可以用於交換資訊。有效使用 JWT，可以降低伺服器查詢資料庫的次數。
- JWT 最大的優勢是伺服器不再需要儲存 Session，使得伺服器認證鑑權業務可以方便擴展。但這也是 JWT 最大的缺點：由於伺服器不需要儲存 Session 狀態，因此使用過程中無法廢棄某個 Token 或者更改 Token 的權限。也就是說一旦 JWT 簽發了，到期之前就會始終有效，除非伺服器部署額外的邏輯。
- JWT 本身包含了認證資訊，一旦洩露，任何人都可以獲得該令牌的所有權限。為了減少盜用，JWT 的有效期應該設定得比較短。對於一些比較重要的權限，使用時應該再次對使用者進行認證。
- JWT 適合一次性的命令認證，頒發一個有效期極短的 JWT ，即使暴露了危險也很小，由於每次操作都會生成新的 JWT ，因此也沒必要保存 JWT ，真正實現無狀態。
- 為了減少盜用，JWT 不應該使用 HTTP 協議明碼傳輸，要使用 HTTPS 協議傳輸。


# JWT 生成實作
主要會使用到 **jsonwebtoken**

## jsonwebtoken
> - An implementation of JSON Web Tokens.
> - 產生 JWT

透過模組上的 sign() 方法可以產生一組 JWT
```javascript
// import
const jwt = require('jsonwebtoken');
// 產生 JWT
jwt.sign(payload, secretOrPrivateKey, [options, callback])
```
- `payload`：存放在 Token 當中的資料
- `secretOrPrivateKey`：密鑰 Secret
- `options`：非必填，但透過帶入 options 物件能設定許多選項
	- `algorithm`：設定產生簽章要使用的雜湊演算法（預設： HS256）
	- `expiresIn`：設定 Token 多久後會過期（自動在 Payload 新增 exp）
	- `noTimestamp`：設定產生 JWT 時不會自動在 Payload 中新增 iat 時間
- `callback`：非必填，但若要以非同步方式產生 JWT，可以提供一個 Callback 函式，Token 將能在 Callback 函式中取得
```javascript
// verificationService.js
// import
const jwt = require('jsonwebtoken');
// 產生 JWT
const payload = { EmployeeID: employeeID };
const secret = process.env.JWT_PASSPORT_SECRET;
const jwtSignOptions = { expiresIn: '1min' };
const token = jwt.sign(payload, secret, jwtSignOptions);
```


# JWT 驗證實作
主要會使用到 **passport-jwt**、**koa-passport**

```diagram
PHN2ZyB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiB2ZXJzaW9uPSIxLjEiIHdpZHRoPSI2MDFweCIgaGVpZ2h0PSIxMjIycHgiIHZpZXdCb3g9Ii0wLjUgLTAuNSA2MDEgMTIyMiIgY29udGVudD0iJmx0O214ZmlsZSBob3N0PSZxdW90O2VtYmVkLmRpYWdyYW1zLm5ldCZxdW90OyBtb2RpZmllZD0mcXVvdDsyMDIyLTA2LTEwVDAyOjI2OjQzLjIyOFomcXVvdDsgYWdlbnQ9JnF1b3Q7NS4wIChXaW5kb3dzIE5UIDEwLjA7IFdpbjY0OyB4NjQpIEFwcGxlV2ViS2l0LzUzNy4zNiAoS0hUTUwsIGxpa2UgR2Vja28pIENocm9tZS8xMDIuMC4wLjAgU2FmYXJpLzUzNy4zNiZxdW90OyBldGFnPSZxdW90O2VraVBoaUJ2ZkZBNEVoai1TMG5mJnF1b3Q7IHZlcnNpb249JnF1b3Q7MTkuMC4zJnF1b3Q7IHR5cGU9JnF1b3Q7ZW1iZWQmcXVvdDsmZ3Q7Jmx0O2RpYWdyYW0gaWQ9JnF1b3Q7aHdlWjgwYUpDSTRBTDJhYmNLTmUmcXVvdDsgbmFtZT0mcXVvdDtKV1QmcXVvdDsmZ3Q7N1ZwdGM2TTJFUDQxbW1rL0pDUEFZUHdSL0hLZHp2V2FhVEp6dlU4ZEdSU2JpNHdvNERqdXIrK3VFTVlZc1BFbGNlWW1ZZktDVnRLeWtuYWZaeVVnMW5qMTlDbGx5ZklQR1hKQlRCbytFV3RDVE5NWW1DYkJIeHB1QzhuUTBZSkZHb1c2VVNXNGpmN2pXa2kxZEIyRlBLczF6S1VVZVpUVWhZR01ZeDdrTlJsTFU3bXBON3VYb3Y3VWhDMTRRM0FiTU5HVWZvM0NmS21saGpPcUtuN2owV0twSCsyYXc2Sml4Y3JHZWlUWmtvVnlzeWV5cHNRYXAxTG14ZDNxYWN3RlRsNDVMMFcvV1VmdHpyQ1V4M21mRHFhajdjaTM1ZUI0Q0dQVlJabm1TN21RTVJQVFN1cW5jaDJISERWUUtGVnRQa3VaZ05BQTRYZWU1MXU5Y0d5ZFN4QXQ4NVhRdFdCY3V2MGIrMS9iWmZHYlZxY0trNmRhYWF0TGdzMjU4Rm53c0ZBbWpLV1FLVlRGTWthejdtV2NseUppV2pOMWdidzVKM3FhTXJsT0F6MXE3WUE1U3hlOGJLVlhFeWRrcjUrZXlFOWNyamdZQncxU0xsZ2VQZGFkZzJrZlcremFWY3NBTjNvbE9sYWxVUEhJeEZvckphWWpjajNHMm5JNS82NWxXWEdWcVFuM29JRTVTSjdVME10NnVGdm8vMHJSZkNlWURzbG9RRHlYVEIzaXVzUjMxTTJJakN3eXRZazdJWjVSZGdLNzV3MUY2YUVFV2hWbTFzV2RwbmNiMlhlMGh0TXgyaHVab1Ixc0JYN3B4L01zT1cxbWxyRDRXYzhFdlN5SjRPODY0ekEzc3l4YXhGRzg5OVRpQ1NmbTdDQW9xNUREK05rc281emZKa3c1N3dZd3RoNWU5NUVRZTRIQVRFcE5qTE1zVCtVRDM2dHhaaFF1cUdFQ2pBUlpBQ0VDUmpkaTZWNWR4Mkxwa2FjNWZ6b2FKcnAyTU5EWXA4Ry9MRzRxSk4zSmxuc2dhbEQ2L01neW5QTkQ2MmdRUFNjVUlieEdCdkhIS3M1bXhEc2VVNDNJNit1cC9iR2dkRlU2QmcycEZBSWR1TnVBRDE4KzlHVklBWXdMZW5PWlZyeDc5aTZwZXArK3l4VHI4dlJkV25NaC9yYUpiNUNSVGFiQTJjRGNQdDU0Y0Q4a1U1ZDRFK1R5dnZ4OURsRSttNXdUbG1VSitOODF1TmdTVmpjS1dNNC9RQVFjdHpjanZrU3U2Ylk0NitWQjVUVlRlOW9DRHNhYjVmYjBvdUF3SVA2RStMTXF5emZwNzEvdjRHOExTa0FidUFFTXdjWTI4ZXlYekVmdTJTb1MyMklBTzZUUkRiNUljQWVUM3JJNGczOTM0OE42WW81YmV2Mlo4SGpYcTYxREJqVlhrSVJIUmFEU2M0SHA2dnVtV0k4VWdHbXg3VTU3K3NIbSswRXd5N2xrVG0rT1ByS2dHcWpWZ001OE02QXIzZWdWdDFyZDdYdHY0MGRIZG1mK2xJeW1CZkljMi9EMHpvVGNqbWNCenZ5VE1DRlorRktIRTUyalF1QWY0cW1PYjVZakhMWlJnVXQ4aTdpRnhDV3VkM3JQMStOa3FOZCsrY2lLZ0MxRDRydUtuMFpxNHp6QW91ZW9vUlJuVjdDVjF0VFZoY3BOUTEvcldLcHJ2UitSa3pEb0FpYkVITERneExKM2JMN2ZLNzNZNWlYcHhhS051WDJuOU5KeVJtNWFiMFl2M1lma3gxRnlveDBFSTNTdTN2eTA1SWFtMVFsQ1BiQVM4Y2tqN2dCUDBxR05aNm5VMmlTK3A4N1dLY0xZY1R4dFl1ZXp0OXNuWjZjZGNWMVhEMDJQMGNFTkFxQXZJcTZIWjVZdFZTTWNJeEtNaXk4UDNMR2FoeGx1UUxBWFJmbzVkL2huV0oya1ViWmkxM2o4bnAzOW5IY01xNDU3VVZnMUduUDdUbUhWYW9IVndadkJxblhSNDRsRzJ0WjZLdUhiWkVRVnZrQzZPajRySzNvMU9BMWg2WC81OVdlRGpleUI1OEZTTy9VcllJaDcwYmQ1WlpoY3psbXBJcm9lUHRxU0ZPejUrc2hYRzh1UFUvbWZoeDBOMnY5WS9pWG9zUTJIRC9reURqMzhtZ2xuUk1DeVJVRjlndXRzeVoraWZJLzVvUFN0SkVXNHIzZ1BDeVh0L1NoZDlpTytRWlA0eXZmekwwZDh1dXVOakZSZzZOVzFyY1BWSFI0c1dtR3A3bGF0VzBPVGMxSlRNYjZHSnVVQnV4SDFjNHFUZUJmc2dxR0thNHZTSUVDZnJFUTljSkgyeDhVbUhLb3ZpUkFPYlVTKzBVemhJdXdLekdPWWR3cGMwQ2MrWXg2R3JzM0JWRFpYVmJUdTlRMlF3SmdIRkJPZXJsaEZZYWl5eWVOSlhWRXIwNUNuQnpWTjd5NmpWWDl3cUEwajFjdWUwN0R6M0V4T3pyL2pwNDVnbHdEQVhjTEdGcmMvKzhxQ3dSZTIwdnE3M3dFNXlHU3VvWGFzTHI1TjFuM3Z0b251aXgrSHNDam02VzBnRSs3ajFERnRHclR6a2tRZ1owUXlMcHJYM3FaTUxkd2F1c2FEWkZkbFJkRlRGQ3Y3SXh5dXVLeUhvMnBCR0QyMmFvVVZ5cSswOTZCYXdlL3pOclcybmtqVDN2TmNwZlFIVHUxZXhDUzdzR3AvNnNFNGU5SmxuN21QS2ZhZVAycTg2V1IwdEYzdnJZd2VEQit5Ykxucnk5SkFkelhwQWZ2cnVHb1N2N3FhUEY5Y0p5TzRSQUkxYTAwY21NczhsNnVqa1o3QjBLSjRjYWQybDdRUzRKQ29IdUFOeXdGblVDRytYQjJBRkVLVlRjTW8zOUZ3enZJOXJFcDRHa0VUeEtaSmltZ1JMd1MvcVlSK0xEWE1WVnZQR3hhR3hYT0xQRWpJalZkK3lJeVNPdkFVUEo2d3VPUitlTXc2elFCZC91SkYyQlJOd0ViZEErMU1rS0Y0T24wRVdDdjFzbmtteFRybjNtN3hkaTBWdU5qb2ZQUWFRMkZNMVM5NjNWZ0oyMlREcHREQVVxbWhMbXlURGUwMmxVYkxzdzlsWm91d1ZXWExzK21Ca2ZEeklsbW1XMDhlMm5aUHRDWEZkTS9QTUtHb0thSXNWbCtaRjZsSTlhMitOZjBmJmx0Oy9kaWFncmFtJmd0OyZsdDsvbXhmaWxlJmd0OyI+PGRlZnMvPjxnPjxwYXRoIGQ9Ik0gNDgwIDEwMCBMIDQ4MCAxNTMuNjMiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDQ4MCAxNTguODggTCA0NzYuNSAxNTEuODggTCA0ODAgMTUzLjYzIEwgNDgzLjUgMTUxLjg4IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjM2MCIgeT0iMCIgd2lkdGg9IjI0MCIgaGVpZ2h0PSIxMDAiIHJ4PSIxNSIgcnk9IjE1IiBmaWxsPSIjYTIwMDI1IiBzdHJva2U9IiM2ZjAwMDAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMjM4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogNTBweDsgbWFyZ2luLWxlZnQ6IDM2MXB4OyI+PGRpdiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiAjZmZmZmZmOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMjU1LCAyNTUsIDI1NSk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPjxmb250IHN0eWxlPSJmb250LXNpemU6IDI0cHg7Ij48Yj7nlKjmiLbmiZPljaE8L2I+PGJyIC8+PC9mb250Pjxmb250IHN0eWxlPSIiPjxmb250IHN0eWxlPSJmb250LXNpemU6IDE2cHg7Ij5Qb3N0wqA8L2ZvbnQ+PHNwYW4gc3R5bGU9ImZvbnQtc2l6ZTogMTZweDsiPi9hcGkvdXNlci9zaWduaW48L3NwYW4+PC9mb250PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSI0ODAiIHk9IjU0IiBmaWxsPSIjZmZmZmZmIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPueUqOaItuaJk+WNoSYjeGE7UG9zdMKgL2FwaS91c2VyL3NpZ25pbjwvdGV4dD48L3N3aXRjaD48L2c+PHJlY3QgeD0iMzYwIiB5PSIxMTIxIiB3aWR0aD0iMjQwIiBoZWlnaHQ9IjEwMCIgcng9IjE1IiByeT0iMTUiIGZpbGw9IiNhMjAwMjUiIHN0cm9rZT0iIzZmMDAwMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3QgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAyMzhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxMTcxcHg7IG1hcmdpbi1sZWZ0OiAzNjFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogI2ZmZmZmZjsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDI1NSwgMjU1LCAyNTUpOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij48Zm9udCBzdHlsZT0iIj48YiBzdHlsZT0iZm9udC1zaXplOiAyNHB4OyI+5ZG85Y+rPGJyIC8+PC9iPjxzcGFuIHN0eWxlPSJmb250LXNpemU6IDI0cHg7Ij48Yj5zaWduaW4gQ29udHJvbGxlcjwvYj48L3NwYW4+PC9mb250PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSI0ODAiIHk9IjExNzUiIGZpbGw9IiNmZmZmZmYiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+5ZG85Y+rJiN4YTtzaWduaW4gQ29udHJvbGxlcjwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA0ODAgMjYxIEwgNDgwIDMxMy42MyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gNDgwIDMxOC44OCBMIDQ3Ni41IDMxMS44OCBMIDQ4MCAzMTMuNjMgTCA0ODMuNSAzMTEuODggWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iMzYwIiB5PSIxNjAiIHdpZHRoPSIyNDAiIGhlaWdodD0iMTAxIiByeD0iMTUuMTUiIHJ5PSIxNS4xNSIgZmlsbD0iI2EyMDAyNSIgc3Ryb2tlPSIjNmYwMDAwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDIzOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDIxMXB4OyBtYXJnaW4tbGVmdDogMzYxcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNmZmZmZmY7ICIgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7Ij48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigyNTUsIDI1NSwgMjU1KTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+PGZvbnQgc3R5bGU9ImZvbnQtc2l6ZTogMjRweDsiPjxiPuWxlemWi+mpl+itiTwvYj48YnIgLz7CoDwvZm9udD48Zm9udCBzdHlsZT0iZm9udC1zaXplOiAxNnB4OyI+cGFzc3BvcnQuYXV0aGVudGljYXRlPC9mb250PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSI0ODAiIHk9IjIxNCIgZmlsbD0iI2ZmZmZmZiIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj7lsZXplovpqZforYkmI3hhO8KgcGFzc3BvcnQuYXV0aGVudGljYXRlPC90ZXh0Pjwvc3dpdGNoPjwvZz48cGF0aCBkPSJNIDQ4MCA0MjAgTCA0ODAgNDczLjYzIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA0ODAgNDc4Ljg4IEwgNDc2LjUgNDcxLjg4IEwgNDgwIDQ3My42MyBMIDQ4My41IDQ3MS44OCBaIiBmaWxsPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48cmVjdCB4PSIzNjAiIHk9IjMyMCIgd2lkdGg9IjI0MCIgaGVpZ2h0PSIxMDAiIHJ4PSIxNSIgcnk9IjE1IiBmaWxsPSIjYTIwMDI1IiBzdHJva2U9IiM2ZjAwMDAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMjM4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMzcwcHg7IG1hcmdpbi1sZWZ0OiAzNjFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogI2ZmZmZmZjsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGNlbnRlcjsiPjxkaXYgc3R5bGU9ImRpc3BsYXk6IGlubGluZS1ibG9jazsgZm9udC1zaXplOiAxMnB4OyBmb250LWZhbWlseTogSGVsdmV0aWNhOyBjb2xvcjogcmdiKDI1NSwgMjU1LCAyNTUpOyBsaW5lLWhlaWdodDogMS4yOyBwb2ludGVyLWV2ZW50czogYWxsOyB3aGl0ZS1zcGFjZTogbm9ybWFsOyBvdmVyZmxvdy13cmFwOiBub3JtYWw7Ij48Zm9udCBzdHlsZT0iZm9udC1zaXplOiAyNHB4OyI+PGI+5L2/55SoIEpXVCDpqZforYnnrZbnlaU8YnIgLz48L2I+PHNwYW4gc3R5bGU9ImZvbnQtZmFtaWx5OiAmcXVvdDtOb3RvIFNhbnMgVEMmcXVvdDssICZxdW90O09wZW4gU2FucyZxdW90Oywgc2Fucy1zZXJpZjsgZm9udC1zaXplOiAxNnB4OyI+cGFzc3BvcnQtand0IHN0cmF0ZWd5PC9zcGFuPsKgPC9mb250PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSI0ODAiIHk9IjM3NCIgZmlsbD0iI2ZmZmZmZiIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj7kvb/nlKggSldUIOmpl+itieetlueVpS4uLjwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA0ODAgNTgwIEwgNDgwIDYzMy42MyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gNDgwIDYzOC44OCBMIDQ3Ni41IDYzMS44OCBMIDQ4MCA2MzMuNjMgTCA0ODMuNSA2MzEuODggWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iMzYwIiB5PSI0ODAiIHdpZHRoPSIyNDAiIGhlaWdodD0iMTAwIiByeD0iMTUiIHJ5PSIxNSIgZmlsbD0iI2EyMDAyNSIgc3Ryb2tlPSIjNmYwMDAwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDIzOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDUzMHB4OyBtYXJnaW4tbGVmdDogMzYxcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNmZmZmZmY7ICIgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7Ij48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigyNTUsIDI1NSwgMjU1KTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+PGZvbnQgc3R5bGU9IiI+PGIgc3R5bGU9IiI+PHNwYW4gc3R5bGU9ImZvbnQtc2l6ZTogMTlweDsiPuW+niA8L3NwYW4+PGZvbnQgc3R5bGU9ImZvbnQtc2l6ZTogMThweDsiPmp3dF9wYWxvYWQ8L2ZvbnQ+PHNwYW4gc3R5bGU9ImZvbnQtc2l6ZTogMTlweDsiPiDnjbLlvpfpqZforYnos4foqIo8L3NwYW4+PC9iPjxiciAvPjxiIHN0eWxlPSJmb250LXNpemU6IDE5cHg7Ij7op7jnmbzkuKbluLblhaXCoDwvYj48YiBzdHlsZT0iIj48Zm9udCBzdHlsZT0iZm9udC1zaXplOiAxOHB4OyI+dmVyaWZ5IGNhbGxiYWNrPC9mb250PjwvYj48L2ZvbnQ+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjQ4MCIgeT0iNTM0IiBmaWxsPSIjZmZmZmZmIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPuW+niBqd3RfcGFsb2FkIOeNsuW+l+mpl+itieizh+ioii4uLjwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA0ODAgNzQwIEwgNDgwIDc5My42MyIgZmlsbD0ibm9uZSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9InN0cm9rZSIvPjxwYXRoIGQ9Ik0gNDgwIDc5OC44OCBMIDQ3Ni41IDc5MS44OCBMIDQ4MCA3OTMuNjMgTCA0ODMuNSA3OTEuODggWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PHJlY3QgeD0iMzYwIiB5PSI2NDAiIHdpZHRoPSIyNDAiIGhlaWdodD0iMTAwIiByeD0iMTUiIHJ5PSIxNSIgZmlsbD0iI2EyMDAyNSIgc3Ryb2tlPSIjNmYwMDAwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDIzOHB4OyBoZWlnaHQ6IDFweDsgcGFkZGluZy10b3A6IDY5MHB4OyBtYXJnaW4tbGVmdDogMzYxcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNmZmZmZmY7ICIgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7Ij48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigyNTUsIDI1NSwgMjU1KTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+PHNwYW4gc3R5bGU9ImZvbnQtd2VpZ2h0OiBib2xkOyBmb250LXNpemU6IDIzcHg7Ij7pqZforYnos4foqIrnmoTmraPnorrmgKc8L3NwYW4+PGJyIC8+PGZvbnQgc3R5bGU9ImZvbnQtc2l6ZTogMTZweDsiPjxzcGFuIHN0eWxlPSIiPuiIh+izh+aWmeW6q+izh+aWmemAsuihjOavlOWwjTwvc3Bhbj48YnIgLz48c3BhbiBzdHlsZT0iIj5wcmlzbWEudXNlcnM8L3NwYW4+PGJyIC8+PC9mb250PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSI0ODAiIHk9IjY5NCIgZmlsbD0iI2ZmZmZmZiIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMnB4IiB0ZXh0LWFuY2hvcj0ibWlkZGxlIj7pqZforYnos4foqIrnmoTmraPnorrmgKfoiIfos4fmlpnluqvos4fmlpnpgLLooYzmr5TlsI1wcmlzbWEudXNlcnMuLi48L3RleHQ+PC9zd2l0Y2g+PC9nPjxwYXRoIGQ9Ik0gNDgwIDkwMCBMIDQ4MCA5NTMuNjMiIGZpbGw9Im5vbmUiIHN0cm9rZT0icmdiKDAsIDAsIDApIiBzdHJva2UtbWl0ZXJsaW1pdD0iMTAiIHBvaW50ZXItZXZlbnRzPSJzdHJva2UiLz48cGF0aCBkPSJNIDQ4MCA5NTguODggTCA0NzYuNSA5NTEuODggTCA0ODAgOTUzLjYzIEwgNDgzLjUgOTUxLjg4IFoiIGZpbGw9InJnYigwLCAwLCAwKSIgc3Ryb2tlPSJyZ2IoMCwgMCwgMCkiIHN0cm9rZS1taXRlcmxpbWl0PSIxMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxyZWN0IHg9IjM2MCIgeT0iODAwIiB3aWR0aD0iMjQwIiBoZWlnaHQ9IjEwMCIgcng9IjE1IiByeT0iMTUiIGZpbGw9IiNhMjAwMjUiIHN0cm9rZT0iIzZmMDAwMCIgcG9pbnRlci1ldmVudHM9ImFsbCIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3QgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgY2VudGVyOyBqdXN0aWZ5LWNvbnRlbnQ6IHVuc2FmZSBjZW50ZXI7IHdpZHRoOiAyMzhweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiA4NTBweDsgbWFyZ2luLWxlZnQ6IDM2MXB4OyI+PGRpdiBkYXRhLWRyYXdpby1jb2xvcnM9ImNvbG9yOiAjZmZmZmZmOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDEycHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMjU1LCAyNTUsIDI1NSk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPjxmb250IHN0eWxlPSJmb250LXNpemU6IDI0cHg7Ij48Yj7luLblhaXpqZforYnntZDmnpw8L2I+PC9mb250PjxiciAvPjxmb250IHN0eWxlPSJmb250LXNpemU6IDE2cHg7Ij5kb25lKCk8L2ZvbnQ+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjQ4MCIgeT0iODU0IiBmaWxsPSIjZmZmZmZmIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjEycHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPuW4tuWFpempl+itiee1kOaenCYjeGE7ZG9uZSgpPC90ZXh0Pjwvc3dpdGNoPjwvZz48cmVjdCB4PSIzNjAiIHk9Ijk2MCIgd2lkdGg9IjI0MCIgaGVpZ2h0PSIxMDAiIHJ4PSIxNSIgcnk9IjE1IiBmaWxsPSIjYTIwMDI1IiBzdHJva2U9IiM2ZjAwMDAiIHBvaW50ZXItZXZlbnRzPSJhbGwiLz48ZyB0cmFuc2Zvcm09InRyYW5zbGF0ZSgtMC41IC0wLjUpIj48c3dpdGNoPjxmb3JlaWduT2JqZWN0IHBvaW50ZXItZXZlbnRzPSJub25lIiB3aWR0aD0iMTAwJSIgaGVpZ2h0PSIxMDAlIiByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiIHN0eWxlPSJvdmVyZmxvdzogdmlzaWJsZTsgdGV4dC1hbGlnbjogbGVmdDsiPjxkaXYgeG1sbnM9Imh0dHA6Ly93d3cudzMub3JnLzE5OTkveGh0bWwiIHN0eWxlPSJkaXNwbGF5OiBmbGV4OyBhbGlnbi1pdGVtczogdW5zYWZlIGNlbnRlcjsganVzdGlmeS1jb250ZW50OiB1bnNhZmUgY2VudGVyOyB3aWR0aDogMjM4cHg7IGhlaWdodDogMXB4OyBwYWRkaW5nLXRvcDogMTAxMHB4OyBtYXJnaW4tbGVmdDogMzYxcHg7Ij48ZGl2IGRhdGEtZHJhd2lvLWNvbG9ycz0iY29sb3I6ICNmZmZmZmY7ICIgc3R5bGU9ImJveC1zaXppbmc6IGJvcmRlci1ib3g7IGZvbnQtc2l6ZTogMHB4OyB0ZXh0LWFsaWduOiBjZW50ZXI7Ij48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTJweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYigyNTUsIDI1NSwgMjU1KTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IGFsbDsgd2hpdGUtc3BhY2U6IG5vcm1hbDsgb3ZlcmZsb3ctd3JhcDogbm9ybWFsOyI+PGZvbnQgc3R5bGU9ImZvbnQtc2l6ZTogMjRweDsiPjxiPuWwh+mpl+itiee1kOaenOizh+ioiuW4tuWbnjwvYj48YnIgLz7CoDwvZm9udD48Zm9udCBzdHlsZT0iZm9udC1zaXplOiAxNnB4OyI+cGFzc3BvcnQuYXV0aGVudGljYXRlPC9mb250PjwvZGl2PjwvZGl2PjwvZGl2PjwvZm9yZWlnbk9iamVjdD48dGV4dCB4PSI0ODAiIHk9IjEwMTQiIGZpbGw9IiNmZmZmZmYiIGZvbnQtZmFtaWx5PSJIZWx2ZXRpY2EiIGZvbnQtc2l6ZT0iMTJweCIgdGV4dC1hbmNob3I9Im1pZGRsZSI+5bCH6amX6K2J57WQ5p6c6LOH6KiK5bi25ZueJiN4YTvCoHBhc3Nwb3J0LmF1dGhlbnRpY2F0ZTwvdGV4dD48L3N3aXRjaD48L2c+PHBhdGggZD0iTSA0ODAgMTA2MCBMIDQ4MCAxMTE0LjYzIiBmaWxsPSJub25lIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0ic3Ryb2tlIi8+PHBhdGggZD0iTSA0ODAgMTExOS44OCBMIDQ3Ni41IDExMTIuODggTCA0ODAgMTExNC42MyBMIDQ4My41IDExMTIuODggWiIgZmlsbD0icmdiKDAsIDAsIDApIiBzdHJva2U9InJnYigwLCAwLCAwKSIgc3Ryb2tlLW1pdGVybGltaXQ9IjEwIiBwb2ludGVyLWV2ZW50cz0iYWxsIi8+PGcgdHJhbnNmb3JtPSJ0cmFuc2xhdGUoLTAuNSAtMC41KSI+PHN3aXRjaD48Zm9yZWlnbk9iamVjdCBwb2ludGVyLWV2ZW50cz0ibm9uZSIgd2lkdGg9IjEwMCUiIGhlaWdodD0iMTAwJSIgcmVxdWlyZWRGZWF0dXJlcz0iaHR0cDovL3d3dy53My5vcmcvVFIvU1ZHMTEvZmVhdHVyZSNFeHRlbnNpYmlsaXR5IiBzdHlsZT0ib3ZlcmZsb3c6IHZpc2libGU7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHhtbG5zPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hodG1sIiBzdHlsZT0iZGlzcGxheTogZmxleDsgYWxpZ24taXRlbXM6IHVuc2FmZSBjZW50ZXI7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGNlbnRlcjsgd2lkdGg6IDFweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiAxMDkycHg7IG1hcmdpbi1sZWZ0OiA0ODFweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogcmdiKDAsIDAsIDApOyAiIHN0eWxlPSJib3gtc2l6aW5nOiBib3JkZXItYm94OyBmb250LXNpemU6IDBweDsgdGV4dC1hbGlnbjogY2VudGVyOyI+PGRpdiBzdHlsZT0iZGlzcGxheTogaW5saW5lLWJsb2NrOyBmb250LXNpemU6IDExcHg7IGZvbnQtZmFtaWx5OiBIZWx2ZXRpY2E7IGNvbG9yOiByZ2IoMCwgMCwgMCk7IGxpbmUtaGVpZ2h0OiAxLjI7IHBvaW50ZXItZXZlbnRzOiBhbGw7IHdoaXRlLXNwYWNlOiBub3dyYXA7Ij48Zm9udCBzdHlsZT0iZm9udC1zaXplOiAyMHB4OyIgY29sb3I9IiMwMGNjMDAiPjxiPumpl+itieaIkOWKn+aZgjwvYj48L2ZvbnQ+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjQ4MSIgeT0iMTA5NSIgZmlsbD0icmdiKDAsIDAsIDApIiBmb250LWZhbWlseT0iSGVsdmV0aWNhIiBmb250LXNpemU9IjExcHgiIHRleHQtYW5jaG9yPSJtaWRkbGUiPumpl+itieaIkOWKn+aZgjwvdGV4dD48L3N3aXRjaD48L2c+PHJlY3QgeD0iMCIgeT0iMCIgd2lkdGg9IjIwMCIgaGVpZ2h0PSI4MCIgcng9IjEwIiByeT0iMTAiIGZpbGw9Im5vbmUiIHN0cm9rZT0iIzY2NjY2NiIgc3Ryb2tlLWRhc2hhcnJheT0iOCA0IiBwb2ludGVyLWV2ZW50cz0ibm9uZSIvPjxnIHRyYW5zZm9ybT0idHJhbnNsYXRlKC0wLjUgLTAuNSkiPjxzd2l0Y2g+PGZvcmVpZ25PYmplY3QgcG9pbnRlci1ldmVudHM9Im5vbmUiIHdpZHRoPSIxMDAlIiBoZWlnaHQ9IjEwMCUiIHJlcXVpcmVkRmVhdHVyZXM9Imh0dHA6Ly93d3cudzMub3JnL1RSL1NWRzExL2ZlYXR1cmUjRXh0ZW5zaWJpbGl0eSIgc3R5bGU9Im92ZXJmbG93OiB2aXNpYmxlOyB0ZXh0LWFsaWduOiBsZWZ0OyI+PGRpdiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMTk5OS94aHRtbCIgc3R5bGU9ImRpc3BsYXk6IGZsZXg7IGFsaWduLWl0ZW1zOiB1bnNhZmUgZmxleC1lbmQ7IGp1c3RpZnktY29udGVudDogdW5zYWZlIGZsZXgtc3RhcnQ7IHdpZHRoOiAxODJweDsgaGVpZ2h0OiAxcHg7IHBhZGRpbmctdG9wOiA2OXB4OyBtYXJnaW4tbGVmdDogMTBweDsiPjxkaXYgZGF0YS1kcmF3aW8tY29sb3JzPSJjb2xvcjogIzMzMzMzMzsgIiBzdHlsZT0iYm94LXNpemluZzogYm9yZGVyLWJveDsgZm9udC1zaXplOiAwcHg7IHRleHQtYWxpZ246IGxlZnQ7Ij48ZGl2IHN0eWxlPSJkaXNwbGF5OiBpbmxpbmUtYmxvY2s7IGZvbnQtc2l6ZTogMTFweDsgZm9udC1mYW1pbHk6IEhlbHZldGljYTsgY29sb3I6IHJnYig1MSwgNTEsIDUxKTsgbGluZS1oZWlnaHQ6IDEuMjsgcG9pbnRlci1ldmVudHM6IG5vbmU7IHdoaXRlLXNwYWNlOiBub3JtYWw7IG92ZXJmbG93LXdyYXA6IG5vcm1hbDsiPjxmb250IHN0eWxlPSJmb250LXNpemU6IDE2cHgiPjxiPjxkaXYgc3R5bGU9InRleHQtYWxpZ246IGxlZnQiPkpXVCDpqZforYnmtYHnqIs8L2Rpdj48L2I+PC9mb250PjxkaXYgc3R5bGU9InRleHQtYWxpZ246IGxlZnQiPltwYXNzcG9ydC1qd3TjgIFrb2EtcGFzc3BvcnRdPC9kaXY+PC9kaXY+PC9kaXY+PC9kaXY+PC9mb3JlaWduT2JqZWN0Pjx0ZXh0IHg9IjEwIiB5PSI2OSIgZmlsbD0iIzMzMzMzMyIgZm9udC1mYW1pbHk9IkhlbHZldGljYSIgZm9udC1zaXplPSIxMXB4Ij5KV1Qg6amX6K2J5rWB56iLLi4uPC90ZXh0Pjwvc3dpdGNoPjwvZz48L2c+PHN3aXRjaD48ZyByZXF1aXJlZEZlYXR1cmVzPSJodHRwOi8vd3d3LnczLm9yZy9UUi9TVkcxMS9mZWF0dXJlI0V4dGVuc2liaWxpdHkiLz48YSB0cmFuc2Zvcm09InRyYW5zbGF0ZSgwLC01KSIgeGxpbms6aHJlZj0iaHR0cHM6Ly93d3cuZGlhZ3JhbXMubmV0L2RvYy9mYXEvc3ZnLWV4cG9ydC10ZXh0LXByb2JsZW1zIiB0YXJnZXQ9Il9ibGFuayI+PHRleHQgdGV4dC1hbmNob3I9Im1pZGRsZSIgZm9udC1zaXplPSIxMHB4IiB4PSI1MCUiIHk9IjEwMCUiPlRleHQgaXMgbm90IFNWRyAtIGNhbm5vdCBkaXNwbGF5PC90ZXh0PjwvYT48L3N3aXRjaD48L3N2Zz4=
```

## passport-jwt
> - A Passport strategy for authenticating with a JSON Web Token
> - This module lets you authenticate endpoints using a JSON web token. It is intended to be used to secure RESTful endpoints without sessions
>  - JWT 驗證

An example configuration which reads the JWT from the http Authorization header with the scheme 'bearer'.

This code ensures Passport calls the strategy on every authentication request.

The code will execute every time your application calls passport.authenticate('jwt') and as long as the JWT has been signed with the same secret that is held on the server then it will pass when checked. If the JWT is not signed with the same secret held on the server then passport will return 'Unauthorized'.

```javascript
// middleware/passport.js
/**
 * @description passport 的中間件( 解析 token )
 */

const jwtStrategy = require('passport-jwt').Strategy;
const extractJwt = require('passport-jwt').ExtractJwt;
const passport = require('koa-passport');
const prismaClientService = require('../service/prismaClientService');
const opts = {
  jwtFromRequest: extractJwt.fromAuthHeaderAsBearerToken('bearer'),
  secretOrKey: process.env.JWT_PASSPORT_SECRET,
};

// 策略類型
const strategy = new jwtStrategy(opts, async function (jwt_paload, done) {
  try {
    const employeeID = jwt_paload.EmployeeID;
    const whereOpt = {
      EmployeeID: employeeID,
      Active: 1,
    };
    const userInfo = await prismaClientService.prisma.users.findMany({
      select: {
        EmployeeID: true,
      },
      where: whereOpt,
    });
    // 驗證失敗
    if (userInfo.length === 0) {
      return done(null, false);
    }
    // 驗證成功
    return done(null, jwt_paload);
  } catch (error) {
    return done(error, false);
  }
});

// 註冊驗證策略 'jwt'
// passport.use('驗證策略名稱', '想建立的策略類型')
// 註冊驗證策略時，第一個參數可指定註冊驗證策略名稱，passport-jwt 未指定時為 'jwt'
passport.use('jwt', strategy);

module.exports = passport;

```
The JWT authentication strategy is constructed as follows:
```javascript
new JwtStrategy(options, verify)
```
- `options` is an object literal containing options to control how the token is extracted from the request or verified.
 	- `secretOrKey`：必填欄位
  - `jwtFromRequest`：用來代入驗證的函式，找出 JWT（Extractor）的方式
  	- **fromAuthHeaderAsBearerToken()**：從 authorization header 取得 bearer token
  	- fromHeader(header_name)：從指定的 http header name 中找 JWT
    - fromBodyField(field_name)：從 body 的欄位中找 JWT
    - fromUrlQueryParameter(param_name)：從 URL 的 query parameter 中找 JWT
    - fromAuthHeaderWithScheme(auth_scheme)：從 authorization header 中找 JWT
    - fromAuthHeader()：以 scheme 'JWT' 尋找 authorization header（HTTP Header 的寫法要是{Authorization: JWT xxx.yyy.zzz}）
    - fromExtractors([array of extractor functions])：可以用陣列的方式列出所有上述想要使用的方法
- `verify` is a callback function with the parameters **verify(jwt_payload, done)**
	- `jwt_payload` 解碼後的 JWT payload：is an object literal containing the decoded JWT payload.
	- `done` 執行驗證後的結果：is a passport error first callback accepting arguments **done(error, user, info)**
  	- 在 Passport 驗證一個 request 時，它會解析 request 中的登入資訊（credentials），接著以這些 credentials 當作參數來執行 verify callback
		- `error` 錯誤訊息：例如在伺服器端回傳錯誤訊息時，帶入錯誤訊息 err；無錯誤訊息時，則可以帶入 null 取代<error|null>
		- `user` 使用者資料：驗證成功時，帶入使用者資料 user；驗證失敗時，則可以帶入 false 取代
		- `info` 驗證失敗訊息：當驗證失敗時，可以額外補充驗證失敗的原因和資訊
```javascript
// done(<error|null>, <user|false>, {message: 'incorrect reason'})

// credentials 是有效的
// 驗證成功時，提供 Passport 該名使用者資料
return done(null, user)

// credentials 是無效的
// 驗證失敗時，不提供 Passport 任何使用者資料
return done(null, false)

// 驗證失敗時，不提供 Passport 任何使用者資料，但告知失敗原因
return done(null, false, { message: 'Incorrect password.' })

// 當伺服器端產生錯誤訊息時，提供 Passport 錯誤訊息內容，這時候 err 會被設為非 null 的值；如果是驗證失敗（伺服器沒有錯誤）應該則此值要設定成 null
return done(err)
```
## koa-passport
> - Passport middleware for Koa
> - Passport 將身分驗證獨立為「驗證策略」 Strategy，寫好的驗證策略會經由 passport.use 掛鉤於一全域物件，接著由 middleware passport.authenticate 指派使用的驗證策略，因此有關於身份驗證只需經過各自寫好的 passport 模組即可

- 在 app.js 中 import 建立好的 middleware passport ，並初始化
```javascript
// app.js
// import middleware passport
const passport = require('./middleware/passport');
// 初始化 passport
app.use(passport.initialize());
```

- Use **passport.authenticate(strategy, options)** specifying 'jwt' as the strategy
	- `strategy`：使用策略，例如 : 'jwt'
  - `options`：其他參數，{ session: false } 可以設定驗證後是否要存入 session 中
- Securing Routes Individually
- 對單一 router 進行 JWT 驗證
- 設定好的 Passport Strategy 可以直接在 routes 中使用
- 如果認證失敗，Passport 會回傳 401 Unauthorized 的狀態，後續的路由都不會再被處理；如果認證成功，則會觸發 next
```javascript
// routes/api/signin.js
// import
const passport = require('koa-passport');
// 在呼叫 Controller 之前進行 JWT 驗證
/**
 * @description 用戶打卡接口地址( POST api/user/signin )
 */
router.post(
  '/signin',
  // passport as middleware
  // 呼叫策略 'jwt' : JWT 驗證
  passport.authenticate('jwt', { session: false }),
  // 資料校驗
  getValidator(signinValidate),
  // routes handler
  async (ctx, next) => {
    ctx.body = await signinController.signin(ctx, next);
  },
);
```