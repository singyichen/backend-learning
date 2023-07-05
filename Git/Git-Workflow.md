---
title: Git Workflow
description: 
published: true
date: 2023-06-19T00:38:33.187Z
tags: git
editor: markdown
dateCreated: 2023-03-14T00:53:20.145Z
---

# Git Workflow
- [ ] [Git Workflow & Immutability](https://blog.bytebytego.com/p/ep46-step-by-step-guide-on-system?utm_source=profile&utm_medium=reader2)

![Git flow.png](http://192.168.25.60:8000/files/file_storage/325778fc.png)

![Git Workflow & Immutability.png](http://192.168.25.60:8000/files/file_storage/d1212fab.png)

# Git Workflow 工作流程
- 相關 git 指令參閱 [Git 指令列表](http://192.168.25.60:9005/zh-tw/軟體開發/學習心得/11202/Git-CLI)
- [提交線圖(精簡版) example](http://192.168.25.60:9001/11542/Actual_GitWorkFlow/graph)

![git actual workflow graph.png](http://192.168.25.60:8000/files/file_storage/53d1c72b.png)

- [提交線圖 example](http://192.168.25.60:9001/11542/Git_WorkFlow/graph)

![git graph bug.png](http://192.168.25.60:8000/files/file_storage/3041dbb4.png)
![git graph feature.png](http://192.168.25.60:8000/files/file_storage/84f1c08b.png)

# 建立 dev 分支 (開發)
## 取得專案遠端連線網址

ex： http://192.168.25.60:9001/Project/e_board_API

## git 初始化
```shell
git init
```

## 設定遠端連線
```shell
git remote add origin http://192.168.25.60:9001/Project/e_board_API
```

## 程式碼提交 ( commit code )
```shell
git add .
git commit -m "first commit"
```

## 推送至 master 分支
```shell
git push -u origin master
```

## 建立 dev 分支並切換到 dev 分支
```shell
git checkout -b dev 
```

## 推送至 dev 分支
```shell
git push -u origin dev
```

## 將 master 分支進行保護
- 專案 ➔ 設定 ➔ 分支 ➔ 分支保護選擇 master ➔ 啟用分支保護 ➔ 啟用推送

## 將 dev 設定成預設分支 ( 提交程式碼和合併請求的預設分支 )
- 專案 ➔ 設定 ➔ 分支 ➔ 預設分支選擇 dev

![git 分支預設分支及分支保護.png](http://192.168.25.60:8000/files/file_storage/97b91a16.png)


# 開發 feature 流程
## 在 feature 看板取得開發 feature 任務編號
- ex：[資訊看板](http://192.168.25.60:9025/?controller=BoardViewController&action=show&project_id=4)
- 任務#01：後台 api 機房輪值模組
- 將任務移至 **進行中**

## 切換到 dev 分支
```shell
git checkout dev
git pull
```

## 建立 feature 分支並切換到 feature 分支
- 分支名稱：`feature＋任務編號`

```shell
git checkout -b feature01 
```

## 開發 feature ，自測完並 commit
- 參閱 [Git Commit 撰寫格式](http://192.168.25.60:9005/zh-tw/%E8%BB%9F%E9%AB%94%E9%96%8B%E7%99%BC/%E8%A8%AD%E8%A8%88%E8%A6%8F%E7%AF%84/spec01)
```shell
git commit -m "message"
```
- 將任務移至 **測試**

## 推送至 feature 遠端分支
```shell
git push -u origin feature01
```

## 合併 feature 分支到 dev 分支
```shell
git checkout dev
git merge feature01 --no-ff
```

## 推送至 dev 分支
```shell
git push -u origin dev
```

## 刪除本地 feature 分支
```
git branch -d feature01
```

## 刪除遠端 feature 分支
```
git push origin -d feature01
```

## 將任務移至**完成**

# 任務全部完成之後進行部屬及團隊內測
- 後端：docker 部屬到 192.168.25.60 
- 前端：build 完的專案檔部屬到 192.168.25.60

# 修復 bug 流程
## 在 issue 看板取得修復 bug 任務編號
- ex：[資訊看板-Issues](http://192.168.25.60:9025/?controller=BoardViewController&action=show&project_id=5&search=status%3Aopen)
- 任務#94：A0009-重複登入失敗(飲水紀錄按鈕)
- 將任務移至 **進行中**

## 切換到 dev 分支
```shell
git checkout dev
git pull
```

## 建立 bug 分支並切換到 bug 分支
- 分支名稱：`bug＋任務編號`

```shell
git checkout -b bug94
```

## 修復 bug，自測完並 commit
- 參閱 [Git Commit 撰寫格式](http://192.168.25.60:9005/zh-tw/%E8%BB%9F%E9%AB%94%E9%96%8B%E7%99%BC/%E8%A8%AD%E8%A8%88%E8%A6%8F%E7%AF%84/spec01)
```shell
git commit -m "message"
```
- 將任務移至 **開發者自我測試**

(多人協作時需多做以下步驟-起始)
## 更新 dev 分支到最新版本
```shell
git checkout dev
git pull
```

## 合併 dev 分支到 bug 分支
- 此步驟為確保 dev 皆包含於 bug 分支
- 有衝突時解衝

```shell
git checkout bug94
git merge dev --no-ff
```

## 推送至 bug 遠端分支
```shell
git push -u origin bug94
```
(多人協作時需多做以下步驟-結束)

## 合併 bug 分支到 dev 分支
```shell
git checkout dev
git merge bug94 --no-ff
```

## 推送至 dev 分支
```shell
git push -u origin dev
```

## 進行部屬及 bug 複測
- 後端：docker 部屬到 192.168.25.60 
- 前端：build 完的專案檔部屬到 192.168.25.60
- 將任務移至 **部屬與團隊內測**

## 複測人員測試通過
- 將任務移至 **完成**

## 刪除本地 bug 分支
```
git branch -d bug94
```

## 刪除遠端 bug 分支
```
git push origin -d bug94
```

# 上線流程
## 切換到 master 分支
```shell
git checkout master
```

## 合併 dev 分支到 master 分支
```shell
git merge dev --no-ff
```

## 建立 tag 
```shell
git tag -a v1.0.0 -m "Release version 1.0.0"
```

## 推送 tag 
```shell
git push origin --tags
```

## 推送至 master 分支
```shell
git push -u origin master
```

## 到 git 裡面發佈新版本 release

### 選擇要發布新版本的 tag

![git create new release.png](http://192.168.25.60:8000/files/file_storage/fa607726.png)

### 建立 master 分支的新版本
範例
- 標題：`v1.0.0`
- 內容：
```
此版本提供台灣日期時間 API 供給需要的專案使用

- 台灣日期時間( NTP )
- 台灣日期時間( DB )
```
![git create master branch release.png](http://192.168.25.60:8000/files/file_storage/188780df.png)

### 新版本 release

![git master branch release.png](http://192.168.25.60:8000/files/file_storage/2efcc9dd.png)

## 將 master 設定成預設分支 ( 提交程式碼和合併請求的預設分支 )
- 專案 ➔ 設定 ➔ 分支 ➔ 預設分支選擇 master

## 刪除本地 dev 分支
```
git branch -d dev
```

## 刪除遠端 dev 分支
```
git push origin -d dev
``` 

## 進行部屬及測試
- 後端：docker 部屬到 192.168.25.60 
- 前端：build 完的專案檔部屬到 192.168.25.60
