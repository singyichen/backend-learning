---
title: Pipe
description: 
published: true
date: 2023-09-01T03:52:07.223Z
tags: nodejs
editor: markdown
dateCreated: 2023-09-01T03:26:57.249Z
---

# Pipe
- [ ] [Day10 NodeJS-Pipe](https://ithelp.ithome.com.tw/articles/10272140)


# Pipe 概念
> 藉由從一個串流讀取資料並寫入另一個串流以連結兩個串流的概念就是 `Pipe` ，在 NodeJS 中可以透過將讀取串流 `Pipe` 到寫入串流，讓讀取串流上的 `Chunk` (資料片段)可以連結到寫入串流並輸出，也可以透過讀寫字串進行多次的 `Pipe` 。

![Pipe concept.png](http://192.168.25.60:8000/files/file_storage/d8163b89.png)

# 從程式碼來理解
`Pipe` 的作用其實就是把讀取串流讀取的資料片段交給寫入串流輸出，如果不使用 `Pipe` ，可以透過讀取串流註冊的 `data` 事件讀取資料片段後，再由寫入串流的 `write` 輸出資料到檔案，下面分別為不使用 `Pipe` 與運用 `Pipe` 函式的程式碼。

## 不使用 Pipe

```js
let fs = require("fs");

// 建立讀取串流與寫入串流
let readable = fs.createReadStream(__dirname + "/data.txt");
let writable = fs.createWriteStream(__dirname + "/copy.txt");

// 註冊 data 事件
readable.on("data", function(chunk){
  console.log(chunk);
  // 將 chunk 寫入目標檔案
  writable.write(chunk);
});
```

## 使用 Pipe

```js
let fs = require("fs");

let readable = fs.createReadStream(__dirname + "/data.txt");
let writable = fs.createWriteStream(__dirname + "/copy.txt");

readable.pipe(writable);
```

所以 `pipe()` 函式的概念可以從程式碼中更清楚的理解，而透過 NodeJS 中建立好的 `pipe()` 又可以再次的讓程式碼變的簡潔。