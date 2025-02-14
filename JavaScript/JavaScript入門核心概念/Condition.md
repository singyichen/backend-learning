---
title: 6. 流程控制 if/else 條件判斷
description: 
published: true
date: 2022-11-29T07:37:34.170Z
tags: javascript
editor: markdown
dateCreated: 2022-11-29T01:58:20.047Z
---

# JavaScript
- [ ] [流程控制 if/else 條件判斷](https://javascript.alphacamp.co/condition.html)

# 流程控制 if/else 條件判斷
認識完 JavaScript 的基本元素後，我們接下來要學習怎麼控制流程。

這裡介紹三種 JavaScript 控制流程的方式：

- `if` / `else`
- `switch case`
- 三元運算子

首先，讓我們先來學習如何利用 `if` / `else` 判斷式來進行流程控制。

## 使用 if / else 進行流程控制
JavaScript 的流程控制中常常會用到 `if` 判斷式，一個 `if` 的語法的結構，除了關鍵字 `if` 、小括號 `( )`、和大括號 `{ }` 外，還有以下兩個要素：

- 條件 ( condition )：判斷式（如 `number == 5`）。
- 執行步驟 ( statements )：對應結果所要執行的程式碼（如 `console.log("X is 5")`）。

`if` 可以搭配 `else`、`else if` 使用，然而在 `if` / `else` 流程控制小括號裡條件成立下，大括號的程式才會被執行；我們將透過一些範例來仔細理解這些判斷式用法。

### if … 判斷式
`if` 就是如果的意思，如果判斷式的結果為 true，則執行大括號裡的程式碼。

以下為範例程式碼：

```javascript
let number = 2;
if (number < 5) {
  console.log("Number is smaller than 5");
}
```

### if … else … 二分法判斷式：A 或非 A
如果條件為 `true`（A），則印出：number is 5，否則就執行 `else` （非A）大括號裡的程式碼，印出 number is not 5。

以下為範例程式碼：

```javascript
if (number === 5) {
  console.log("number is 5");
} else {
  console.log("number is not 5");
}
```

### if … else if … 判斷兩種情況：A 或 B
需要判斷的條件若超過二種或以上，也可以使用 `else if` 來增加條件判斷。以下為範例程式碼：

```javascript
if (number > 5) {
  console.log("number is bigger than 5");
} else if (number < 5) {
  console.log("number is smaller 4");
}
```


### if … else if … else 三分法判斷式：A 或 B 或「非A非B」
使用 `if else if` 和 `else` 來組合出更多的判斷，幫助我們進行流程控制。

當 `if` 的結果為 false 就繼續判斷 `else if`，判斷的結果再為 false 就繼續判斷 `else`。

以下為範例程式碼：

```javascript
if (number > 5) {
  console.log("number is bigger than 5");
} else if (number < 5) {
  console.log("number is smaller 4");
} else (number === 5) {
  console.log("number is equal to 5");
}
```

### 多個 else if 參雜其中的邏輯判斷式
也可以使用多個 `else if` 語法在 `if` 和 `else` 的中間，進行更多種情況的判斷。

以下為範例程式碼：

```javascript
if (number === 5) {
  console.log("number is 5");
} else if (number === 4) {
  console.log("number is 4");
} else if (number === 3) {
  console.log("number is 3");
} else if (number === 2) {
  console.log("number is 2");
} else if (number === 1) {
  console.log("number is 1");
} else {
  console.log("number is not 1 to 5");
}
```

## 使用 switch case 進行流程控制
當 `if` / `else` 的條件太多時，我們可以使用 `case` 來敘述表達一連串的條件判斷與回應。在 `switch case` 的結構裡有以下三個要素：

- 變數 ( variable )：用途為判斷（例如 number）。
- 變數的值 ( value )：用途為比較變數（例如 case 右邊的 5）。
- 程式碼 ( statements )：根據不同變數的值所要執行的程式碼（例如 `console.log("number is 5 ");`）。

接著，我們要特別介紹 `break` 這個語法，由於 `switch case` 不同於 `if else` 語法，沒有大括號限制「執行步驟 (  statement )」的範圍，所以如果我們不希望繼續執行下一個 `case` 的判斷，我們就可以在 `statement` 後加上 `break` 來結束整個 `switch case`，如果沒有退出，則會往下個 `case` 進行判斷。

然而，如果程式都找不到適合的 `case` 數值，我們可以設計 `default` 來判斷其餘的所有狀況， `default` 為預設語法，其效果和 `else` 一樣，用來判斷不屬於以上所有可能性的數字。

以下為範例程式碼：

```javascript
switch (number) {
  case 5:
    console.log("number is 5 ");
    break;
  case 4:
    console.log("number is 4 ");
    break;
  case 3:
    console.log("number is 3 ");
    break;
  case 2:
    console.log("number is 2 ");
    break;
  case 1:
    console.log("number is 1 ");
    break;
  default:
    console.log("number is not 1 to 5");
}
```

接下來，我們要認識一個特別的流程控制判斷式！

## 使用三元運算子進行流程控制
我們先來看看三元運算子的語法結構，有以下兩個要素：

- 條件 ( condition )：問號 `?` 左邊的邏輯判斷式（例如 `x > 2`）。
- 程式碼 ( statements )：使用冒號 `:` 區隔邏輯判斷式結果為 true 和 false 各要執行的程式碼。

當只有兩種相反的結果時，我們可以用問號 `?` 進行二分法判斷。

例如剛剛看過的例子：

```javascript
if (number > 5) {
  console.log("number is bigger than 5");
} else if (number < 5) {
  console.log("number is smaller 4");
}
```

可以改寫成：

```javascript
let number = 3;
number > 5 ? console.log("number is bigger than 5") : console.log("number is smaller 5");
//印出 “number is smaller 4”
```

上面這段程式碼翻成白話文就是：如果數字大於五，就幫我印 " number is bigger than 5 " ，否則，就幫我印 " number is smaller 5 " 。

五行程式碼瞬間變成一行，是不是很簡潔呢？

學習如何使用判斷式進行流程控制後，我們要來學習使用迴圈來重複執行程式！
