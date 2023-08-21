---
title: Webhook
description: 
published: true
date: 2023-08-14T09:23:30.909Z
tags: api
editor: markdown
dateCreated: 2023-06-26T01:50:27.132Z
---

# Webhook
- [ ] [EP65: What Is A Webhook](https://blog.bytebytego.com/p/ep65-what-is-a-webhook?utm_source=profile&utm_medium=reader2)
- [ ] [Webhooks 是什麼？](https://matthung0807.blogspot.com/2022/04/what-is-webhooks.html)
- [ ] [A simple explanation of what a Webhook is](https://www.wallarm.com/what/a-simple-explanation-of-what-a-webhook-is)
- [ ] [33 成為看起來很強的後端：什麼是 Webhook？](https://www.youtube.com/watch?v=YpgaK1Ho-lI&ab_channel=Web%E5%AF%A6%E9%A9%97%E5%AE%A4&loop=0)
- [ ] [34 成為看起來很強的後端：更多 Webhook](https://www.youtube.com/watch?v=zhwDNMeo1pc&ab_channel=Web%E5%AF%A6%E9%A9%97%E5%AE%A4&loop=0)

![What is a webhook.png](http://192.168.25.60:8000/files/file_storage/623ab20d.png)

# 關於 Webhook
> 反向 Web API 

## Web API
談到 Webhooks 前通常會先談 Web API。Web API 是網路應用程式（服務端( server )）的資源介面，供客戶端( client )存取所需資源。由 client 發起請求( request )給 server ， server 收到請求才將結果回應( response )給 client ，動作的主動權都是 client 。這樣的機制稱為 HTTP 請求響應模型 ( HTTP Request-Response model )。

```
┌──────────┐                   ┌─┬──────────┐
│         ├──────Request──────►│A│         │
│  Client │                   │P│ Server  │
│         │◄─────Response──────┤I│         │
└──────────┘                   └─┴──────────┘
```

## Webhooks
而 Webhooks 則是由 server 將資料透過 POST 請求發給 client ，而客戶端同樣以 API 接收。（下圖沒畫出 client 的response，事實上也是有的，通常是一個簡單的 200 OK 回應。）

```
┌─────────┬─┐                   ┌────────────┐
│         │ │                  │   Server  │
│         │A│                  ├───────┐    │
│  Client │P│◄─────Request──────┤Webhook│   │
│         │I│                  ├───────┘    │
│         │ │                  │           │
└─────────┴─┘                   └────────────┘
```

那 server 端要怎麼知道 client 的 API 呢？所以使用 webhooks 時 client 端需要向 server 註冊（提供）API 位址，這個動作就稱為 hooks，又稱訂閱( subscribe )。而 server 端需要提供 webhooks 發送請求的資料格式給 client 去設計接受請求的 API 。

所以說到底 Webhooks 算是一種機制，其也是利用 HTTP REST API 來實現。

那為什麼用 Webhooks ？ Webhooks 有什麼好處呢？有了 Webhooks server 端就能即時主動發送更新給 client ，不必由 client 不斷發送請求來詢問是否有更新（又稱輪詢(polling)）。