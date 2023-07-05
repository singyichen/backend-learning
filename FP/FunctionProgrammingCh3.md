---
title: Function Programming CH3
description: Grokking Simplicity
published: true
date: 2022-06-29T02:29:20.861Z
tags: fp
editor: markdown
dateCreated: 2022-06-23T00:34:41.117Z
---

# CH3 Distinguishing actions, calculations, and data
**大綱：學習如何應用三個分類在實際生活的程式碼中**
> - 學習動作、計算、資料的差異
> - 當遇到問題或撰寫程式碼或閱讀現有的程式碼時，可以區分出動作、計算、資料
> - 當動作佈及程式碼時要如何追蹤
> - 可以在現有的程式碼中點出動作

## 1. 動作、計算、資料
> Actions, calculations, and data (ACD)

### 動作 `Actions`
> - 取決於什麼時候被呼叫，或被呼叫了多少次
> - 稱為：有副作用的函式 `functions with side-effects` `side-effecting functions`、非純函式 `impure function`
#### ex：
- 發送電郵
- 從資料庫讀取資料

### 計算 `Calculations`
> - 將輸入值計算完之後再回傳輸出值
> - 稱為：純函式 `pure function`、數學函式 `mathematical function`
#### ex：
- 找到最大值
- 檢核電郵地址是否有效

### 資料 `Data`
> - 事件的資料
#### ex：
- 使用者給我們的電郵地址
- 從銀行的 api 讀取到的金額

可以將這些概念應用在以下情況
1). 想一個問題時：在撰寫程式前，需要將程式分類成動作、計算、資料，動作需要特別注意，資料可以被抓取到，什麼樣的計算要去決定
2). 寫一個解決方案：在寫程式時可以反映出這三個分類，計算會從動作中抽哩，而資料會從計算中抽離，動作不能被寫成計算，計算不能被寫成資料
3). 閱讀程式碼：可以藉由這三個分類知道程式碼在幹嘛，也比較容易重構

## 2. 將動作、計算、資料應用在任何情況
> Actions, calculations, and data apply to any situation

以下用到雜貨店購物為例

![FP grocery shopping process actions.png](http://192.168.25.60:8000/files/file_storage/06e4e0fb.png)

### 檢查冰箱
- 一個動作：因為什麼時候檢查會有差別，關於食物的資訊稱為資料，我們會稱之為當前庫存 `current inventory`

### 開車去商店
- 一個動作：會有一些資訊，商店的位置與去的方向

### 買需要的東西
- 一個動作：要怎樣知道需要什麼東西? 可以建立一個 購物清單 = 所需庫存 - 當前庫存
- 可以將買需要的東西再進行拆分
	- 資料：所需庫存、當前庫存、購物清單
  - 計算：相減
  - 動作：用購物清單購物

![FP buying what you need.png](http://192.168.25.60:8000/files/file_storage/b31ffd7a.png)

### 開車回家
- 一個動作

將以上綜合分析畫成圖

![FP grocery shopping process.png](http://192.168.25.60:8000/files/file_storage/8fb2e780.png)

#### 動作
- 檢查冰箱()：當前庫存
- 開車去商店()
- 用購物清單購物(購物清單)
- 開車回家()
#### 計算
- 庫存相減(當前庫存, 所需庫存)：購物清單
#### 資料
- 當前庫存
- 所需庫存
- 購物清單

## 3. 從購物過程得到的課題
> Lessons from our shopping process

### 我們可以將 ACD 應用在各種情況

### 動作可以隱藏動作、計算、資料
- 一個簡單的動作可能由不同分類組成，一部分 FP 是在於將動作拆分成更小的動作、計算、資料，並知道什麼時候要將他們組合起來

### 計算可以用更小的計算、資料組成
- 計算可以再拆分成多個計算，第一個計算的輸出值會變成下一個計算的輸入值

### 資料可以用多個資料組成

### 計算通常在我們手中發生
- 我們需要做任何決定嗎?有什麼事情是我們可以預先規劃的?決定跟規劃是計算很好的候選人

### 資料
- 資料的結構會反映出目前問題領域的實作結構
- 兩個主要實作不活躍資料
	1). 寫入時複製 `Copy-on-write`：在修改前先複製一份資料起來  
  2). 預防性的複製 `Defensive copying`：將要保存的資料複製一份起來
- 資料不像動作和計算，他不能夠跑，他是不活躍的
- 可序列化 `serializable`：資料可以被妥善保存很久
- 比較是否相等 `compare for equality`：可以比較兩個資料是否相同，動作及計算無法比較
- 開放翻譯 `open for interpretation`：資料可以被不同方式翻譯，相同的資料可能會有不同的翻譯結果
- 資料為不可數名詞 `mass noun`
	- 事件的真相
  - 理由、討論、計算的實際資料
  - 從一個需要被有效執行的輸入裝置獲取到的資訊
- 資料從他的生命週期裡可以被翻譯成很多層，從位元組 `bytes`、字元 `character`、JSON、使用者資訊

![FP multiple levels of interpretation of a web request.png](http://192.168.25.60:8000/files/file_storage/51cf29f4.png)

#### ex：
- 要購買的食物清單
- 你的姓氏
- 我的電話號碼
- 餐點的食譜

## 4. 將函式思惟應用在新的程式碼中
> Applying functional thinking to new code

- 一個 CouponDog 的新商業策略，CouponDog 有許多人感興趣的優惠券
- 為了擴大業績，行銷長 `CMO` 有一個計畫，若是有人推薦 CouponDog 給10個朋友，就可以拿到更好的優惠
- 有一個表格紀錄電郵，並記錄推薦給朋友的次數
- 有一個表格紀錄優惠券，紀錄被推薦的等級

![FP email and coupon database table.png](http://192.168.25.60:8000/files/file_storage/fc10c5a4.png)

將以下進行 ACD 分類

- 發送電郵：A
- 從資料庫讀取訂閱者：A
- 優惠券的等級：D
- 從資料庫讀取優惠券：A
- 電郵的主旨：D
- 電郵地址：D
- 推薦次數：D
- 決定誰得到電郵：C
- 訂閱者記錄：D
- 優惠券紀錄：D
- 優惠券記錄清單：D
- 訂閱者記錄清單：D
- 電郵內文：D

## 5. 畫優惠券電郵流程圖
> Drawign the coupon email process

### 1). 從資料庫取得訂閱者資料
- 從資料庫取得訂閱者資料：動作，今天取的資料跟明天取的資料會不一樣，這取決於什麼時候跑
- 訂閱者資料：資料

![FP coupon email process calculations fetch subscribers.png](http://192.168.25.60:8000/files/file_storage/38463ac2.png)

### 2). 從資料庫取得優惠券資料
- 從資料庫取得優惠券資料：動作，在資料庫裡的優惠券會有異動，什麼時候跑會有差別
- 優惠券資料：資料

![FP coupon email process calculations fetch coupons.png](http://192.168.25.60:8000/files/file_storage/e83aaa88.png)

### 3). 生成要寄出的電郵
- 生成要寄出的電郵：計算，產生出電郵清單

![FP coupon email process calculations generating the emails to send.png](http://192.168.25.60:8000/files/file_storage/4fbc9a15.png)

- 電郵清單(訂閱者清單, 優惠券清單) = 電郵清單：計算

![FP coupon email process calculations generating the emails to send plan list of emails.png](http://192.168.25.60:8000/files/file_storage/17a2c855.png)

- 選取好的優惠券清單(優惠券清單) = 好的優惠券清單：計算
- 選取最佳的優惠券清單(優惠券清單) = 最佳的優惠券清單：計算

![FP coupon email process calculations generating the emails to send select good or best coupons.png](http://192.168.25.60:8000/files/file_storage/21cb920b.png)

- 決定優惠券的等級(訂閱者) = 優惠券等級

![FP coupon email process calculations generating the emails to send decide coupon rank.png](http://192.168.25.60:8000/files/file_storage/88e47b65.png)

- 根據優惠券等級生成電郵(訂閱者, 好的優惠券清單, 最佳的優惠券清單, 優惠券等級) = 電郵：計算

![FP coupon email process calculations generating the emails to send decide coupon rank and plan good or best email.png](http://192.168.25.60:8000/files/file_storage/a74fae3b.png)

### 4). 發送電郵
- 發送電郵：動作

![FP coupon email process calculations sending the emails.png](http://192.168.25.60:8000/files/file_storage/a6ee2113.png)

## 6. 實作優惠券電郵流程
> Implementing the coupon email process

### 以下實作一個計算，兩個資料
- 訂閱者資料：資料
- 優惠券等級資料：資料
- 決定優惠券的等級：計算

![FP Implementing coupon email process.png](http://192.168.25.60:8000/files/file_storage/c847f545.png)

#### 訂閱者資料：資料
- 訂閱者資料格式為 json

```javascript
var subscriber = {
  email: "sam@pmail.com",
  rec_count: 16
};
```

![FP Implementing coupon email process subscriber.png](http://192.168.25.60:8000/files/file_storage/a40ce5c4.png)

#### 優惠券等級資料：資料
- 優惠券等級資料格式為 string

```javascript
var rank1 = "best";
var rank2 = "good";
```

![FP Implementing coupon email process coupon rank.png](http://192.168.25.60:8000/files/file_storage/e5c5172b.png)

#### 決定優惠券的等級：計算
- 計算通常為一個函式，輸入值為參數，輸出值為回傳值

```javascript
function subCouponRank(subscriber) {
  if(subscriber.rec_count >= 10)
    return "best";
  else
    return "good";
}
```

### 以下實作一個計算，一個資料
- 優惠券資料：資料
- 利用等級選取優惠券：計算

![FP Implementing coupon email process select good or best coupons.png](http://192.168.25.60:8000/files/file_storage/ed22d432.png)

#### 優惠券資料：資料
- 優惠券資料格式為 json

```javascript
var coupon = {
  code: "10PERCENT",
  rank: "bad"
};
```

![FP Implementing coupon email process coupon.png](http://192.168.25.60:8000/files/file_storage/3876a4f9.png)

#### 利用等級選取優惠券：計算
- 輸入值：亂序等級的優惠券清單
- 輸入值：等級
- 輸出值：有相同等級的優惠券清單

```javascript
function selectCouponsByRank(coupons, rank) {
  // initialize an empty array
  var ret = [];
  // loop through all coupons
  for(var c = 0; c < coupons.length; c++) {
    var coupon = coupons[c];
    // if it's of the rank we want,add the coupon string to the array
    if(coupon.rank === rank)
      ret.push(coupon.code);
  }
  // output
  return ret;
}
```

### 以下實作一個動作，二個計算，一個資料
- 電郵資料：資料
- 回覆訂閱者的電郵：計算
- 組合所有訂閱者的電郵：計算
- 發送電郵：動作

![FP Implementing coupon email process plans an email.png](http://192.168.25.60:8000/files/file_storage/a15efc91.png)

#### 電郵資料：資料
- 電郵資料格式為 json

```javascript
var message = {
  from: "newsletter@coupondog.co",
  to: "sam@pmail.com",
  subject: "Your weekly coupons inside",
  body: "Here are your coupons ..."
};
```

#### 回覆訂閱者的電郵：計算

```javascript
function emailForSubscriber(subscriber, goods, bests) {
  var rank = subCouponRank(subscriber);
  // decide the rank
  if(rank === "best")
    // create and return an email
    return {
      from: "newsletter@coupondog.co",
      to: subscriber.email,
      subject: "Your best weekly coupons inside",
      body: "Here are the best coupons: " + bests.join(", ")
    };
  else // rank === "good"
    // create and return an email
    return {
      from: "newsletter@coupondog.co",
      to: subscriber.email,
      subject: "Your good weekly coupons inside",
      body: "Here are the good coupons: " + goods.join(", ")
    };
}
```

#### 組合所有訂閱者的電郵：計算

```javascript
function emailsForSubscribers(subscribers, goods, bests) {
  var emails = [];
  for(var s = 0; s < subscribers.length; s++) {
    var subscriber = subscribers[s];
    // generating all emails is just a loop around generating one email
    var email = emailForSubscriber(subscriber, goods, bests);
    emails.push(email);
  }
  return emails;
}
```

#### 發送電郵：動作

```javascript
// the action that ties it all together
function sendIssue() {
  var coupons     = fetchCouponsFromDB();
  var goodCoupons = selectCouponsByRank(coupons, "good");
  var bestCoupons = selectCouponsByRank(coupons, "best");
  var subscribers = fetchSubscribersFromDB();
  var emails = emailsForSubscribers(subscribers, goodCoupons, bestCoupons);
  for(var e = 0; e < emails.length; e++) {
    var email = emails[e];
    emailSystem.send(email);
  }
}
```

-  假設已知可以一次發送 20 封電郵，可以改為以下 code

```javascript
function sendIssue() {
  var coupons = fetchCouponsFromDB();
  var goodCoupons = selectCouponsByRank(coupons, "good");
  var bestCoupons = selectCouponsByRank(coupons, "best");
  // start at page 0
  var page = 0;
  var subscribers = fetchSubscribersFromDB(page);
  // loop until we fetch empty page
  while(subscribers.length > 0) {
    var emails = emailsForSubscribers(subscribers,
                                      goodCoupons, bestCoupons);
    for(var e = 0; e < emails.length; e++) {
      var email = emails[e];
      emailSystem.send(email);
    }
    // go to next page
    page++;
    subscribers = fetchSubscribersFromDB(page);
  }
}
```

### 什麼是計算?
- 將輸入值計算得到輸出值
- 不論什麼時候跑，或跑了幾次，相同輸入值會有相同的輸出值

### 如何實作計算?
- 由函式 `function` 實作
- 若沒有函式的語言，需要使用一個包含方法的類別

### 計算代表的意義
- 將輸入值計算得到輸出值
- 當你使用或如何使用時，會取決於計算是否符合這個情境

### 計算與動作做比較的優點
- 較簡單測試
- 較容易分析
- 可組合性

### ex：
- 加法、乘法
- 字串串接
- 規劃一趟購物

### 通常會稱為什麼?
- 純函式 `pure function`
- 數學函式 `mathematical functions`
- 計算 `calculations`

## 7. 應用函式思維到現有的程式碼
> Applying functional thinking to existing code

- 現有 Jenna 的程式碼，發送附屬公司的佣金
- sendPayout()：動作，轉帳金額到銀行戶頭
- figurePayout()：動作
- affiliatePayout()：動作
- main()：動作

```javascript
function figurePayout(affiliate) {
  var owed = affiliate.sales * affiliate.commission;
  if(owed > 100) // don’t send payouts less than $100
    sendPayout(affiliate.bank_code, owed);
}

function affiliatePayout(affiliates) {
  for(var a = 0; a < affiliates.length; a++)
    figurePayout(affiliates[a]);
}

function main(affiliates) {
  affiliatePayout(affiliates);
}
```

## 8. 動作佈及程式碼
> Actions spread through code

- 一個動作呼叫另外一個動作，就會成為另外一個動作，要盡量避免

## 9. 動作可以是很多形式
> Actions can take many forms

### 以下為 Javascript 的一些動作

#### 函式呼叫 ( Function calls )
- 告警視窗
```javascript
alert("Hello world!")
```
#### 方法呼叫 ( Method calls )
- 控制台印出訊息
```javascript
console.log("hello")
```
#### 構造函式 ( Constructors )
- 日期：當你呼叫時會有不同的值，初始值為現在的日期時間
```javascript
new Date()
```
#### 表達式 ( Expressions)
- 存取變量 ( variable access )：可分享的、可變動的，在不同時間讀取會有不同的值
```javascript
y
```
- 屬性存取 ( property access )：可分享的、可變動的物件，在不同時間讀取會有不同的值
```javascript
user.first_name
```
- 陣列存取 ( array access )：可分享的、可變動的陣列，在不同時間讀取會有不同的值
```javascript
stack[0]
```
#### 陳述式 ( Statements )
- 指派 ( assigment )：可分享的、可變動的變數，可以影響其他部分的程式碼
```javascript
z = 3;
```
- 屬性刪除 ( property deletion )：刪除屬性會影響其他部分的程式碼
```javascript
delete user.first_name;
```

### 什麼是動作?
- 動作可以影響世界，或被世界影響
- 動作取決於什麼時候執行或被執行了幾次
- 什麼時候執行：有次序
- 被執行了幾次：可重複性

### 如何實作動作?
- 由函式 `function` 實作

### 動作代表的意義
- 如何影響世界，並需要確認此影響是不是我們需要的

### ex：
- 發送電郵
- 從帳戶提取金錢
- 修改一個全域變數
- 發送一個 ajax 請求

### 通常會稱為什麼?
- 非純函式 `impure function`
- 有副作用的函式 `side-effecting function`、`function with side effects`
- 動作 `actions`





