---
title: Mopcon 2022
description: 
published: true
date: 2022-11-09T05:53:40.102Z
tags: mopcon
editor: markdown
dateCreated: 2022-10-21T00:32:06.248Z
---

# Mopcon
- [ ] [Mopcon](https://mopcon.org/2022/)
- [ ] [共筆](https://hackmd.io/@mopcon/2022/https%3A%2F%2Fmopcon.org%2F2022%2Fschedule%2F)

# 現代 Web 框架效能優化技巧分享
> - **Web Development**
> - Web 框架層出不窮，但各種重要元件的使用方式漸漸殊途同歸。而在效能優化上，許多坊間的做法較為著重在程式語言層、高併發等實務上其實較少遇到的效能瓶頸處，真正的效能瓶頸通常是運用的不是那麼洽當的解決方案，例如 ORM 的濫用、邏輯結構設計不良... 等等，這部分的效能優化通常成本低廉但效益十足。這個議程將會分享在 Web 的技術專案中，常見的效能優化技巧與思維。

## Laravel 框架       

# 如何以 JS 實作 Shared Components 的效益分析工具
> - **Web Development**
> - Design System 能為產品帶來更為一致性的體驗。但 Design System 需要時間醞釀、開發，而其影響並不容易直觀的觀測、闡述，也因此難以闡述其影響力。講者爬梳網路上各種 Design System 效益分析的文章，並以 JS 實作分析工具，讓 Design System 的效益得以統計、分析。


# 前端好朋友 - tailwindcss
> - **Web Development**
> - 2022 年 CSS 的決解方案
> - 導入 tailwindcss 的優點
> - 透過 tailwindcss 與設計師優雅的溝通


# Visual Testing
> - **Web Development、UI/UX**
> - 前端工程師遇到什麼問題？
> - 什麼是 Visual Testing？
> - 怎麼做  Visual Testing？
> - 專案範例 👏🏻
> - 工作流程💡
> - 疑難雜症 🤔

[mopcon-visual-testing](https://www.cythilya.tw/2022/10/15/mopcon-visual-testing/)

## 定義
visual testing 是檢視 UI 視覺變更的一種測試，它對使用者看到的畫面做快照，然後做比對。

## 能帶給我們什麼好處？
- 第一，省時省力省空間。
- 第二，精確比對。
- 第三，提升覆蓋率，同時也能減少寫測試的量和維護測試程式碼的時間。
- 第四，visual testing 幫我們率先排除了一些問題，讓後續測試能順利進行、順便提升 pipeline 穩定度。
- 最後是，提升開發的爽度。

## 怎麼做
分為兩種方式，第一種是針對元件，第二種是針對頁面。

## 工作流程
- 第 1 步：跑測試，可以選用 Storybook 結合 Chromatic 或 Percy 來做元件測試，或是用 E2E testing 工具像是 Cypress 來做頁面測試。
- 第 2 步：針對元件或是頁面做快照。
- 第 3 步：比對快照，找出不同點。
- 第 4 步：根據快照，可以選擇更新程式碼或是更新快照的比較基準。在這個步驟，可能會回到第 1 步跑測試，來回多次。
- 第 5 步：確認無誤後，即可 merge 程式碼和上線。

# 用 DDD 事件風暴 & OOA 來展開你對 Kubernetes 的認知！
> - **Web Development、Cloud Service、DevOps、Domain-Driven Design**
> - 這一次演講中，我們要來做一個實驗💡💡——我們是否能夠善用在領域驅動設計和物件導向領域中學到的「建模」方法，藉由上帝視角來加速我們的學習效率！？？
為了能夠帶大家和我們走一趟這一場實驗，水球（主修軟體設計模式和物件導向）和 Yiyu （主修雲端技術、Kubernetes）要來在大家面前透過事件風暴和物件導向的建模技巧，來實際藉由大量的對話建構出共同對於 Kubernetes 的認知；快速用上帝視角來把 Kubernetes 的各種行為流程和元素給摸個清楚。
因此我們不只會一起複習 Kubernetes 的元素，還會熟悉領域驅動設計和物件導向的核心思想和技法！會有什麼樣子的火花呢，水球和 Yiyu 邀請大家和我們一起走一趟！

## 定義
領域驅動設計 DDD (Domain-Driven Design)：
- 對這領域很熟悉，發揮這領域的特性設計出來的一種 Methodology
- DDD 是一種基於領域知識來解決複雜業務問題的軟體開發方法論

## 三個重點
- 跟領域專家 (domain expert) 密切合作來定義出 domain 的範圍及相關解決的方案。
- 切分領域出數個子領域，並專注在核心子領域。
- 透過一系列設計模式，將領域知識注入進程式模型 (model) 中。

## DDD 示範
使用工具：[Miro](https://miro.com/app/dashboard/)

將討論過程中的一切資訊可視化 (**行為面 + 結構面**) ，如 Object、Action、Process

[Miro 圖](https://miro.com/app/board/uXjVPLAuuZI=/)
[Miro 圖](https://miro.com/app/board/uXjVPF_1HJ4=/)

- 行為面
![miro 用 DDD 事件風暴 & OOA 來展開你對 Kubernetes 的認知 Basic.png](http://192.168.25.60:8000/files/file_storage/09464722.jpg)

![miro 用 DDD 事件風暴 & OOA 來展開你對 Kubernetes 的認知 Example.png](http://192.168.25.60:8000/files/file_storage/3eb7166b.jpg)

![miro 用 DDD 事件風暴 & OOA 來展開你對 Kubernetes 的認知 Detail Basic.png](http://192.168.25.60:8000/files/file_storage/21a01a61.jpg)

- 結構面：使用 UML
# 幫你的 Web 開發添加一點 Functional Programming 味
> - **DevOps、Web Development**
> - 以 Spring Framework 的 Web 開發作為例子，加入 Functional Programming 的函式庫 Arrow-Kt，學習 Option, Either 和 Validated 等觀念如何應用在 web 開發上