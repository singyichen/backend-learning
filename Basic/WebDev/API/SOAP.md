---
title: SOAP
description: 
published: true
date: 2023-05-19T01:50:50.703Z
tags: soap
editor: markdown
dateCreated: 2022-11-21T05:58:05.320Z
---

# SOAP
- [ ] [node-soap](https://github.com/vpulim/node-soap)
- [ ] [透過網路交換資料的第一種方式：SOAP](https://saffranblog.coderbridge.io/2021/02/06/soap-and-restful-api/)
- [ ] [reqres.in](https://reqres.in/)
- [ ] [pokeapi](https://pokeapi.co/)

# SOAP ( Simple Object Access Protocol )
> - SOAP 是一個協定，定義了另一種跟 server 溝通的方式
> - SOAP 的資料交換（request , response）都是透過 XML 的格式

因為 XML 的格式寫起來比較麻煩，因此都會透過一套 Node.js 的 library 叫做 node-soap 讓我們更方便地去寫 request

例如：
先建立一個 server，在 server 提供一個 function 叫做 myFunction

client 端連接到 server 後，就可以用 client.MyFunction() 直接呼叫這個 myFunction 函式，這個 function 就是「server 提供給我們呼叫的 function」

注意！底層還是一樣是用 XML 在溝通（也就是說，myFunction 會使用 XML 的格式，帶入一些參數）

```javascript
var soap = require('soap');
  var url = 'http://example.com/wsdl?wsdl';
  var args = {name: 'value'};
  soap.createClient(url, function(err, client) {
      client.MyFunction(args, function(err, result) {
          console.log(result);
      });
  });
```

# SOAP 以外的 HTTP API
什麼叫做「其他的」API？

HTTP API 主要分成兩類：

- 第一類：SOAP
- 第二類：SOAP 以外的 HTTP API（佔大多數）

「SOAP 以外的 HTTP API」交換資料的方式是：
發送 request 到一個 API 網址，然後就會回傳一個 JSON 格式的 response
例如：PokéAPI、reqres

# RESTful 
RESTful 到底是什麼？

RESTful 不是一個協定，它只是一種「風格」（建議你遵循 RESTful 這種風格，但不強制）

RESTful 風格就是：希望你可以好好的去使用這些 HTTP 的 methods

## 沒有遵循 RESTful 的 API
下圖中的 endpoints （也就是最右邊那欄）就是"沒有遵循" RESTful 風格的

![沒有遵循 RESTful 的 API.png](http://192.168.25.60:8000/files/file_storage/36ac02f9.png)

可以看到在上圖的 API，要「刪除使用者」的話，是使用 POST 發送 request 到 /delete_user 這個網址去

這裡你可能會疑問的是：刪除不是要用 DELETE 嗎？

要注意的是：
「刪除」用 DELETE，是一個「良好的習慣」，並不是一個「規範」

只要後端 server 有處理好這件事情，用 POST 一樣也可以模擬「刪除」的功能

因為後端 server 也有 /delete_user 這個網址，所以，就算我是使用 POST，後端也會知道「所有被送到 /delete_user 這個網址的 request 都是要“刪除使用者”的」，並不會跟「新增使用者的 POST」搞混

但是，「不遵循良好習慣」的壞處是：

### 壞處一：每個工程師都會有不同的命名方式
例如：
「新增使用者」這個功能，有人取名為 /new_user，另一個人取名為 /create_user，另一個人取名為 /create_new_user

### 壞處二：還是可能會造成混淆，因為用 POST 來刪除是「不具有語意的」
雖然說使用 POST 一樣也可以做到「刪除」的功能，但是「用 DELETE 來刪除」才是具有語意的作法

就像在寫 html 時，所有的標籤都可以用 `<div>` , `<span>` 來寫，但這樣是沒有語意的。應該要使用 `<session>` , `<article>` 這些有語意的標籤比較好

## 有遵循 RESTful 的 API
下圖中的 endpoints （也就是最右邊那欄）就是有遵循 RESTful 風格的

RESTful 針對「API 網址、HTTP 動詞（POST, GET, DELETE 這些）」都有一個建議的規範。只要有遵循 RESTful 風格，就可以被稱為是 RESTful API
- 「新增」就用 POST
- 「刪除」就用 DELETE
- 主要的 API 網址會是一個「資源名稱」（會使用複數）：/users
- 「刪除、查詢、更改」某個特定的使用者：/users 後面加上 :id

![有遵循 RESTful 的 API.png](http://192.168.25.60:8000/files/file_storage/5ba6ed74.png)

例如：
[reqres](https://reqres.in/) 就是一個 RESTful API

## SOAP REST 比較
| SOAP |	REST |
|  --- |  ---  |
|SOAP 是一種傳輸協議 |	REST 是一種架構風格|
|SOAP 不能與 REST 交互，因為 SOAP 是一種協議	| REST 可以使用 SOAP Web Service ，因為 REST 是一種架構風格，可以使用任何的 Web 協議，如 HTTP 、 SOAP |
|SOAP 只允許使用 XML 數據格式	| REST 允許使用多種數據格式，如 JSON 、文本、 HTML 和 XML 等 |
|SOAP 需要佔用的帶寬更多 |	REST 需要的帶寬資源較少|
|SOAP 比 REST 更可靠	| REST 安全性沒有 SOAP 高|
|SOAP 比 REST 慢|	REST 比 SOAP 快|
|SOAP 可以自行實施對數據安全控制,自由度更高|	REST 嚴重依賴傳輸層的安全控制|
|SOAP 支持 SMTP 和 HTTP 協議	| REST 只需要使用 HTTP 協議|


# 透過網路交換資料的第 n 種方式：跳脫 HTTP 的限制
## 自定義資料交換
其實，有很多 API 是沒有建立在 HTTP 之上的，而是建立在其他的 protocol 上，並使用不同的資料格式來交換資料