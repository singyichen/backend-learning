---
title: Processor Group
description: 
published: true
date: 2023-07-13T08:45:10.330Z
tags: nifi
editor: markdown
dateCreated: 2023-07-13T08:16:42.008Z
---

# Processor Group
> 當 Date Pipeline 複雜且多樣化時，有些小流程可能會有重複的建置，此時可以透過 Processor Group(接下來簡稱 PG) 來做處理，簡單來說就是變成 Module，然後整合到 Data Pipeline 來做一個重複使用的功能。

PG 除了有作為 Module 的功能之外，他也有幾個額外重點，其實之前都有提到他，這邊再稍微列一下給各位：

- 可以用來做 Team 或 Project 的劃分，加強組織管理與權限控管。
- NiFi Registry 版本控管的最小單位。

# 新增 Processor Group
從 gif 檔可以看到如何建立 PG，而當我們移動到 PG 內部時，可以注意到左下角也會改變成我們目前所屬的階層(很像 Folder 的概念)。

![nifi add processor group.gif](http://192.168.25.60:8000/files/file_storage/e6962e9b.gif)


# 設定 Processor Group
## 設定 Input Port 與 Output Port

## 加入 SplitRecord Processor

## 加入 UpdateRecord Processor

## 加入 EvaluateJsonPath Processor

## 加入 RouteOnAttributes Processor

# 對接 Processor Group