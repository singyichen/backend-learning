---
title: Stream
description: 
published: true
date: 2023-10-06T00:09:15.813Z
tags: nodejs
editor: markdown
dateCreated: 2023-09-01T02:50:49.816Z
---

# Stream
- [ ] [Day9 NodeJS-Buffer與Stream](https://ithelp.ithome.com.tw/articles/10271443)

![Buffer and Streams.png](http://192.168.25.60:8000/files/file_storage/d18223ca.png)

# Stream 概念
> `Stream` (串流)指的是資料傳遞資料的序列流，用具體一點的物質來說明的話，資料和串流就像河流上漂著的東西和河流，資料會順著串流移動，移動至處理資料的目標，在許多情況下也可能會藉由緩衝先暫存部分順著串流移動的資料，再一併傳遞進行資料處理。

![Stream concept.png](http://192.168.25.60:8000/files/file_storage/42a85a11.png)

在 NodeJS 中的串流，作為 `EventEmitter` 的子類別，繼承了 `on()`、`emit()` 等方法，而串流根據資料流向的方向不同又分成`writable`(寫入)、`readable`(讀取)、`duplex`(讀寫)、`transform`(轉換)等類型，每一個串流的類別都繼承串流的方法。從官方的檔案也可以發現串流的類別是一個抽象/基底類別(abstract/base class)，不會被直接使用，而是透過繼承取用的其屬性及方法，因此必須透過建立新的客製化物件再去繼承基底類別。

![Stream 的原型鏈.png](http://192.168.25.60:8000/files/file_storage/8b7947c0.png)

# Stream 使用
- 製作一份測試用的文字檔案 `data.txt`

```txt
Hello, world!
```

- 由於 `Stream` 要透過客製化的物件進行繼承，在 NodeJS 中的 `fs` 模組提供了建立客製化 `Stream` 的功能，因此需要引用 `fs` 模組，再透過 `createReadStream()` 建立一個新的讀取串流，在讀取資料時，會先以 `Buffer` 的形式暫存。

```js
let fs = require("fs");

let readable = fs.createReadStream(__dirname + "/data.txt");	//__dirname為目前資料夾
``` 

- 使用 `on()` 註冊 `data` 事件，以監聽資料讀取的過程，而綁定該事件內容的函式，加入 `chunk`作為讀取資料時的參數，`chunk` 是串流中傳遞的資料片段，透過印出 `chunk` 可以確認資料讀取的情況。

```js
readable.on("data", function(chunk){
  console.log(chunk);
});
```

```
<Buffer 48 65 6c 6c 6f 2c 20 77 6f 72 6c 64 21>
```

- 若想以字串的形式檢視成果，可以在 `createReadStream()` 函式中加入編碼，就會以字串印出整份資料。

```js
let readable = fs.createReadStream(__dirname + "/data.txt", {encoding: "utf8"});
readable.on("data", function(chunk){
  console.log(chunk);
});
```

```
Hello, world!
```

- Buffer 的大小也可透過 `highWaterMark` 設定，單位為 bytes 。這邊以 32KB 作為例子，另外將印出的資訊改為 `chunk.length` 確認資料是否因 `Buffer` 尺寸變動而分段。

```js
let readable = fs.createReadStream(__dirname + '/data.txt', {
  encoding: 'utf8',
  highWaterMark: 32 * 1024,
});

readable.on('data', function (chunk) {
  console.log(chunk.length);
});
```

```
13
```
