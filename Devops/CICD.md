---
title: CI/CD
description: 
published: true
date: 2023-08-07T02:25:34.914Z
tags: devops
editor: markdown
dateCreated: 2022-12-14T06:08:20.338Z
---

# CI/CD
- [ ] [Day12 什麼是 CICD](https://ithelp.ithome.com.tw/articles/10219083)
- [ ] [CI/CD (持續性整合 / 部署) - 因為懶，所以更要 CI/CD！概念講解！](https://blog.kennycoder.io/2020/04/07/CI-CD-%E6%8C%81%E7%BA%8C%E6%80%A7%E6%95%B4%E5%90%88-%E9%83%A8%E7%BD%B2-%E5%9B%A0%E7%82%BA%E6%87%B6%EF%BC%8C%E6%89%80%E4%BB%A5%E6%9B%B4%E8%A6%81CI-CD%EF%BC%81%E6%A6%82%E5%BF%B5%E8%AC%9B%E8%A7%A3%EF%BC%81/)
- [ ] [[DevOps] CI/CD 介紹 - 基礎概念與導入準備](https://enzochang.com/cicd-introduction/)
- [ ] [DevOps漫談之一：DevOps、CI、CD都是什麼鬼？](https://blog.jjonline.cn/linux/238.html)
- [ ] [TeamCity CI/CD 指南](https://www.jetbrains.com/zh-cn/teamcity/ci-cd-guide/)
- [ ] [CI/CD是什麼？一篇認識CI/CD工具及優勢，將日常瑣事自動化](https://www.wingwill.com.tw/zh-tw/%E9%83%A8%E8%90%BD%E6%A0%BC/%E9%9B%B2%E5%9C%B0%E6%B7%B7%E5%90%88%E6%87%89%E7%94%A8/cicd%E5%B7%A5%E5%85%B7/)
- [ ] [DevOps books](https://blog.bytebytego.com/p/devops-books?utm_source=profile&utm_medium=reader2)
- [ ] [What is CI/CD? How does it help us ship faster? Is it worth the hassle?](https://www.youtube.com/watch?v=42UP1fxi2SY&embeds_referring_euri=https%3A%2F%2Fblog.bytebytego.com%2F&feature=emb_imp_woyt&ab_channel=ByteByteGo)
- [ ] [EP71: CI/CD Pipeline Explained in Simple Terms](https://blog.bytebytego.com/p/ep71-cicd-pipeline-explained-in-simple?utm_source=profile&utm_medium=reader2)

# 概念

![roadmap-ci-cd.png](http://192.168.25.60:8000/files/file_storage/c0944141.png)

# CI/CD ( Continuous Integration / Continuous Delivery、Continuous Deployment )
> - 敏捷式軟體開發生命週期中，從開發、測試到部署是一系列持續迭代的過程，CI/CD 指的是持續整合和持續交付或持續部署的組合實踐。
> - CI/CD 通過在應用程式的構建、測試和部署中實施自動化，在開發和運營團隊之間架起了橋梁。

![CICD.png](http://192.168.25.60:8000/files/file_storage/5e4671c1.png)

![CICD Pipeline.png](http://192.168.25.60:8000/files/file_storage/0581e116.png)

![CICD Pipeline Explained in Simple Terms.png](http://192.168.25.60:8000/files/file_storage/f7cc01f0.png)

# CI ( Continuous Integration ) 持續整合
> 每當開發人員 push 程式碼變更到 remote repository 時，觸發自動化 build 和 test 的程序，確保這次的變更是符合規範且通過所有測試案例的，減少有問題的程式碼影響到生產環境的機會。

![CI(Continuous Integration).png](http://192.168.25.60:8000/files/file_storage/36de13ed.png)

## 流程
1. 程式建置 ( build )
開發人員在每一次的 Commit & Push 後，都能夠於統一的環境自動 Build 程式，透過此一步驟可以避免每個開發人員因本機的環境＆套件版本不相同，造成 Service 異常。

2. 程式測試 ( test )
當程式編譯完成後，將會透過「單元測試」測試新寫的功能是否正確，或者確認是否有影響到現有功能，透過該步驟進行測試，可以避免掉開發人員遺忘於本機先行檢查，作為「雙重驗證」工用。

## 目的
- 將低人為疏失風險
- 減少人工手動的反覆步驟
- 進行版控管制
- 增加系統一致性與透明化
- 減少團隊 Loading

# CD ( Continuous Delivery ) 持續交付
> 在持續交付的階段，程式碼仍然需要先通過品質的驗證，最後會產生一個可以部署的成品，可能是一個執行檔或是 docker image。這個成品就可以用來進行發佈上線。而持續交付通常包含了一個手動的步驟，用來讓開發人員確認和部署到生產環境。

![CD(Continuous Delivery).png](http://192.168.25.60:8000/files/file_storage/61967cfc.png)

## 流程
1. 部署服務 ( deploy )
透過自動化方式，將寫好的程式碼更新到機器上並公開對外服務，另外需要確保套件版本＆資料庫資料完整性，也會透過監控系統進行服務存活檢查，若服務異常時會即時發送通知告至開發人員。

## 目的
- 保持每次更新程式都可順暢完成
- 確保服務存活

# CD ( Continuous Deployment ) 持續部署
> 持續部署不再需要開發人員手動部署成品到生產環境，只要程式碼提交的 PR 通過了，剩下的整個過程都會自動完成。

![CICD(Continuous Deployment).png](http://192.168.25.60:8000/files/file_storage/d305be98.png)

# CI/CD Workflow

![CICD Workflow.png](http://192.168.25.60:8000/files/file_storage/510f52fa.png)

主要的工作流：

1. 當專案程式碼提交 ( commit → push / merge )
2. 進入 CI Pipeline，建立一個測試環境，跑過所有單元測試+整合測試、程式碼規範檢查
3. 進入 CD Pipeline，完成程式碼部署

# 為什麼要 CI/CD ?
## CD/CD 的好處
CI/CD 可以為團隊帶來以下好處，同時也是我們所追求的目標：

- 避免版本衝突
- 降低人為操作失誤
- 減少人工時間花費
- 及早發現( bug )及早修正( fix )
- 專注開發提高生產力
- 加速產品迭代

# CI/CD 如何做？
## 先備知識與技能
做 CI/CD 之前，建議團隊要有一定的成熟度，才能快速將現有的程序轉換為自動化的方式運行，以下是我認為必須先具備的知識和技能，導入 CI/CD 時較能快速上手：

- **Shell Script ( Command )**：與作業系統 Kernel 互動的語言(指令)是一切的基礎
- **SSH 連線**：創建公鑰和私鑰，以特定使用者連線至遠端 Server 進行操作
- **Git 版本控制**：任何一種 Git workflow，用來管理分支、切換版本、合併流程
- **容器化技術**：用來隔離環境，快速搭建和部署（ex： docker、docker-compose )
- **程式碼規範檢查**：檢查你的程式碼，是否符合團隊規範的程式碼風格
- **單元測試、整合測試、自動化測試**：檢查你的程式碼，能否在目標環境正常運作

## 流程與準備
上述的知識與技能你都已經掌握了之後，開始盤點 CI/CD 各個步驟會用到的指令，接著確認這些指令的順序（哪些具有相依性？哪些可以平行化？）

先將工作流程的 DAG ( Directed Acyclic Graph ，有向無環圖)畫出來，然後手動執行一遍所有流程（這時有就能感受到人工做這些事其實相當繁瑣及耗時），下一步就是將相同的程序透過工具自動化執行！

## CI/CD 工具
> 常見的 CI/CD 工具有 Jenkins、GitLab、Circle CI、Travis CI、Drone 等，另外，各大雲端平台( AWS, GCP, Azure )也有推出 CI/CD 解決方案，更好地整合了自家的雲端服務。

![CICD 工具.png](http://192.168.25.60:8000/files/file_storage/02611ebb.png)

- GitLab：版控工具
> GitLab 主要的服務是提供 git 版本控制系統，其 CI/CD Pipeline 功能簡單又實用，使用者只需要設定於專案根目錄下的「.gitlab-ci.yml」檔，便可以開始驅動各種 Pipeline 協助您完成自動化測試及部署。目前有提供 GitLab CE（社群版）與 GitLab EE（企業版）兩種，使用者可以根據自己的需求選擇適合不同的方案。

- GitHub：版控工具
> GitHub 是眾所皆知的 Git Server 網站，其 CI/CD 服務稱為 GitHub Action ，提供了多項控制 API ，能夠幫助開發者編排、掌握工作流程，在提交程式碼後自動編譯、測試並部署至伺服器，讓每位開發者都能受惠於平台本身自有的 CI/CD 功能。

- Jenkins：自動化建置工具
> 老牌的 CI/CD 服務，缺點就在於設定檔很難撰寫，學習成本高。

- Drone：自動化建置工具
> 也是近年來新出的，是一套以 Golang 開發的工具，建置速度快又便利，學習成本低 + 容易維護，新手好上手，最重要的一點它是以 Docker 為 Base 的，所有流程都是在 Docker 內完成，所以非常適合在 Kubernetes 平台啟動容器，當然缺點就是套件不多，有客製化的需求需要自行撰寫。

- Circle：自動化建置工具
- Docker：迅速佈署環境工具
> Docker 是一個可以讓開發者簡單快速部署開發環境的工具，並且可以部署在任何地方(單機/遠端)

- K8S：管理 Docker Container 工具
- Helm：快速建置各環境 K8S 工具
- Grafana：機器數據監控工具
- ELK：Log 蒐集工具
- Telegram：通訊、錯誤通知工具
- Slack：通訊、錯誤通知工具

一般我們會希望這些工具通常具備以下功能：

- 最基本的建立、編輯腳本
- 相容各 Git 程式碼託管平台
- 視覺化呈現工作流( Workflow )、狀態( Status )和進度( Progress )
- 管理不同使用者權限( Maintainer, Developer, Reviewer, etc. )
- 可將任務分散給多個 Runner / Worker 執行
- 設定成功 / 失敗發送信件通知
- 支援第三方工具整合
- …

