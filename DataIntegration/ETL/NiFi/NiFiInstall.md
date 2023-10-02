---
title: NiFi Install
description: NiFi 安裝
published: true
date: 2023-09-28T00:31:13.076Z
tags: nifi
editor: markdown
dateCreated: 2023-07-06T00:50:19.373Z
---

# NiFi Install
- [ ] [Apache NIFI login issue after installation](https://stackoverflow.com/questions/68876855/apache-nifi-login-issue-after-installation)

# 安裝方式
NiFi 有本地部署（Running NiFi locally）和 Docker 部署（Running NiFi in Docker）兩種方式，本文件使用 Docker 部署

# 環境建置
## VsCode
- 目前使用 v1.78.2

## Docker Desktop
- 安裝步驟參閱 [docker desktop install](/軟體開發/學習心得/11542/Devops/Docker/DockerDesktopInstall)
- 目前使用 v4.19.0

## Python
- 安裝步驟參閱 [python install](/軟體開發/學習心得/11542/Python/PythonInstall)
- Python 目前使用 v3.10.11
- pip 目前使用 v23.0.1

## apache/nifi
- 目前使用 v1.20.0
>  使用最新版會有資料庫連接的問題( `Connection timed out` )，需降版處理
{.is-warning}


# 安裝
## 專案資料夾
- 路徑：`D:\mandy\Project\ETL\nifi`
- git：`http://192.168.25.60:9001/11542/nifi`

## 初始化專案
- 建立一個虛擬環境 venv
- `.gitignore`：參閱 [toptal](https://www.toptal.com/developers/gitignore/api/python)，並新增 `logs`、`*.log`
- `.env`：
	- `SINGLE_USER_CREDENTIALS_USERNAME=nifi`
  - `SINGLE_USER_CREDENTIALS_PASSWORD=nifi`

## 新增 `docker-compose.yaml`
```yaml
version: "3.9"
services:
  nifi:
      image: nifi-service
      container_name: nifi-service
      restart: always
      ports:
          - 8443:8443/tcp
          - 8082:8082/tcp
      env_file: ./.env
      environment:
          SINGLE_USER_CREDENTIALS_USERNAME: ${SINGLE_USER_CREDENTIALS_USERNAME}
          SINGLE_USER_CREDENTIALS_PASSWORD: ${SINGLE_USER_CREDENTIALS_PASSWORD}
      volumes:
          - "./jtds-1.3.1.jar:/usr/local/nifi/jars/jtds-1.3.1.jar"
      networks:
          - nifi-network
  nifi-registry:
      image: apache/nifi-registry:latest
      container_name: nifi-registry-service
      restart: always
      ports:
          - 18080:18080/tcp
      networks:
          - nifi-network
networks:
  nifi-network:
```

## 新增 `requirements.txt`
- 將所需的套件列出
```

```

## 新增 JDBC Driver 到根目錄
### 連接 mssql 使用
- 到 [官網](https://sourceforge.net/projects/jtds/) 下載 `jtds-1.3.1-dist.zip`
- 將 zip 解壓縮，將資料夾中的 `jtds-1.3.1.jar` 複製到根目錄

### 連接 postgresql 使用
- 到 [官網](https://mvnrepository.com/artifact/org.postgresql/postgresql/42.5.1) 下載 `postgresql-42.5.1.jar`
- 將 `postgresql-42.5.1.jar` 複製到根目錄

## 新增 `dockerfile`
- 加載 JDBC Driver：
	- `jtds-1.3.1.jar` 路徑：`/opt/nifi/nifi-current/lib/jtds-1.3.1.jar`
  - `postgresql-42.5.1.jar` 路徑：`/opt/nifi/nifi-current/lib/postgresql-42.5.1.jar`
  
```dockerfile
FROM apache/nifi:1.20.0

USER root

COPY jtds-1.3.1.jar /opt/nifi/nifi-current/lib/jtds-1.3.1.jar
COPY postgresql-42.5.1.jar /opt/nifi/nifi-current/lib/postgresql-42.5.1.jar
ADD requirements.txt /tmp/project/

RUN apt-get update && \
    apt-get install -y vim && \
    apt-get install -y python3 python3-pip && \
    pip3 install --no-cache-dir -r /tmp/project/requirements.txt && \
    rm -rf \
    /tmp/* \
    /var/lib/apt/lists/*
```

## 資料夾結構
```
│  .env                   　# 環境變數檔
│  .env.example           　# 環境變數範例檔
│  .gitignore             　# git 忽略檔案
│  docker-compose.yaml    　# docker compose 設定檔
│  dockerfile             　# dockerfile
│  README.md              　# 專案說明檔案
│  requirements.txt       　# 安裝套件清單
│  jtds-1.3.1.jar      　 　# JDBC Driver for mssql
│  postgresql-42.5.1.jar   # JDBC Driver for postgresql
└─
```

## 打包 nifi-service 映像檔
```bash
docker build -t nifi-service --no-cache .
```

## 啟動 NiFi 服務
```bash
docker compose up -d
```

## 可於 Docker Desktop 上查看運行狀況
啟動後的服務 Java 及 NiFi 位置如下
- Java home: `/opt/java/openjdk`
- NiFi home: `/opt/nifi/nifi-current`

![airflow running on docker desktop.png](http://192.168.25.60:8000/files/file_storage/36c69d7b.png)

## 取得登入帳密
- 點選 `nifi-service` 從 `Logs` 上面查看登入帳密，搜尋 `Generated Username`

```
2023-07-06 16:45:34 Generated Username [acfdd85c-8571-4bf0-821f-08e42947b8b5]
2023-07-06 16:45:34 Generated Password [amzYiE82vjjawxNaMUL5LsjsAyjLnGu3]
```

![nifi service login info search Generated Username.png](http://192.168.25.60:8000/files/file_storage/9d739715.png)


## 登入 NiFi
- 訪問：`https://localhost:8443/nifi/`
- 帳號：`acfdd85c-8571-4bf0-821f-08e42947b8b5`
- 密碼：`amzYiE82vjjawxNaMUL5LsjsAyjLnGu3`

![nifi login.png](http://192.168.25.60:8000/files/file_storage/6d593f33.png)
