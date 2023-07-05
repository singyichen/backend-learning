---
title: Function Programming CH2
description: Grokking Simplicity
published: true
date: 2022-06-23T00:35:46.582Z
tags: fp
editor: markdown
dateCreated: 2022-06-17T02:34:23.662Z
---

# CH2 Functional thinking in action
**大綱：綜觀兩個本書提出的概念**
> - 如何將將函式思維在實際問題上應用
> - 明白分層式設計 `stratified design` 可以如何幫助組織軟體架構
> - 學習如何在時間線 `timelne` 使動作顯現
> - 得知時間線如何幫助你解決時間相關的問題

## 1. 歡迎來到 Toni's Pizza
> Welcome to Toni's Pizza

歡迎來到 Toni's Pizza ，目前為 2118 年，似乎在未來人類依舊喜歡 pizza，但所有的 pizza 是由機器人製作的，而且機器人是由 Javascript 程式做出的。
Toni 在餐廳運作的程式碼使用了大量的函式思維，我們將會去窺探一下他的系統，包含廚房與存貨以及他如何實現兩層函式思維

### Part1：區分動作、計算、資料
> Distinguishing actions, calculations, and data

- Tni 很確定有將程式區分出使用到原料與其他資源例如不需經過計算的動作
- 我們將會看到他如何利用分層式設計的原則將程式碼組織到各分層

### Part2：使用 First Class 的抽象概念
> Using first-class abstractions

- Toni 的廚房是一個分散式系統因為有多個機器人在一起製作 pizza
- 我們可以窺探到 Toni 是用時間線圖 `timelne diagram` 去得知系統運作失敗的情況
- 我們也可以看到 Toni 如何用 First Class 函式 (函式使用函式當作參數) 去協調機器人如何成功的擴大烘焙範圍

## 2. Part1：區分動作、計算、資料
> Distinguishing actions, calculations, and data

- Toni's Pizza 事業成長了很多也跟著面臨到一些大問題。然而她勤奮的利用函式思維去處理好各種困難
- 她最主要應用到的函式思維是最為機本的：區分出動作、計算、資料
- 每一個函式開發人員都要能夠做這個區分，如此可以得知哪一部分的程式碼是單純的(計算及資料)，哪一個部分則需要更多的注意力
- 她的程式碼主要可以分成三個種類

### 動作
取決於什麼時候被呼叫，或被呼叫了多少次
動作可能會使用到一些原料或其他資源，例如烤箱、送貨車，所以 Toni 對於如何使用他們會非常小心
#### ex：
- 把麵團擀開
- 傳遞 pizza
- 訂購原料

### 計算
計算代表著決定或計畫，運行時不會影響到世界
Toni 很喜歡他們，因為可以在任何時間地點使用，也不用擔心他們會將廚房弄亂
#### ex：
- 兩倍的食譜
- 決定一個購物清單

### 資料
Toni 盡己所能的用無作用資料呈顯，包含會計、存貨、pizza 的食譜
資料被儲存之後是非常靈活的，可以在網路上被傳誦，可以使用在很多地方
#### ex：
- 客戶訂單
- 食譜
- 收據

這些都是 Toni's Pizza 事業中的例子，但這些分類應用到各層面，小至 Javascript 的表述式，大到函式
我們將會在第三章學習到這些分類彼此呼叫的時候是如何互動的
做這些區分對於 FP 非常重要

## 3. 透過變化率來組織程式碼
> Organizing code by "rate of change"

分散式設計：

- 隨著時間遞移，Toni 的事業成長使得系統難以異動，但透過函式思維，她學習到如何組織程式碼，以最小化異動程式碼所需要付出的代價
- 如下圖所示。最下方為最不常異動到的部分，最上方為最常異動到的部分
- 最底層(技術棧)：Javascript 語法變動會是最少的，內建的語言構造，像是陣列或物件 ➔ 因為需要依賴的程式碼較多，所以改變的幅度一般不會太大
- 中間層(領域規則)：製作 pizza 的部分。
- 最上層(商業規則)：實際的業務，像是這週的菜單會有哪些。 ➔ 因為很少程式碼依賴，所以可以快速修改
- 這樣的層層設計會促使每一寸程式碼能夠建立在一個相對穩定的基礎上
- 透過這樣的軟體開發模式建立系統，我們能確保更加容易的修改程式碼

![FP Organizing code by rate of change.png](http://192.168.25.60:8000/files/file_storage/4be2f38c.png)

## 4. 一等抽象
> First-class abstractions

- Toni 目前只有一個機器人，她遇到一個很大的問題，她沒辦法在短時間內烹飪出足夠的 pizza 來滿足客人
- 以下為機器人製作 pizza 的時間線圖
- 我們利用時間線圖去理解每一個動作需要耗費多少時間
- 動作是取決於什麼時候運作，所以確認在哪一個順序執行是非常重要的

![FP timeline diagram to make a pizza.png](http://192.168.25.60:8000/files/file_storage/ff7797b3.png)

## 5. 時間線用來視覺化分散式系統
> Timelines visualize distributed systems

- Toni 有一個機器人可以做好 pizza，但不夠快
- 程序是有順序性的，她很確定有一個方法可以讓三個機器人一起製作 pizza
- 她可以將工作分成三個部分並可平行運作：製作麵團、製作醬汁、粉碎起司
- 一旦她有超過一個機器人在工作，她就有一個分散式系統。動作可以不一定要有順序性
- Toni 可以藉由畫時間線圖，來幫助了解機器人在遇到問題時會做什麼，但無法
- 每一個機器人有自己的時間線
- 但用此方式做出來的很多 pizza 都不是很好，因為這些時間線導致有很多製作 pizza 方式可以做

![FP timeline visualize distributed system.png](http://192.168.25.60:8000/files/file_storage/43409c52.png)

## 6. 多個時間線可以應用在不同的訂單上面
> Multiple timelines can execute in different orderings

- 在時間線圖中，預設在不同的時間線間沒有協調
- 在時間線中不需要等待其他時間線完成，不會有等待的情況發生
- 在不同的時間線中的動作就會發生沒有次序的情況
- 製作醬汁的機器人會在麵團還沒製作好之前開始

![FP dough takes longer.png](http://192.168.25.60:8000/files/file_storage/cc1a9a2d.png)

- 製作醬汁的機器人會在起司還沒製作好之前開始

![FP cheese takes longer.png](http://192.168.25.60:8000/files/file_storage/c4fc1c37.png)

- 以下為 6 種可能執行的準備程序，只有兩個是醬汁是最後完成的，才會製作出好的 pizza 

![FP all 6 possible orderings.png](http://192.168.25.60:8000/files/file_storage/9402fb97.png)

- 分散式系統最困難的點在於時間線之間沒有協調好，導致在奇怪的順序下執行
- Toni 必須在機器人之間協調好，在原料還沒有準備好之前不要組裝 pizza 

## 7. 從分散式系統學習到難得的課題
> Hard-won lessons about distributed systems

- Toni 做了一個檢討會，她需要去關注會依賴時間的動作，確保他們在對的順序發生
	- 1). 預設時間線是沒有被協調的：麵團可能還沒被製作好，但其他時間線可能持續的運作，她需要去做時間線的協調
  - 2). 在各動作期間不能互相依賴：製作醬汁需要時間比較多但不見得每一次都是如此，每一個時間線必須要有獨立的順序性
  - 3). 在某些不好的時間點會有不好的成品：在測試階段都可行，但一旦晚餐時間到來，很多稀少的事情會變成常規，時間線必須在每一次都有良好的產出
  - 4). 時間線圖會揭露系統的各種問題：她可以從時間線圖可以知道起司無法在第一時間準備好，使用時間線圖可以更加理解系統
  

## 8. 切割時間線：讓機器人等待其他機器人
> Cutting the timeline：Making the robots wait for each other

- Toni 會帶著我們來看一下會在 CH17 學習到的觀念切割時間線 `cutting a timeline`
- 這是一個可以平行地協調多時間線的工作，可以稱之為高階運算 `higher-order operation`
- 每一個時間線會獨立執行，並在結束執行時等待彼此，這樣就不用在意哪一個先完成

![FP three robot setup with coordination.png](http://192.168.25.60:8000/files/file_storage/b722407b.png)

## 9. 從時間線學習到的正面課題
> Positive lessons learned about timelines

- 切割時間線可以讓每一個工作更加獨立作業：切割時間線讓 Toni 區分出可以平行處理的準備程序，而組裝程序則是需要一定的順序性。
- 透過時間線圖可以更加了解系統如何透過時間運作的：時間線可以視覺化平行及分散式系統
- 時間線圖是靈活的：時間線圖是一個可以協調各時間線的工具

![FP retrospective on coordinationg robots.png](http://192.168.25.60:8000/files/file_storage/67cc8ff9.png)




