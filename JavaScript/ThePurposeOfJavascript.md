---
title: JavaScript 的用途
description: 
published: true
date: 2023-05-17T02:04:32.010Z
tags: javascript
editor: markdown
dateCreated: 2022-11-24T03:04:44.622Z
---

# JavaScript
- [ ] [JavaScript 的用途](https://javascript.alphacamp.co/the-purpose-of-js.html)


# JavaScript 的用途
JavaScript 不僅能夠作用在前端，隨著 Node.js 的問世， JavaScript 這個為了網頁而生的語言也可以撰寫後端伺服器上的應用程式。

另外，JavaScript 不止用於開發網頁，框架如 React Native 就允許開發者用 JavaScript 開發 Android 和 iOS 的 mobile app；而 electron js 則可以用來開發 window 和 macOS 的應用程式。

# JavaScript 可以做到什麼？

讓我們一起來看看 JavaScript 可以做出哪些有趣的互動與作品：

## 常見的輸入表單

![常見的輸入表單.png](http://192.168.25.60:8000/files/file_storage/f24687bd.gif)

比如這個，這是一個常見的輸入表單，讓使用者填入 Email 以及密碼。

通常這種形式的填答格式都相當無趣，頂多在你輸入錯誤 Email 格式時跳出幾個提示訊息。但 [這個作品](https://codepen.io/dsenneff/full/QajVxO/2d338b0adf97472ebc5d473cf1fa910b) 很細心地在使用者體驗（user experience）上下了不少功夫，除了使用者在輸入 Email 時，上方的猩猩會跟著轉頭；輸入密碼時，更會做出把眼睛遮住的動作。

比起一般的表單，這樣的呈現手法顯得創意十足。

最後再舉兩個日常生活中常見的功能，也是用 JavaScript 來實現的：

## 即時檢查名稱功能
我們在網站上註冊新帳號時，常會需要確認輸入的名稱是否已經被他人使用過。

以在 GitHub 註冊新的專案為例，如果輸入的專案名稱沒有被使用過，會直接在畫面上出現一個綠色的勾勾，表示這個名稱沒人使用，可用來登記，這也是使用 JavaScript 開發的即時檢查名稱功能。


![即時檢查名稱功能.png](http://192.168.25.60:8000/files/file_storage/037270ce.gif)


## Single Page Application
你是否曾經留意過，使用 Gmail 點擊「撰寫 ( Compose )」 時，我們可以在不刷新畫面的情況下，在右下角展開一個新視窗撰寫新郵件，完全沒有換頁。全部的動作都是在「同一個頁面」上面發生的，所以你載入的檔案從頭到尾就只有一個 index.html，完全沒有換過。

![Single Page Application.png](http://192.168.25.60:8000/files/file_storage/ad86622b.gif)

這種高度的網頁互動性是透過 Ajax ( Asynchronous JavaScript and XML ) 技術來達成的。而這種「不跳頁」的設計原則稱為 Single Page Application，簡稱 SPA。