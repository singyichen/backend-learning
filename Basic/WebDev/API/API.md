---
title: API
description: 
published: true
date: 2023-07-07T00:43:49.571Z
tags: api
editor: markdown
dateCreated: 2022-11-17T05:44:26.014Z
---

# API
- [ ] [API 到底是什麼？ 用白話文帶你認識](https://medium.com/codingbar/api-%E5%88%B0%E5%BA%95%E6%98%AF%E4%BB%80%E9%BA%BC-%E7%94%A8%E7%99%BD%E8%A9%B1%E6%96%87%E5%B8%B6%E4%BD%A0%E8%AA%8D%E8%AD%98-95f65a9cfc33)
- [ ] [什麼是 API（應用程式設計介面）？](https://www.tibco.com/zh-hant/reference-center/what-is-an-api-application-program-interface)
- [ ] [認識 API 與 Web API ，實用的 API 工具](https://tw.alphacamp.co/blog/api-introduction-understand-web-api-http-json)
- [ ] [8.4 帶來無限便利的 API](https://docs.f5ezcode.in/cs-basic/di-ba-zhang-gong-cheng-de-gong-ju/8.4-api)
- [ ] [Comparing API Architectural Styles: SOAP vs REST vs GraphQL vs RPC](https://www.altexsoft.com/blog/soap-vs-rest-vs-graphql-vs-rpc/)
- [ ] [SOAP vs REST vs GraphQL vs RPC](https://blog.bytebytego.com/p/soap-vs-rest-vs-graphql-vs-rpc?utm_source=profile&utm_medium=reader2)
- [ ] [The 2022 API Platform Landscape: Trends and Challenges by Postman](https://blog.postman.com/2022-api-platform-landscape-trends-and-challenges/)
- [ ] [Code First v.s. API First - A change of software development philosophy](https://blog.bytebytego.com/p/twitter-architecture-2022-vs-2012?utm_source=profile&utm_medium=reader2)
- [ ] [What are the API architectural styles?](https://blog.bytebytego.com/p/ep49-api-architectural-styles?utm_source=profile&utm_medium=reader2)
- [ ] [Top 6 Most Popular API Architecture Styles](https://www.youtube.com/watch?v=4vLxWqE94l4&ab_channel=ByteByteGo&loop=0)

# API ( Application Programming Interface ) 應用程式介面
> - API（應用程式設計介面）是一種軟體介面，允許兩個不相關的應用程式相互通訊，它充當橋樑，從一個程式取得請求或訊息後，將其傳遞給另一個程式、翻譯訊息，及根據 API 程式設定的協議來執行指定動作
> - API 的工作是讓 AB 兩端資料拋接，一端提供資料輸入、另一端依據資料回傳結果，其目的在於「不必了解對方的技術與邏輯即可加速開發的共識」
> - 透過 API，可以讓雙方交換資料

![API.png](http://192.168.25.60:8000/files/file_storage/7f2f72fa.png)

## 兩個主體溝通的交接點——介面
在認識 API 以前，讓我們來認識介面 (Interface)。

「介面」是兩個主體溝通的交接點：

![API 介面.png](http://192.168.25.60:8000/files/file_storage/efd5a1b4.png)


「人」作為一個主體，如果人要去使用機械、物品或是應用軟體，人無法直接和這些東西進行互動，因此需要有一個「介面」，來讓兩個主體之間可以溝通。

例如，如果今天「人」想要請收音機播放自己想聽的電台節目，可以透過收音機上的調頻按鈕來設定頻道，讓收音機播送電台節目；或者，如果今天「人」想要請咖啡機泡咖啡，人可以透過咖啡機的控制面板來啟動機器的泡咖啡程序。

![API 收音機的調頻按鈕和咖啡機的控制面板即為人機互動的介面.png](http://192.168.25.60:8000/files/file_storage/0821d29e.png)

在上述例子裡，收音機的調頻按鈕和咖啡機的控制面板即為人機互動的「介面」。

如果今天人要和應用軟體互動，例如 Google Maps，Google Maps 系統裡儲存著大量的地理資訊，可以供人查詢。而人需要在網頁上的輸入框輸入地址，系統才能得知人想要查詢的地址，因而進入資料庫搜尋，並將結果回傳到網頁上讓人閱讀。

![API Google Maps 系統.png](http://192.168.25.60:8000/files/file_storage/b23cb109.png)

在這個例子中，Google Maps 的前端網頁扮演了介面的角色。

## Web API 與 HTTP

在 Web Application 的開發情境下的 API 被稱為 Web API，在 Web API 作用時，客戶端和伺服器端會透過 HTTP 通訊協定來進行請求與回應。

![API HTTP Request Response.png](http://192.168.25.60:8000/files/file_storage/69e5f5ad.png)

使用 Web API 的一方是客戶端，可能是瀏覽器、手機或者穿戴式設備等等，客戶端會向伺服器端發出請求，要求執行某個 CRUD 動作。

伺服器端會依照客戶端的請求內容，經過內部的處理程序後，將結果回應給客戶端。

你有可能扮演客戶端的一方，也有可能是伺服器端。

## Web API 與你
### 客戶端：串接他人開發的 Web API
作為開發者，你會常常需要使用第三方開發的 Web API：

![API 串接他人開發的 Web API.png](http://192.168.25.60:8000/files/file_storage/f2e169a7.png)


這時候，做為客戶端，你會向 Web API 伺服器發送請求。

舉個例子，假設你現在是個開發餐廳網頁的工程師，你需要在餐廳網頁上放入 Google Maps 地圖資料，你會經歷以下步驟：

- 登入 Google Map API 的官方網站，找到你要使用的 API。
- 閲讀 Google Map API 的文件，了解如何使用 Google Maps API。
- 註冊 Google Maps API 帳號，並且取得金鑰 ( Key ) 通過驗證。
- 取得金鑰 ( Key ) 後，就可以將 Google Maps API 提供的 API 放進自己的專案，API 的呈現會是一組程式碼，你需要自行改寫其中的 Key 和地址。
- 完成程式碼改寫之後，就可以在網頁中讀取 Google Maps 的地圖了。

![API Google Map API.png](http://192.168.25.60:8000/files/file_storage/a024b902.png)

餐廳網頁作為一個客戶端，可以透過 Google Maps 提供的 Web API，從 Google Maps 的資料庫取得自家餐廳所在位置的地圖，傳遞到網頁裡展示。

### 伺服器端：開發 Web API 給他人使用
你也有可能做為伺服器端，將你擁有的資料庫開放給第三方使用：

![API 開發 Web API 給他人使用.png](http://192.168.25.60:8000/files/file_storage/02b55a16.png)

延續著 Google Maps API 的例子，這次我們想像自己是 Google Maps 的開發團隊，我們決定將手上龐大的地圖資料 OpenSource，意即我們將開放 API 供他人使用，步驟可能會是這樣：

撰寫一個明確的 API 文件，清楚定義 API 的使用方式及格式等等。
按照文件開發一個 Web API，也就是在 Google Maps 系統上開發可供外部呼叫的方法 ( method )，用這個方式做為使用介面。

![API 文件.png](http://192.168.25.60:8000/files/file_storage/b0a51837.png)

以上是 Web API 的初步說明，為了建立實際的體會，接下來教案會帶你實際去使用他人的 Web API，並為自己的專案開發 Web API。

## HTTP 通訊協定
HTTP ( HyperText Transfer Protocol ) 是通訊協定 ( Transfer Protocol ) 的一種，通訊協定制定了規則和標準，讓設備之間不但能認出彼此，也能夠根據不同情境選擇溝通的方式，而 HTTP 是專門用於瀏覽器與伺服器之間的通訊協定。

### URL + Parameter
你需要先知道正確的 HTTP Verb 和 URL，才能使用正確的動詞對 URL 發出請求。

![HTTP Verb 和 URL.png](http://192.168.25.60:8000/files/file_storage/8f60cd4a.png)

### Request Format
根據 HTTP 規範，伺服器端和客戶端進行請求與回應時，會使用定義明確的請求格式 ( request format ) 和回應格式 ( response format )。

Request format 指的是，客戶端需要使用特定的 HTTP 動詞，對 URL 發出請求。Rails 採用的 RESTful 風格就是一種 request format。

### Response Format
Response format 是指伺服器端回應時採用的資料格式，可能會是：XML、JSON、Protocol Buffers、Thrift、YAML 等等。

JSON 是最常用的回應格式之一，各種程式語言都可以產生或解析 JSON 字串

運用 HTTP 來表達語義的路由設計風格稱為 REST API。

# API Architectural Styles

![API Architectural Styles Comparison.png](http://192.168.25.60:8000/files/file_storage/79773843.png)

![API architectural styles.png](http://192.168.25.60:8000/files/file_storage/3e0aa892.png)

![API Architecture Styles.png](http://192.168.25.60:8000/files/file_storage/641bccf0.png)

![RPC and RESTful.png](http://192.168.25.60:8000/files/file_storage/bec401cf.png)

## REST
Proposed in 2000, REST is the most used style. It is often used between front-end clients and back-end services. It is compliant with 6 architectural constraints. The payload format can be JSON, XML, HTML, or plain text.

## GraphQL
GraphQL was proposed in 2015 by Meta. It provides a schema and type system, suitable for complex systems where the relationships between entities are graph-like. For example, in the diagram below, GraphQL can retrieve user and order information in one call, while in REST this needs multiple calls.

GraphQL is not a replacement for REST. It can be built upon existing REST services.

## Web socket
Web socket is a protocol that provides full-duplex communications over TCP. The clients establish web sockets to receive real-time updates from the back-end services. Unlike REST, which always “pulls” data, web socket enables data to be “pushed”.

## Webhook
Webhooks are usually used by third-party asynchronous API calls. In the diagram below, for example, we use Stripe or Paypal for payment channels and register a webhook for payment results. When a third-party payment service is done, it notifies the payment service if the payment is successful or failed. Webhook calls are usually part of the system’s state machine.

## gRPC
Released in 2016, gRPC is used for communications among microservices. gRPC library handles encoding/decoding and data transmission.

## SOAP
SOAP stands for Simple Object Access Protocol. Its payload is XML only, suitable for communications between internal systems.

# API Platform Landscape

![2022-postman-api-platform-landscape-pm.png](http://192.168.25.60:8000/files/file_storage/d7b47252.png)

