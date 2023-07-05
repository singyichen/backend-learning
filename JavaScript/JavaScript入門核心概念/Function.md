---
title: 8. Function 函式
description: 
published: true
date: 2022-11-30T01:29:11.728Z
tags: javascript
editor: markdown
dateCreated: 2022-11-30T00:08:16.669Z
---

# JavaScript
- [ ] [Function 函式](https://javascript.alphacamp.co/function.html)

# Function 函式
函式最基本的用途，是把會重覆使用的程式碼封裝起來，以便可以在需要的時候拿出來使用。

JavaScript 的函式是一個非常重要的學習主題，大部分的程式語言都有這種「封裝程式碼」的函式功能，但 JavaScript 的函式比其他的程式語言來得強大。

- 一般的函式特性：
	-	用來封裝程式碼
	- 有需要時才呼叫
- JavaScript 特殊的函式特性：
	- 函式可以當成值來傳遞 (可以放進變數或放進物件)
	- 函式可以當成函式的參數
	- callback - 在特定事件中觸發函式 (非同步特性)
  
接下來我們將依序討論這些主題。

## 基本結構
以下是一個函式的基本寫法：

![函式的基本寫法.png](http://192.168.25.60:8000/files/file_storage/db08589e.png)

## 調用 ( invoke ) 函式
當我們想使用函式時，會「調用 ( invoke )」函式，形式如下：

```javascript
subtotal(3, 8)
```

「invoke (調用)」和「call (呼叫)」是一樣的，你可以開始使用這個動詞，聽起來更厲害。

請注意，你需要使用 `()` 來調用函式，如果只輸入函式本身，只是單純地回傳函式本身。

![調用 (invoke) 函式.png](http://192.168.25.60:8000/files/file_storage/853ea448.png)

當你使用 `subtotal(3, 8)` 調用函式時，`3, 8` 被稱為「引數 (argument)」。引數 `3, 8` 會為 `price`, `quantity` 等參數賦值。

## 括號的意義：執行函式
函式不一定需要傳入參數：

```javascript
function greeting() {
  console.log('Hi!')
}
greeting()
```

請注意，即使沒有參數，在呼叫函式時，還是不能省略括號。

你也可以指定參數的預設值：

```javascript
function multiply(x, y = 2) {
  return x * y
}

console.log(multiply(10))     // 20
console.log(multiply(10, 3))  // 30
```

指定了預設值之後，`multiply(10)` 雖然只傳入第一個參數，但還是會變成 `10 * 2`；而 `multiply(10, 3)` 則會去覆蓋掉原本的預設值。

有預設值的參數請放在後面，這樣才能優先傳入必須要指定的參數。

## 回傳值 ( return value )
所有的函式都有回傳值，回傳值可以用關鍵字 `return` 來指定：

![所有的函式都有回傳值，回傳值可以用關鍵字 return 來指定.png](http://192.168.25.60:8000/files/file_storage/406830c2.png)

如果沒有特別指定，回傳值一律會是 `undefined`：

![如果沒有特別指定，回傳值一律會是 undefined.png](http://192.168.25.60:8000/files/file_storage/cb9b5a73.png)


## 用函式封裝程式碼
函式最重要的功能之一是封裝程式碼，把具有特定功能的程式碼移出主程式，另外獨立管理。

在之前的作業中，我們曾經寫過一個「產生彩券編號」的小程式，你可能會寫出以下的程式碼：

![產生彩券編號的小程式.png](http://192.168.25.60:8000/files/file_storage/499017ad.png)

但這樣每次執行程式，都只能獲得一組彩券編號，這不是太蝦了嗎？

所以想當然爾，我們會用函式把這組程式碼封裝起來，這樣看起來聰明多了：

![產生彩券編號的小程式用函式封裝.png](http://192.168.25.60:8000/files/file_storage/43bcfa4e.png)

當你使用封裝好的 `generateTicket` 時，你不需要在意產生彩券編號的邏輯是什麼，你可以把這組函式丟到程式碼的其他地方，讓你的主程式乾乾淨淨。








