---
title: Git
description: 
published: true
date: 2023-05-24T01:28:26.589Z
tags: git
editor: markdown
dateCreated: 2022-07-04T07:07:03.502Z
---

# Git
- [ ] [How does Git Work](https://blog.bytebytego.com/p/ep-40-git-workflow?utm_source=profile&utm_medium=reader2)
- [ ] [Git Merge vs. Git Rebase: What are the differences?](https://blog.bytebytego.com/p/ep-43-8-data-structures-that-power?utm_source=profile&utm_medium=reader2)
- [ ] [How Git commands work](https://blog.bytebytego.com/p/ep49-api-architectural-styles?lli=1&utm_source=profile&utm_medium=reader2)

![How does Git Work.png](http://192.168.25.60:8000/files/file_storage/ea724cbd.png)

![Git Merge vs Git Rebase.png](http://192.168.25.60:8000/files/file_storage/2ddc78de.png)

![How Git commands work.png](http://192.168.25.60:8000/files/file_storage/866c4f8b.png)

## 記錄什麼？
記錄檔案內容變化
## 為什麼要做版控
保留歷史紀錄
## 分散式版控
- 每個人都有完整的儲存庫。Ex.Git
- 不需要 push 也可以拿到資料
## 集中式版控
- 只有server端有正式的儲存庫。 Ex.CVS、SVN、TFVC
- 統一傳送到 server ,可以做權限控制

# 基礎介紹
## 安裝與設定
### 安裝
- notepad++
- powershell
- 把預設分支由 main 改成 master
### 設定
- system < global < local
- 設定使用者名稱與電郵
```shell
git config --global user.name "陳欣怡"
git config --global user.email "mandy.chen@hokia.com.tw"
```
- 查看 global 使用者名稱及電子郵件
```shell
git config --global --list
```
## 新增 .gitignore 檔案
- 可以透過 [toptal](https://www.toptal.com/developers/gitignore) 依照不同語言生成 .gitignore
- 上傳至 git 時需忽略的檔案清單
## 三種模式
工作區域 git add 暫存區 git commit 儲存庫 push 遠端
### 工作區域：未追蹤的檔案
```shell
// 將檔案不追蹤
git rm --cached
```
### 暫存區：將要提交的檔案
### 儲存庫(本機)
## 常見的情境
### 忽略已存在的檔案
新增一個.gitignore 放置不需要進版控的檔案


```shell
// 初始化:將工作目錄加到版控
git init
```
可以使用 git clone 一個 Repository 可以不需要 git init 也可以使用 git

```shell
// 查詢工作目錄在 git 中的狀態
git status
```

```shell
// 把單一或多個檔案加到索引暫存區
git add . 
```

```shell
// 提交
git commit
git commit -m "info"
```

```shell
// 重新提交變更
git commit --amend
```
- 修改的部分紀錄會保留，但會等到 git GC 時清除
- SHA1 值會更新

```shell
// 查看歷史紀錄
git log
// 
git log --stat
// 簡化 log 輸出紀錄
git --pretty=oneline
```


```shell
// 刪除檔案,直接變更到暫存區
git rm -f
// 僅將檔案
git rm --cached 

```
# 分支
## 為什麼要使用分支
產生不同的產品線 (類似標籤)
- 開發模式 dev (底定功能)
- 產品發佈模式

## 使用時機
- 開發新功能
- 多人協作

## 原理
- 不是複製檔案出來，而是貼不同的標籤
- master、dev

## 基本用法

```shell
// 建立分支但不會直接切換到分支
git branch 分支名稱
```

```shell
// 建立並切換到指定分支
git checkout -b 分支名稱
```

```shell
// 合併
git merge 要合併進來的分支名稱
```

```shell
// fast-forwards 快轉:不保留分支線歷程
git merge
// non-fast-forwards 不快轉:保留分支線歷程
git merge 分支 --no-ff
```

```shell
// 二進制檔案合併處理
// 以 ours 為主合併
git checkout --ours
// 以 theirs 為主合併
git checkout --theirs
```

```shell
// 增加 Git 衝突合併工具設定，讓 Git 顯示出衝突的位置
git config --global merge.tool meld
git config --global merge.conflictstyle diff3
git config --global mergetool.prompt false
```


# 遠端協作
## 如何 push 上傳到 github

```shell
// push 前的設定:加入遠端連線設定
git remote add 遠端儲存庫名稱 <repository_url>
```

```shell
// 推送指定分支到指定的遠端分支
git push origin <local_branch_name>:<remote_branch_name>
```

## pull 與 fetch 的差異
git pull = git fetch + git merge
- pull 是快轉的行為所以看不到 merge 的歷程

```shell
// 更新並合併資料
git pull
```

```shell
// 更新資料
git fetch
// 合併資料，不快轉保留歷程
git merge origin/master --no-ff
```

```shell
// 更新資料
git fetch
```

# 改寫提交
```shell
// 回復到前 n 版本
git reset head~n
```




