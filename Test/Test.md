---
title: Test
description: 單元測試
published: true
date: 2023-11-20T05:58:46.677Z
tags: test
editor: markdown
dateCreated: 2023-11-20T01:43:07.814Z
---

# Test
- [ ] [Paradigm Shift: How Developer to Tester Ratio Changed From 1:1 to 100:1](https://blog.bytebytego.com/p/ep68-top-architectural-styles?utm_source=profile&utm_medium=reader2)
- [ ] [A compilation of outstanding testing articles (with JavaScript)](https://practica.dev/blog/a-compilation-of-outstanding-testing-articles-with-javaScript/)
- [ ] [Testing in software development: A practical guide](https://learningdaily.dev/testing-in-software-development-a-practical-guide-f2b4d9d51cae)
- [ ] [Best ways to test system functionality](https://blog.bytebytego.com/p/ep82-open-sourcing-over-100-byte?utm_source=profile&utm_medium=reader2)
- [ ] [EP83: Explaining 9 Types of API Testing](https://blog.bytebytego.com/p/ep83-explaining-9-types-of-api-testing?utm_source=profile&utm_medium=reader2)
- [ ] [軟體測試方法](https://ellis-wu.github.io/2015/09/04/test-method-introduction/)
- [ ] [初嚐軟體測試(Software Testing) ](https://medium.com/jimmy-wang/%E5%88%9D%E5%98%97%E8%BB%9F%E9%AB%94%E6%B8%AC%E8%A9%A6-software-testing-3c7d786525a9)
- [ ] [《軟體工程篇 - 5》 — 軟體測試方法 (Software Testing)](https://ithelp.ithome.com.tw/articles/10337526)
- [ ] [軟體測試分類和方法](https://www.796t.com/content/1542985090.html)
- [ ] [測試二三事（上）｜還記得你說，要和我寫測試](https://hackmd.io/@Heidi-Liu/learn-about-testing)

# Types

![UnitTest three types of test.png](http://192.168.25.60:8000/files/file_storage/e4908507.png)

![The 9 Testing Types.gif](http://192.168.25.60:8000/files/file_storage/dbb38a1d.gif)

![Explaining 9 types of API testing.gif](http://192.168.25.60:8000/files/file_storage/0d56d76e.gif)

在不同的測試類型中，所需要保護的面向不太相同，以下是不同測試類型的大致介紹：
## 單元測試 ( Unit testing )
- 以程式碼的最小單位進行測試，保護程式邏輯不會在系統維護的過程中遭到破壞，也進一步確保維護中的程式碼品質
- 這種測試類型通常由開發人員自行撰寫，自己寫的 Code 自己寫測試，有經驗的開發人員可以用非常有效率的方式撰寫單元測試，因為測試範圍小，這種類型的測試通常不需要設立測試環境，因此可以得到較高的撰寫效率，也是所有測試類型中最容易撰寫的測試類型
- 單元測試以程式碼的最小單位進行測試，所以這種測試類型通常執行速度快 (測試範圍小)、可靠度高 (確保小部分程式碼正常運作)、有效隔絕失敗 (從數萬行程式碼之間隔絕出可能的錯誤)
- 不過實務上單元測試相當考驗系統架構的能力，程式架構不夠 `S.O.L.I.D.` 通常也意味著單元測試很難寫。當單元測試很好寫的時候，通常也代表著你的程式架構還算漂亮
- 照理來說，單元測試的比例應該是所有測試中最高的，因為開發的成本相對低廉。(但前提是架構漂亮的程式，單元測試才會好寫)

## 整合測試 ( Integration testing )
- 整合多方資源進行測試，確保模組與模組之間的互動行為正確無誤，也讓不同模組在各自開發維護的過程中不會因為功能調整而遭到破壞
- 這種類型的測試通常介於單元測試與端對端測試之間，有時候會由專職的測試人員進行開發，但大部分還是由開發團隊中負責特定模組的人來撰寫。有時候單一模組即便完全通過單元測試，獨立運作也正常，但是當需要與其他模組互動時，也是有可能發生錯誤，這時就是整合測試的主要負責領域
- 整合測試會跨越單元測試的範圍，對不同模組之間的交互作用進行測試，意味著你也要準備更完整的模擬環境，來幫助你完成測試工作
- 一般來說，整合測試的數量應該介於單元測試與端對端測試之間，針對幾個主要的模組進行整合測試即可

## 端對端測試 ( End-to-end testing ) ( E2E testing )
- 所謂的「端對端」( E2E ) 是指從使用者的角度出發(一端)，對真實系統(另一端)進行測試
- 這種類型的測試對許多公司來說，就是「人工測試」的主要範圍，因為你可以透過人工對已經完整部屬的網站進行測試，因此可以驗證出系統是否符合客戶的實際需求。這部分也可以透過撰寫 E2E 測試程式來進行自動化，增加測試效率
- 端對端測試是從使用者的角度出發對系統進行測試，你所要準備的測試環境就是一套完整的系統部署，如果能透過 Docker 容器技術進行部署，那麼設立測試環境的成本將會大幅降低
- 由於端對端測試可以正確反映出使用者需求是否滿足，因此商業價值較高，但要對一個複雜的系統進行完整的 E2E 測試開發，可能會有相當多的測試案例，如果真的要做到 100% 的測試覆蓋率，成本也會相對的提高許多

## 壓力測試 ( Stress Testing )

## 負載測試 ( Load Testing )

## 效能測試 ( Performance Testing )





