---
title: Buffer
description: 
published: true
date: 2023-09-01T03:51:19.436Z
tags: 
editor: markdown
dateCreated: 2023-09-01T01:50:59.470Z
---

# Buffer
- [ ] [Node.js Buffer(緩衝區)](https://www.runoob.com/nodejs/nodejs-buffer.html)
- [ ] [Node.js | Buffer(緩衝區)](https://morosedog.gitlab.io/nodejs-20200123-Nodejs-11/)
- [ ] [Node.js 中的緩衝區（Buffer）究竟是什麼？)](https://juejin.cn/post/6844903897438371847)
- [ ] [JavaScript學習筆記：你遲早都要懂後端的 — Node.js 的學習筆記vol-4 ( Streams and Buffers 篇)](https://a498390344.medium.com/javascript%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98-%E4%BD%A0%E9%81%B2%E6%97%A9%E9%83%BD%E8%A6%81%E6%87%82%E5%BE%8C%E7%AB%AF%E7%9A%84-node-js-%E7%9A%84%E5%AD%B8%E7%BF%92%E7%AD%86%E8%A8%98vol-4-streams-and-buffers-%E7%AF%87-bc7cc7084183)
- [ ] [Day9 NodeJS-Buffer與Stream](https://ithelp.ithome.com.tw/articles/10271443)


# Buffer 概念
> `Buffer` (緩衝)是在資料移動時，提供資料暫存的記憶體位置，可以暫時的累積資料再繼續傳遞，就像在網路上看影片時，會有灰色區塊的緩衝暫存還沒播放到的影片資料。

在 NodeJS 中的緩衝，是以二進制資料( `binary data` )的形式暫存資料，可以透過函式如： `from` 或 `toString` 進行資料的解碼或編碼，支援的編碼包含常用的 `utf8` 與 `ASCII` 等，可以參考 NodeJS 官網的檔案說明。

![Buffer concept.png](http://192.168.25.60:8000/files/file_storage/b6a09aa3.png)

# Buffer 使用
- 由於 `Buffer` 在資料串流上經常使用，因此 NodeJS 將 `Buffer` 定義為全域參數，不需再另外引用，以 `new Buffer.from(字串,編碼)` 可以直接建立新的 `Buffer` ，在定義新的 `Buffer` 變數時，編碼預設為 `utf8` 。

```js
let buffer = new Buffer.from("Hello");		// 編碼預設為 utf8
```
- 直接印出 buffer 可以確認 buffer 是以二進制資料儲存。

```js
console.log(buffer);
```
```
<Buffer 48 65 6c 6c 6f>
```

- 印出使用 `toString()` 的成果，會將 buffer 轉成字串印出。

```js
console.log(buffer.toString());
```
```
Hello
```

- 透過 `toJSON()` 轉成 JSON 格式物件，buffer 暫存的資料會以 `unicode` 編碼的陣列表現。

```js
console.log(buffer.toJSON());
```
```
{ type: 'Buffer', data: [ 72, 101, 108, 108, 111 ] }
```





