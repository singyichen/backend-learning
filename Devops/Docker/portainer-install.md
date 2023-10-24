---
title: Portainer Install
description: Docker學習筆記
published: true
date: 2023-10-20T01:32:15.944Z
tags: docker
editor: markdown
dateCreated: 2023-10-19T09:23:12.000Z
---

# Portainer Install
- [ ] [portainer](https://docs.portainer.io/start/install-ce/server/docker/linux)
- [ ] [Portainer 環境建置 - Docker 伺服器端最易上手的 GUI 管理工具](https://www.casper.tw/development/2023/10/11/portainer/)

# Portainer 介紹
> Portainer 是一個開源的 Docker 容器 GUI 管理工具。

## 簡介
Portainer 是一個開源的輕量級容器管理工具，用於簡化容器的部署、管理和監控。它提供一個直觀的 Web 介面，讓使用者可以方便地管理 Docker、Kubernetes 和 Swarm 等容器平台。

## 功能
使用 Portainer，你可以輕鬆地進行以下操作：
- 創建、啟動和停止容器
- 檢視容器的日誌和狀態
- 管理映像檔，包括拉取、推送和刪除映像檔
- 設定容器網絡、卷和環境變數
- 配置容器的資源限制和訪問控制
- 監控容器和主機的性能指標
- 管理容器編排平台，如創建和管理 Swarm 集群、部署和擴展 Kubernetes 應用等

Portainer 是一個易於安裝和使用的工具，適用於個人開發者、小型團隊和企業用戶。它支援多種平台和部署方式，包括本地安裝、容器化部署和雲服務集成。

總結來說，Portainer 是一個方便、可視化的容器管理工具，讓容器的管理和操作變得更加簡單和可視化。

# Portainer 安裝
## 安裝
- 建立 Portainer Server 將用來儲存其資料庫的磁碟區
```shell
docker volume create portainer_data
```

- 下載並安裝 Portainer Server 容器
- 多一個 port `9000` 對應，可以使用 `http` 連線
- port `9443`，使用 `https` 連線
```shell
docker run -d -p 8000:8000 -p 9000:9000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
```
## 設定 VirtualBox 上的 Port 轉發功能
- 設定 portainer 為 9000

![ubuntu 新增連接埠傳送規則 portainer 與本機 IP.png](http://192.168.25.60:8000/files/file_storage/822080a9.png)

## 登入
- 首次連線 http://127.0.0.1:9000/#!/init/admin
- 首次登入會建立 admin 的帳號，輸入密碼之後，需 restart container 才可登入
- 連線 http://127.0.0.1:9000/#!/auth




