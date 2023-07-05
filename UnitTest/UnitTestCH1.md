---
title: Unit Test CH1
description: The Art of Unit Testing Third Edition v7
published: true
date: 2022-06-27T05:46:56.725Z
tags: unit test
editor: markdown
dateCreated: 2022-06-16T06:54:42.745Z
---

# CH1 The basics of unit testing
> - 定義進入點 `entry points` 與出口點 `exit points`
> - 定義工作單元 `unit of work` 與單元測試 `unit test`
> - 用整合測試對比單元測試
> - 開發一個簡單的單元測式案例
> - 理解測試驅動開發 TDD `Test-driven development`

## 1.1 第一步
> The first step

- 此章節第一步會來分析測試的經典定義，並與整合測試做對比
- 將單元測試與其他測試做區分，在測試時將會有效的提升自信，不論是成功或者失敗
- 我們將會討論相對於整合測試，單元測試的優缺點 `pons and cons`
- 發展出一個好的單元測試的定義
- 我們會使用測試驅動開發模式

## 1.2 一步步來定義單元測試
> Defining unit testing, step by step

### 單元測試 ( unit test )
- Wiki 定義：單元測試通常是自動化撰寫並執行，軟體開發人員用來確保一小部分應用程式是否有達到預期的設計與行為
- 在一系列的程式設計當中，單元可以是一整個模組 `module`，但更常見的為一個單獨的函式或程序
- 在物件導向程式語言中，一個單元通常是一整個介面，像是類 `class`，或者是一個單獨的方法

### 被測系統（`SUT` system under test） 
- 撰寫測試的為被測系統
- SUT 代表著主體，系統或 `suite under test`，或 CUT ( component,module or class under test or code under test )

### 單元 ( unit )
- 單元代表著系統中的工作單元 `unit of work` 或用例 `use case`
- 工作單元都有一個進入點(開始)與出口點(結束)，最簡單的例子為函式，計算一些東西並回傳一個值
- 在計算的過程中，函式也可以使用其他函式，其他模組與其他元件，此一系列流程可稱之為單元工作(從進入點到出口點)，這時已經跨越了僅僅是一個函式而已

### 工作單元 ( Unit of Work )
- 工作單元是一系列動作的組合，從進入點的發起到一個可注意到的結束點(透過一個或多個出口點)
#### ex：
- 函式本身是一個或其中一的部分的工作單元
- 函式的宣告與簽署為進入本體的進入點
- 函式的輸出值或行為是出口點

## 1.3 進入點與出口點
> Entry Points & Exit Points

### 工作單元都有進入點與出口點
- 工作單元可以是一個單一函式，多個函式，多個模組或物件
- 都會有從外部觸發的進入點，並在結束時做有用的事情，若不是有做有用的事情理應當從程式碼當中移除

![Unit Test A unit of work has entry points and exit points.png](http://192.168.25.60:8000/files/file_storage/bec65ceb.png)

### 什麼是有用的
- 有回傳值，狀態改變，呼叫外部

### 為什麼是出口點?
- 使用出口點而非行為 `behavior`
- 出口點代表著離開工作單元的情境，並回到測試的情境

![Unit Test Types of Exit Points.png](http://192.168.25.60:8000/files/file_storage/81132796.png)

### 一個簡單的工作單元程式碼，一個進入點一個出口點
```javascript
// number-parser.js
/**
 * Our System Under Test (SUT)
 * @param {string} numbers
 * @returns {number}
 */
const sum = (numbers) => {
  const [a, b] = numbers.split(',');
  const result = parseInt(a, 10) + parseInt(b, 10);
  return result;
};

module.exports.sum = sum;
```

- 此工作單元由一個函式組成，有進入點與出口點
- 使用 sum(numbers) 而非 numbers，因為進入點為一個函式聲明 `function signature`，把函式名字去掉以後剩下的東西（回傳值、參數、調用方式等）

![Unit Test A function that has the same entry point as the exit point](http://192.168.25.60:8000/files/file_storage/4f7e5122.png)

### 一個有一個進入點與多個出口點的工作單元程式碼
```javascript
// number-parser2.js
let total = 0;

/**
 *
 * @returns {number}
 */
const totalSoFar = () => {
  return total;
};

/**
 * Our "production" code to be tested.
 * @param {string} numbers
 * @returns {number}
 */
const sum = (numbers) => {
  const [a, b] = numbers.split(',');
  const result = Number.parseInt(a, 10) + Number.parseInt(b, 10);
  total += result;
  return result;
};

module.exports = {
  sum,
  totalSoFar,
};
```

- 此版的 sum 有兩個出口點
	- 1). 回傳值
  - 2). 有一個新的函式：透過呼叫這個進入點，可以設定模組的狀態
- 這兩個不同的出口點可以想像為同一個工作單元中兩個不同的途徑或需求
- 也需要針對兩個不同的出口點做單元測試
- 從設計層面來看，可以將這兩個主要動作拆分為查詢動作 `Query actions` 與命令動作 `Command actions`
- 查詢動作 `Query actions`：沒有改變東西，僅回傳值
- 命令動作 `Command actions`：有改變東西，無回傳值

![Unit Test A unit of work with two exit points.png](http://192.168.25.60:8000/files/file_storage/a5030d19.png)

### 一個呼叫第三方套件的工作單元程式碼
```javascript
// number-parser3.js
const winston = require('winston');
let total = 0;

/**
 *
 * @returns {number}
 */
const totalSoFar = () => {
  return total;
};

const makeLogger = () => {
  return winston.createLogger({
    level: 'info',
    transports: new winston.transports.Console(),
  });
};

const logger = makeLogger();

const sum = (numbers) => {
  const [a, b] = numbers.split(',');
  logger.info('this is a very important log output', {
    firstNumWas: a,
    secondNumWas: b,
  });

  const result = Number.parseInt(a, 10) + Number.parseInt(b, 10);
  total += result;
  return result;
};

module.exports = {
  totalSoFar,
  sum,
};
// run this file with node (name of this file) to see the logging
sum('10,20');
```
- 此例呼叫第三方套件 `dependency (3rd party)`
- 套件是一個我們無法用單元測試完全掌控的東西
- 需要利用出口點拆分單元測試，這樣可以讓測試更加有可讀性，更容易去除錯，做改變時不會影響到其他功能

![Unit Test A unit of work has calling a third party.png](http://192.168.25.60:8000/files/file_storage/f331205c.png)

## 1.4 出口點類型
> Exit Point Types

- 一個被調用的函式，有回傳值。`non void function`
- 系統有顯著的狀態變化，並再被調用之後可以不用被查問私人狀態而被決定
	- wasCalled() 在狀態被改變之後回傳不同的值
- 呼叫第三方套件/系統，其無法被單元測試控制，第三方系統不會回傳值或值可以被忽略
- 進入點與出口點會影響單元測試的定義
- 單元測試會調用一個工作單元並確認一個特定的出口點，若預期的結果出來是錯誤的，單元測試就是失敗的
- 單元測試可以擴展為一個函式或多個模組/物件，取決於在進入點到出口點之間有多少資源被使用到

## 1.5 不同的出口點，不同的技術
> Different Exit Points, Different Techniques

不同的出口點需要用不同的技術來進行測試

### 有回傳值型 ( Return-value based exit points ) ➔ 有直接的輸出
- 觸發進入點，檢查回傳值是否正確

### 狀態型 ( State based tests) ➔ 無直接的輸出
- 呼叫某個東西，再呼叫其他東西來確認是否有按照計畫進行

### 第三方型 ( Third-party situation ) ➔ 無直接的輸出
- 需要在測試中使用到模擬對象 `mock object`，如此可以取代外部系統進而可以掌控測試

## 1.6 從頭開始的測試
> A Test from Scratch

為 number-parser.js 撰寫一個單元測試

![Unit Test A visual view of our test.png](http://192.168.25.60:8000/files/file_storage/88056c83.png)

```javascript
// custom-test-phase1.js
// I'm using 'require' to be compatible with node.js,
// so we won't need a transpiler like babel.
const { sum } = require('./number-parser');

/**
 * Our "Test" definition.
 */
const parserTest = () => {
  try {
    const result = sum('1,2');
    if (result === 3) {
      console.log('parserTest example 1 PASSED');
    } else {
      throw new Error(`parserTest: expected 3 but was ${result}`);
    }
  } catch (e) {
    console.error(e.stack);
  }
};

parserTest();
```

- 進入點為 sum ，輸入值為稱為 numbers 的一個 string
- 但此測試寫法還不夠完善

```javascript
// our production code (Suite Under Test - SUT)
const { sum } = require('./number-parser');

/**
 * A Test helper function for a simple assertion
 * @param actual (any type)
 * @param expected (any type)
 */
const assertEquals = (expected, actual) => {
  if (actual !== expected) {
    throw new Error(`Expected ${expected} but was ${actual}`);
  }
};

/**
 * A Test Helper Function
 * I named it 'check' so it doesn't get confused with other frameworks.
 * Wraps my code in try-catch and outputs things nicely to the console.
 * @param {string} name
 * @param {function} implementation
 */
const check = (name, implementation) => {
  try {
    implementation();
    console.log(`${name} passed`);
  } catch (e) {
    console.error(`${name} FAILED`, e.stack);
  }
};

/**
 * Our actual tests begin here
 * To run: "node ch1/custom-test-phase2.js
 */
check('sum with 2 numbers should sum them up', () => {
  const result = sum('1,2');
  assertEquals(3, result);
});

check('sum with multiple digit numbers should sum them up', () => {
  const result = sum('10,20');
  assertEquals(30, result);
});
```

## 1.7 好的單元測試的特徵
> Characteristics of a good unit test

### 1.7.1 什麼是好的單元測試
#### 好的自動化測試 `automated tests`，不局限於單元測試
- 可閱讀及理解作者的想法
- 可閱讀及撰寫
- 可自動化及可重複性的
- 有用的並提供可操作的動作
- 每一個人可透過按一個按鈕來執行
- 當失敗時，可以輕易地偵測到並決定如何查明問題
#### 好的單元測試
- 能快速執行
- 有一致性的結果：在沒有改變任何規則的情況下，應輸出相同的結果
- 在測試底下對程式碼有完全的掌控性
- 每一個測試案例都可以被獨立執行，且互不依賴
- 應該在記憶體中執行，且不需要系統擋、網路、資料庫

### 1.7.2 一個簡單的檢查清單
- [X] 是否能從兩個星期前或幾個月或幾年前寫的測試執行並取得結果?
- [X] 是否團隊裡每個成員可以從兩個月前寫的測試執行並取得結果?
- [X] 是否測試可以在幾分鐘內執行完畢?
- [X] 是否能透過撰寫出來的按鈕執行所有測試?
- [X] 是否在沒有程式碼異動的情況下，測試執行完會有相同的結果?
- [X] 是否在其他團隊裡有 bug 時，我的測試是能過的?
- [X] 是否在其他機器或環境底下測試執行完會有相同的結果?
- [X] 是否在沒有資料庫、網路、部屬時測試會停止運作?
- [X] 當我刪除或移除一個測試時，其他測試是否可以保持不被影響?
- [X] 我是否能簡單的準備測試所需要的東西，不依賴外部資源如資料庫、第三方套件?

## 1.8 整合測試
> Integration tests

- 整合測試：測試一個工作單元，沒有真的掌控到他真實的相關依賴，如其他團隊的元件、其他 services、時間、網路、資料庫、線程、亂數生成器
- 整合測試會使用真實的依賴，單元測試會將工作單元從依賴獨立出來

### 1.8.1 對比自動化的單元測試，整合測試有的缺點

#### 用檢查清單來檢視

- [X] 是否能從兩個星期前或幾個月或幾年前寫的測試執行並取得結果?
	- 如果不能，要如何知道新增加的功能是否有被破壞?
  - 在共享程式碼時並在整個應用程式裡有規則的做異動，若在異動程式碼之後，不能用測試跑之前的所有功能，就在無意識中破壞了功能，此行為可稱之為回歸 `regression`
  - 回歸常常發生在最後階段或發表階段，當開發者在有壓力的情況下修復現有的 bugs
  - 通常會在修復舊的 bugs 時又產生出新的 bugs 
  - 回歸 `regression`：破壞原本可以運作的功能

---

- [X] 是否團隊裡每個成員可以從兩個月前寫的測試執行並取得結果?
  - 當我們在異動東西時不會影響到其他人的程式碼
  - 很多開發者會很擔心修改到老舊的程式碼 `legacy code` ，擔心其他程式碼是否是依賴著現在要修改的程式碼
  - `legacy code`：通常指的是難以測試或閱讀的程式碼

---

- [X] 是否測試可以在幾分鐘內執行完畢?
	- 當測試無法在短時間內執行完(幾秒內優於幾分鐘內)，你就會更少去跑測試(每天或每週或每月)
  - 問題就在於當你修改了程式碼，你需要快速地知道是否有破壞了其他東西
  - 當你跑測試時花的時間越多，表示你對系統的異動越多，表示當有破壞了東西時會有更多地方需要去查找
  - 測試必須跑得快
  
---

- [X] 是否能透過撰寫出來的按鈕執行所有測試?
	- 如果不能的話，表示需要先做機器的一些設定(如設定 docker 環境或資料庫連線)，單元測試才能進行自動化測試
  - 如果不能自動化測試的話，可能就會避免重複性的測試
  - 好的測試必須要能在原始狀態執行，而非手動
  
---

- [X] 是否能幾分鐘內寫完一個測試?
	- 一個簡單的方式去檢視整個測試，他會需要時間去正確的準備並實現，並非只是單純執行
  - 好的系統測試必須很簡單並快速的被寫出來

---

- [X] 是否在其他團隊裡有 bug 時，我的測試是能過的?
- [X] 是否在其他機器或環境底下測試執行完會有相同的結果?
- [X] 是否在沒有資料庫、網路、部屬時測試會停止運作?
	- 測試程式碼必需對於不同依賴是獨立的
  - 因為我們能完全的掌握需要進入到系統裡的間接輸入值 `indirect inputs`，所以測試結果是一致的
  - 我們可以有假的資料庫、網路、時間，可以稱之為 `stubs`，而可以注入這些 `stubs` 的稱為 `seams`

---

- [X] 當我刪除或移除一個測試時，其他測試是否可以保持不被影響?
	- 單元測試不需要共享狀態，但整合測試常常需要
  - 一個外部的資料庫、外部的 service，共享狀態會導致在測試之間有依賴性
  - 例如在一個不對的順序執行測試會弄壞為了以後測試的狀態
  
#### 綜合以上討論可以得到一些主要的結論
1. 可讀性 `Readability`：若不好閱讀的話，會很難維護或除錯
2. 可維護性 `Maintainability`：如果很難透過測試而可以有效的維護測試程式碼與產品程式碼，將會是一場夢魘
3. 可信賴性 `Trust`：當我們不夠信賴測試，而導致需要手動除錯時，將會耗費更多的時間


## 1.9 敲定我們的定義
> Finalizing our definition

- 更新最終版的定義：單元測試是一段調用工作單元的自動化程式，透過進入點與出口點
- 單元測試通常用測試框架寫出
- 可以簡單的撰寫並可快速執行
- 可信賴、可讀性、可維護性
- 產品的程式碼沒有異動的話具有一致性
- 第一版的定義：非控制流程程式碼 ( only running against control flow code )
- 控制流程程式碼 `control flow code`：帶有邏輯的程式碼，例如：if、loop、計算、做決定的程式碼


## 1.10 什麼是回歸測試?
> What About Regression Tests?

- 維基定義：檢驗軟體原有功能在修改後是否保持完整，可重複執行且非函式測試
- 單元測試在測試新的程式碼時就很像一種回歸測試
- 我們完成之後就會在建置後或提交後持續的執行，以去找到更多的回歸項目

## 1.11 測試驅動開發
> Test-driven development ( TDD )

### 寫一個失敗的測試給一個產品的功能
- 當已經有現有的程式碼存在時，失敗代表程式碼有 bug，成功代表沒有 bug
- 當產品還沒開發時，測試就會是失敗的

### 藉由增加產品的功能使測試成功以達到測試效果
- 產品的程式碼越簡單越好

### 重構程式碼
- 當測試成功之後，可以重構產品及測試的程式碼，讓他們更加有可讀性，並移除冗贅的部分
- 重構也要能維持原本的功能

## 1.12 成功的測試驅動開發三個重要的技能
> Three core skills needed for successful TDD

![Unit Test Three core skills of Test Driven Development.png](http://192.168.25.60:8000/files/file_storage/8b33e4d9.png)

### 寫好的測試 ( Writing Good Tests )
- 當快速寫好一個測試，不代表他們是可維護的、可讀性的、可信賴的

### 先寫測試 ( Writing Test-First )
- 當寫一個可維護的、可讀性的測試，不代表先寫測試時可以得到相同的效益

### SOLID 設計 ( SOLID Design )
- 當快速寫好一個測試，且是可維護的、可讀性的，不代表這會是一個設計良好的系統




















