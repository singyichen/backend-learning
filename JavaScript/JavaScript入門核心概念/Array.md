---
title: 4. Array 陣列
description: 
published: true
date: 2022-11-25T08:52:50.648Z
tags: javascript
editor: markdown
dateCreated: 2022-11-25T03:08:58.700Z
---

# JavaScript
- [ ] [Array 陣列](https://javascript.alphacamp.co/array.html)

# Array 陣列
資料其實有很多種型態，接下來要介紹的「陣列（ array ）」可以說是在 JavaScript 世界中處理資料時最常使用的資料格式。

因為在實務上，資料未必單獨出現，反而有很大的機會是同時出現一卡車的資料，這時，如何定義資料格式就是一門藝術了。

我們要介紹如何用陣列（ array ）與物件（ object ）來管理資料。先來認識這兩種資料結構的不同：

| 名稱 | 方法 | 描述 |
|:----:|:----:|:----:|
| Index	| 透過索引（index）管理資料	| 每個資料都有一個數字代表所在位置，有序列性，透過數字對資料進行存取。| 
| Key-Value Pair | 使用 key 管理資料 | 每個資料只會對應到一個 key，透過 key 對資料進行存取的動作。（key 通常是字串） |

在 JavaScript 裡，使用 index 的資料結構叫陣列，使用 key-value pair 的資料結構叫物件。在這個單元裡，我們會先介紹陣列，到下個單元再介紹物件。

陣列像是一個櫃子，有不同的抽屜讓我們存放資料。因此，陣列就是一組變數的集合，如同櫃子一般，每個抽屜都有不同且從 0 開始的編號，後面的櫃子緊接著前面的櫃子。

![變數與資料.png](http://192.168.25.60:8000/files/file_storage/2e982d66.png)

## 宣告陣列
在宣告陣列時，透過中括號 ，將每個位置的內容物放入中括號  內，並用逗號 `,` 隔開，而陣列內的內容可以是數值、字串或者另一個物件。

```javascript
let myFriends = ['派大星', '章魚哥', '小蝸', '蟹老闆', '珊迪']
```
## 取出陣列的內容
你需要利用 index 來取出陣列的內容，取值的方式是在陣列名稱後加上中括號和索引數字。

### 用 push 增加陣列元素
`.push()` 方法能把一個元素推送到陣列的尾端，例如：

```javascript
let myFriends = ['派大星', '章魚哥', '小蝸', '蟹老闆', '珊迪']
myFriends.push('皮老闆')
console.log(myFriends)
```

這個 myFriends 的陣列，就增加了 皮老闆 的名字

如果想要一次增加兩個人，則需要寫成：

```javascript
let myFriends = ['派大星', '章魚哥', '小蝸', '蟹老闆', '珊迪']
myFriends.push('皮老闆', '凱倫')
console.log(myFriends)
```

請注意人名是字串，記得加上單引號

### 用 concat 合併陣列
你也可以使用 `.concat()` 方法，新開一個陣列，並與原始的陣列合併：

```javascript
let myFriends = ['派大星', '章魚哥', '小蝸', '蟹老闆', '珊迪']
let myNewFriends = myFriends.concat(['皮老闆', '凱倫'])
console.log(myNewFriends)
```

相對於 `.push()` 直接修改了 myFriends 這個陣列，注意這裡 `.concat()` 回傳了一個新陣列，我們運用等號把新陣列指派給另一個變數 myNewFriends。myFriends 的內容並沒有改變。

### 用 pop 移除元素
和 `.push()` 相反，如果你不小心錯誤新增了一個人（雖然其實全部都不是人）。可以使用 `.pop()` 來移除它：

```javascript
let myFriends = ['派大星', '章魚哥', '小蝸', '蟹老闆', '珊迪']
myFriends.push('皮老闆')
console.log(myFriends)
myFriends.pop()
console.log(myFriends)
```

`.pop()` 會直接移除陣列中的最後一個元素

## 常見陣列方法
陣列可以用來做非常多的事情。上面是其中一些陣列方法的舉例，整理如下表。未來遇到需要使用的情景，或是想要查找新方法時，可以透過這個表格，找到適合搜尋的關鍵字：

| 方法 | 功能 | 範例 |
|:----:|----|----|
| push | 在陣列最後方加入新元素 | array.push('new element') |
| pop	| 移除最後一位元素 | array.pop() |
| unshift | 在陣列最前方加入新元素 | array.unshift('new element') |
| shifit | 移除第一位元素 | array.shift() |
| length | 計算陣列長度 | array.length |
| concat | 合併兩個陣列 | array.concat([1, 3]) |
| splice | 指定從特定 | index 移除數個元素	let numbers = [1, 3, 5, 7, 9]numbers.splice(3, 1)說明：(3, 1) 的意思表示，移除 numbers[3] 開始的 1 位元素。執行後，陣列中的元素會變成 1, 3, 5, 9（也就是移除了 7） | 
