---
title: Function Programming CH1
description: Grokking Simplicity
published: true
date: 2022-06-23T00:35:08.504Z
tags: fp
editor: markdown
dateCreated: 2022-05-16T02:17:47.246Z
---

# CH1 Welcome to Grokking Simplicity
**大綱：介紹本書及函式語言程式設計的概念**
## 1. 什麼是函式語言程式設計?
> What is functional programming?

- a programming paradigm charactrized by the use of mathematical functions and the avoidance of `side effects`
- a programming style that uses only `pure functions` without `side effects`
### Function 主要的作用

### Side effects 額外作用/副作用
> Side effects are any behavior of a function besides the return value

- 副作用是在計算結果的過程中，系統狀態的一種改變，或是外部世界可觀察的交互作用
- 常見的副作用如下：
	發送電郵
	讀一個檔案
	發送 HTTP Request
	修改任何外部變數
  更改檔案系統 Changing the filesystem (fs)
	在資料庫寫入紀錄
	可變資料
	印出至畫面 / log
	取得使用者輸入
	DOM 查詢
	存取系統狀態
  
### Pure Function 純函式
> Pure functions depend only on their arguments and don't have any side effects

- 在相同的輸入值時，需產生相同的輸出 ➔ Same input, Same Output
- Input ➔ Process ➔ Output
- 輸入值Input便是指其參數（Parameter），而輸出值Output自然就是其回傳值
- 不能有語義上可觀察的函數副作用，諸如「觸發事件」，使輸出設備輸出，或更改輸出值以外物件的內容等
- 較為容易理解及控制

**Property：**
- 1). 單一功能
- 2). 可預測性 `Predictable` ：因為是同輸入同輸出的特性，將有利重構 `Refactor`
```javascript
❌  // Not total，測試 count(5) 就 GG
const count = i => {
	if(i===0) return 0;
	if(i===1) return 1;
	if(i===2) return 2
}

✅ // 不管輸入什麼都會有相對應的值
const count = i => {
	if(i===0) return 0;
	if(i===1) return 1;
	if(i===2) return 2
    return 100;
}

const count = i => i
```
- 3). 穩定的 `Deterministic`：
```javascript
❌
var x = 1;
var result = () => x++
result() // 2
result() // 3
result() // 4

✅
const result = y => y + 1;
result(1) // 2
result(1) // 2
result(1) // 2
```
- 4). 可組合性 `Composable` ：在任何地方都可以呼叫，易與其他函數組合
```javascript
function someMath(n) {
  return 6 + (5 * n)
}
function g(n) {
  return n * 5
}
function f(n) {
  return n + 6
}
const z = someMath(x)
// 等同於
const y = g(x)
const z = f(y)
// 能利用組合率
z = f(g(x))
```
- 5). 不變性 `Immutable` ：不改變資料或狀態的形況下，可回傳值
```javascript
// impure
var minimum = 21;

var checkAge = function(age) {
  return age >= minimum;
};

// pure
var checkAge = function(age) {
  var minimum = 21;
  return age >= minimum;
};
```
- 6). 一定要有回傳值 return
- 7). 沒有狀態共享 `Stateless` (Avoid share state)：不依賴任何外部狀態只依賴函式參數
ex : abc三個function，共享同一個x時，可能會改變 x 原本的值，應該用參數的方式傳入，變成區域變數，不會改變到 x 原本的值
```javascript
❌ // impure
var minimum = 21;

var checkAge = function(age) {
  return age >= minimum;
};

✅ // pure
var checkAge = function(age) {
  var minimum = 21;
  return age >= minimum;
};
```
- 8). 沒有任何副作用 No Observable Side-Effects
```javascript
❌
const add = (x, y) => {
    console.log(`Adding ${x} ${y}`)
    return x + y
}

✅
const add = (x, y) => {
    return {result: x + y, log: `Adding ${x} ${y}`}
}
```
### Impure Function 非純函式
- 會產生副作用的函式
### ex：
```javascript
function 內服藥(症狀){
  console.log('肚子痛')
  return `治療${症狀}`;
}
內服藥('流鼻水'); 
// '肚子痛' ➔ 副作用
// '治療流鼻水' ➔ 主要的作用
```

任何在運算的過程中改變了 function 內服藥 以外的變動就是 Side Effects

console.log('肚子痛') 會印在 function 外瀏覽器的 console tab (windows的物件)，所以這就是 Side Effect

### ex：
```javascript
function sum() {
  x += 1;
	return x;
}
let x = 3;
console.log(x); // 3 ➔ x的初始值
sum(); ➔ 會產生副作用
console.log(x); // 4 ➔ 修改了x的值
```

### ex：
```javascript
const fruits = ['pineapple','orange','pawpaw','kiwi']
function removeData(dataArr){
	return dataArr.pop()
}
console.log(fruits); // ['pineapple','orange','pawpaw','kiwi'] ➔ fruits的初始值
removeData(fruits) ➔ 會產生副作用
console.log(x); // ['pineapple','orange','pawpaw'] ➔ 修改了fruits的值
```
### ex：
```javascript
let xs = [1, 2, 3, 4, 5]
// impure
xs.splice(0, 3) // xs -> [1, 2, 3]
xs.splice(0, 3) // xs -> [4, 5]
xs.slice(0, 3)  // xs -> []

// pure
xs.slice(0, 3) // xs -> [1, 2, 3]
xs.slice(0, 3) // xs -> [1, 2, 3]
xs.slice(0, 3) // xs -> [1, 2, 3]
```

## 2. 實際在使用定義時會遇到的問題
> The problems with the definition for pratical use 

對於純理論來說，定義可適用於學術界，但對於軟體開發會遇到以下幾個問題
### 問題1：FP 需要副作用
### 問題2：FP 擅長處理副作用
### 問題3：FP 是實用的

## 3. FP 的定義會使管理者感到疑惑
> The definition of FP confuses managers

當我們在寫一個信件系統時，卻不能發送信件?為什麼?

## 4. 我們將 FP 視為一系列的技巧與概念
> We treat functional programming as a set of skills and concepts

FP的美在於可以處理優越的 `beneficial` 、普遍的 `universal` 程式碼

## 5. 區分功能 `Actions` 、計算 `Calculations` 、資料 `Data`
> Distinguishing actions, calculaions, and data

![FP distinguish action calculations data example.png](http://192.168.25.60:8000/files/file_storage/a22b859a.png)

- 當撰寫FP的工程師看到程式碼，可以迅速的將程式碼分類成三種
功能 `Actions` 、計算 `Calculations` 、資料 `Data`
```javascript
// 個人資訊
{
	"firstname":"Eric",
  "lastname":"Normand"
}

// 發送電郵
sendEmail(to,from,subject,body)

// 處理數字相加的函式
sum(numbers)

// 儲存資料到DB
saveUserDB(user)

// 若傳相同的字串兩次，則回傳相同長度兩次
string_length(str)

// 每次呼叫都可以拿到不同的時間
getCurrentTime()

// 陣列	
[1,10,2,45,3,98]

```
## 6. 撰寫 FP 時要可區分當呼叫程式碼時是在做什麼的
> Functional programmers distinguish code that matters when you call it

![FP distinguish action and not action.png](http://192.168.25.60:8000/files/file_storage/c1d97c3f.png)

動作 `Actions` ：取決於什麼時候被呼叫，或被呼叫了多少次
Actions depend on when they are called or how many times they are called

```javascript
// 發送電郵
sendEmail(to,from,subject,body)

// 儲存資料到DB
saveUserDB(user)

// 每次呼叫都可以拿到不同的時間
getCurrentTime()
```
非動作 `Not Actions` ：不取決於什麼時候被呼叫，隨時都會回傳正確的值，也不限制被呼叫了多少次
```javascript
// 個人資訊
{
	"firstname":"Eric",
  "lastname":"Normand"
}

// 處理數字相加的函式
sum(numbers)

// 若傳相同的字串兩次，則回傳相同長度兩次
string_length(str)

// 陣列	
[1,10,2,45,3,98]
```

## 7. 撰寫 FP 時要區分什麼是不活躍資料/無作用資料 `inert data`
> Functional programmers distinguish inert data from code that does work

![FP distinguish action calculations data.png](http://192.168.25.60:8000/files/file_storage/61055fff.png)

**動作 `Actions` ：** 取決於什麼時候被呼叫，或被呼叫了多少次
**Actions depend on when they are called or how many times they are called**

```javascript
// 發送電郵
sendEmail(to,from,subject,body)

// 儲存資料到DB
saveUserDB(user)

// 每次呼叫都可以拿到不同的時間
getCurrentTime()
```

**計算 `Calculations` ：** 可以被執行的 `executed` ，將輸入值計算完之後再回傳輸出值
**Calculatios are computations from imputs to outputs**

```javascript
// 處理數字相加的函式
sum(numbers)

// 若傳相同的字串兩次，則回傳相同長度兩次
string_length(str)
```

**資料 `Data` ：** 不可以被執行的 `unexecuted` ，紀錄關於事件的詳細的資料
**Data is recorded facts about events**

```javascript
// 個人資訊
{
	"firstname":"Eric",
  "lastname":"Normand"
}

// 陣列	
[1,10,2,45,3,98]
```
一般來說，**資料會給計算使用，計算會給功能使用**

## 8. 撰寫 FP 時要可區分動作、計算、資料
> Functional programmers see actions, calculations, and data

### 案例：一個專案管理的雲端服務

當客戶端將任務標示為完成時，伺服器將會發送電郵進行通知

![FP example cloud service.png](http://192.168.25.60:8000/files/file_storage/b7518aa5.png)

#### Step1：使用者將任務標示為完成
此為一個**動作**，將會觸發一個 UI 事件，取決於被呼叫了多少次

#### Step2：客戶端發送一個訊息伺服器
發送訊息為一個**動作**，訊息本身為資料

#### Step3：伺服器接受到訊息
收到訊息為一個**動作**，取決於發生了多少次

#### Step4：伺服器異動資料庫
改變內部狀態為一個**動作**

#### Step5：伺服器決定要去通知誰 ( deciding )
做決定為一個**計算**，給定相同的輸入值，伺服器會做相同的決定

#### Step6：伺服器發送一個電郵通知 ( carrying out the decision )
發送電郵為一個**動作**，發送兩次跟發送一次不一樣

## 9. FP 程式碼中的三大分類
> The three categories of code in FP

### 動作 Actions
> 取決於什麼時候被呼叫，或被呼叫了多少次

#### ex：
- 隨著時間的推移，安全的改變狀態的工具
- 確保訂單的方法
- 確保動作只執行一次的工具

### 計算 Calculations
> 將輸入值進行計算再輸出，有相同輸入值時會有相同的輸出值，不在乎被呼叫了幾次或什麼時候被呼叫，也可被安全的進行測試

#### ex：
- 靜態分析幫助資料正確性
- 提供給軟體良好的數學工具
- 測試策略
### 資料 Data
> 紀錄事件的事實

#### ex：
- 為了有更有效率探訪的組織資料的方法
- 訓練將資料長久保存
- 一個獲得什麼是重要的事情需使用資料來保存的準則

## 10. 區分動作、計算、資料對我們有什麼幫助?
> How does distinguishing actions, calculations, and data help us?

### 在分散式系統 `distributed systems` 裡 FP 是能起到很大的作用的，而且大部分系統都是用分散式的寫法 

### 三個分散式系統的規則
- 訊息是可以亂序到達的
- 每則訊息到達的次數可以是0,1或多次
- 如果沒有得到回應，會不知道發生什麼事情

## 11. 這本書跟其他 FP 有什麼不一樣?
> Why is this book different from other FP books?

### 本書對軟體工程師很實用
- 很多關於 FP 的討論都很學術性。通常都是研究發展出理論，理論好到可以付諸實現
- 很多 FP 的書籍都是專注在學術性的探討，他們教導遞迴 `recursion` 與傳遞樣式 `continuation-passing`
- 本書不一樣，主要講述專業撰寫 FP 的程式經驗

### 本書使用真實世界的情境
- 你不會在這本書裡看到 Fibonacci 的定義或合併排序法 `merge sort`
- 本書模擬的 `mimic` 情境可能是工作實際上會遇到的
- 我們應用函數的思維到現有的、新的程式與結構

### 本書關注在軟體設計
- 很容易收到一個小問題，而且可以找到一個優雅的解決方式
- 在寫 Fizz Buzz 時不太需要使用到結構，只有當事情比較大時需要用到設計準則
- 很多 FP 的書不太需要使用到軟體設計因為專案都太小
- 在真實的世界，我們需要去建構系統以確保長時間的可維護性
- 本書教授各不同等級的函式設計準則，從一行程式到整個應用

### 本書傳達了 FP 的豐富性
- 從 1950s 開始，FP 就開始累積原理與技術，有很多在計算時改變了，有很多則是經過了時間的考驗
- 本書比起其他相關書籍討論的函式思維更加深入

### 本書跟使用的程式語言無關
- 很多書會介紹特定程式語言的特色，這很常表示當使用者使用了不同語言時無法獲得效益
- 本書使用 Javascript 當作程式範例，但其實 Javascript 不是很適合做 FP
- 然而 Javascript 就是因為他的不完美轉變成一個很好的教材，一個語言的限制會促使我們停下並去思考
- 即使使用 Javascript 當作程式範例，但本書不是一個關於在 Javascript 中的 FP，本書著重在思維而非程式語言
- 不建議使用特定 Javascript 風格去將程式碼寫的清晰易懂
- 使用其他語言 C ,Java ,C# ,C++ 時也是可以遵循

## 12. 什麼是函式思維?
> What is functional thinking?

- 函式思維是撰寫 FP 的軟工用來解決問題的一連串的技術與想法
- 本書致力於引導你在 FP 時透過兩個強大的思維
	(1) 區分動作、計算、資料
	(2) 使用 `First Class` 的抽象概念

### Part1：區分動作、計算、資料
> Distinguishing actions, calculations, and data

- 撰寫 FP 時要能將程式區分出動作、計算、資料
- 這些分類相當於會程式碼是否會難以閱讀、測試與重複使用
- 透過本書的第一部分可以學習到如何從每一部分程式辨認分類，並重構動作為計算，讓動作有更好的運作
- 關於分辨程式碼各層次分類的設計章節，可以使程式碼更好維護、可測試、可重複使用

### Part2：使用 `First Class` 的抽象概念
> Using first-class abstractions

- 各領域的程式開發都會找到一個通用的程序並命名使其可以重複使用
- FP 做同樣的事情，不過通常能藉由傳遞程序給其他程序達到重複使用
- 這可能聽起來很瘋狂，但卻是非常實際的
- 我們將會學習如何實作並避免做得過火
- 我們將會透過兩個常見的設計名為響應式結構 `reactive architecture` 與洋蔥式結構 `onion architecture`

## 13. 本書的基本規則與技巧
> Ground rules for ideas and skills in this book

### 技巧不能基於語言的特色
- 有很多函式程式語言的特色適合撰寫 FP
- 如果不是使用那些語言的話，函式開發的概念可以幫助你

### 技巧必須有即時的實際效益
- FP 在業界與學術界皆有在使用
- 學術界，著重在理論性的部分
- 本書會教授可以使你撰寫出更好程式的技巧
- 比如，能區分出動作可以對於一些 bug 有更佳的理解

### 不論現行的程式碼狀態如何技巧都必須使用
- 有些人可以在實作一些新的專案，可能也還未撰寫任何程式
- 有些人則是工作於有上百行程式的現有系統
- 不論什麼樣的情境本書的概念與技巧都能幫助到你

### 腦力激盪
#### Q：我使用的是物件導向的語言，這本書會有幫助嗎?
#### A：
- 是的，本書會有用的，書裡的原則普遍都能實現，有些甚至跟你熟悉的物件導向設計原則很相近
- 當然可能還是會有不同的地方，有些是來自於函式的觀點
- 函式思維是非常有價值的，不論使用什麼程式語言

#### Q：每次看 FP 都會覺得十分學術性與需要數學基礎，此書也會類似這樣嗎?
#### A： 
- 不是的，FP 很理論的原因是因為計算大多很抽象且很適合在論文中進行分析，但很不幸的是，研究通常都支配著 FP 的議題
- 有很多人使用 FP 在開發產品，我們可以透過看到學術文獻，有很多專業的開發人員使用 FP
- 我們會分享知識給彼此以解決每天發生的問題

#### Q：為什麼使用 Javascript
#### A：
- Javascript 是一個廣為人知且容易取得的語言
- 如果是網頁開發人員應該會至少熟知這一點
- Javascript 的語法對於很多開發人員應該是相當熟悉
- Javascript 有很多 FP 需要的元素，包含函式與一些基礎的資料結構
- 當然，這對於 FP 距離完美還是有點遠，但這些不完美都是促使我們發展 FP 概念的起源
- 學習去實現一個語言的原則通常是因為原生的技能不是很好用或者根本沒有一些技能

#### Q：為什麼現有的 FP 定義不是很足夠?為什麼要使用"函式思維"一詞呢?
#### A：
- 標準定義是發展新學術性理論途徑中佔有一個有用的重要地位
- 這是一個很重要的思考：有什麼事情是我們不允許副作用可以完成的呢?
- 標準定義使得很難去發掘很多隱含假設，本書會優先教授這些假設
- 函式思維與函式開發是滿相似的






