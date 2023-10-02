---
title: Airflow Install
description: Airflow 安裝
published: true
date: 2023-09-28T00:31:44.255Z
tags: airflow
editor: markdown
dateCreated: 2023-05-23T09:18:19.253Z
---

# Airflow Install
- [ ] [Python & Airflow 學習筆記_環境架設](https://ithelp.ithome.com.tw/articles/10286480)
- [ ] [Running Airflow in Docker](https://airflow.apache.org/docs/apache-airflow/stable/howto/docker-compose/index.html)
- [ ] [How to install Apache Airflow on Docker? | Build Custom Airflow Docker Image | Airflow | Docker](https://www.youtube.com/watch?v=t4h4vsULwFE&ab_channel=BIInsightsInc&loop=0)
- [ ] [Airflow: Connect to Teradata using JDBC](https://anthony-f-tannous.medium.com/airflow-jdbc-teradata-7bdf17810092)
- [ ] [How to install java in an airflow container using docker-compose.yaml](https://stackoverflow.com/questions/67268054/how-to-install-java-in-an-airflow-container-using-docker-compose-yaml)
- [ ] [jTDS - SQL Server and Sybase JDBC driver Files](https://sourceforge.net/projects/jtds/files/jtds/1.3.1/)

# 安裝方式
Airflow 有本地部署（Running Airflow locally）和 Docker 部署（Running Airflow in Docker）兩種方式，本文件使用 Docker 部署

# 環境建置
## VsCode
- 目前使用 v1.78.2
- 虛擬環境：venv (受限於 airflow 必須使用 pip 進行安裝)

## Docker Desktop
- 安裝步驟參閱 [docker desktop install](/軟體開發/學習心得/11542/Devops/Docker/DockerDesktopInstall)
- 目前使用 v4.19.0

## Python
- 安裝步驟參閱 [python install](/軟體開發/學習心得/11542/Python/PythonInstall)
- Python 目前使用 v3.10.11
- pip 目前使用 v23.0.1

## apache/airflow
- 目前使用 v2.6.1
>  使用最新版會有資料庫連接的問題( `Connection timed out` )，需降版處理
{.is-warning}

# 安裝
## 專案資料夾
- 路徑：`D:\mandy\Project\ETL\airflow`
- git：`http://192.168.25.60:9001/11542/airflow`

## 初始化專案
- 建立一個虛擬環境 venv
- `.gitignore`：參閱 [toptal](https://www.toptal.com/developers/gitignore/api/python)，並新增 `logs`、`*.log`
- `.env`：
	- `AIRFLOW_UID=50000`

## 建立四個目錄，將會進行容器掛載以同步資料
```bash
mkdir -p ./config ./dags ./logs ./plugins
```
- `config`：可放置自定義的 log 參數，或新增 `airflow_local_settings.py` 配置集群策略
- `dags`：可放置 DAG 檔案
- `logs`：包含來自任務執行和排程的日誌
- `plugins`：可放置自定義的 plugin

## 新增 `docker-compose.yaml`，並做調整
- 到 [官網](https://airflow.apache.org/docs/apache-airflow/2.6.3/docker-compose.yaml) 下載
- 因需額外新增所需資料庫的 providers，故須 build 自己需要的 image，調整 `docker-compose.yaml` 如下：
```yaml
  # In order to add custom dependencies or upgrade provider packages you can use your extended image.
  # Comment the image line, place your Dockerfile in the directory where you placed the docker-compose.yaml
  # and uncomment the "build" line below, Then run `docker-compose build` to build the images.
  # image: ${AIRFLOW_IMAGE_NAME:-apache/airflow:2.6.1}
  build: .
```

## 新增 `requirements.txt`，將所需資料庫的 providers 寫入此檔
- 在此使用 jdbc 方式連接資料庫 SQL Server，故安裝 jdbc provider

```
apache-airflow-providers-docker==2.5.1
apache-airflow-providers-microsoft-mssql==3.4.0
apache-airflow-providers-mongo==3.2.0
apache-airflow-providers-postgres
alembic
apache-airflow-providers-jdbc
```

## 新增 JDBC Driver 到根目錄
### 連接 mssql 使用
- 到 [官網](https://sourceforge.net/projects/jtds/) 下載 `jtds-1.3.1-dist.zip`
- 將 zip 解壓縮，將資料夾中的 `jtds-1.3.1.jar` 複製到根目錄

## 新增 `dockerfile`
- 安裝 OpenJDK-11：`openjdk-11-jdk`
- 加載 JDBC Driver：`jtds-1.3.1.jar`
	- 路徑：`/usr/local/airflow/jars/jtds-1.3.1.jar`
  
```dockerfile
FROM apache/airflow:2.6.1

USER root

# Install OpenJDK-11 & JDBC Driver
RUN mkdir -p /usr/share/man/man1
RUN mkdir -p /etc/ssl/certs
RUN mkdir -p /usr/local/airflow/jars
COPY jtds-1.3.1.jar /usr/local/airflow/jars/jtds-1.3.1.jar
RUN apt-get update -y && \
    apt-get install -y openjdk-11-jdk && \
    apt-get autoclean && \
    apt autoremove
RUN apt-get update -y && \
    apt-get install -y ca-certificates-java && \
    update-ca-certificates -f && \
    apt-get -y autoclean && \
    apt autoremove

# Set JAVA_HOME
ENV JAVA_HOME /usr/lib/jvm/openjdk-11-jdk-amd64/
RUN export JAVA_HOME

USER airflow

WORKDIR /app

COPY requirements.txt /app

RUN pip install --trusted-host pypi.python.org -r requirements.txt

```

## 資料夾結構
```
│  .env                  # 環境變數檔
│  .env.example          # 環境變數範例檔
│  .gitignore            # git 忽略檔案
│  docker-compose.yaml   # docker compose 設定檔
│  dockerfile            # dockerfile
│  README.md             # 專案說明檔案
│  requirements.txt      # 安裝套件清單
│  jtds-1.3.1.jar      　# JDBC Driver
│
├─config      　   　   　# 放置自定義的 log 參數
├─dags      　   　   　　# 放置 DAG 檔案
├─logs      　   　   　　# 包含來自任務執行和排程的日誌
└─plugins      　   　   # 放置自定義的 plugin
```
## 進行資料庫初始化
```bash
docker compose up airflow-init
```

- redis Pulling
- postgres Pulling
- 建立自定義的 image：安裝 requirements 中指定的套件
- 初始化成功並建立一個登入帳密
```
airflow-init_1       | Upgrades done
airflow-init_1       | Admin user airflow created
airflow-init_1       | 2.6.1
start_airflow-init_1 exited with code 0
```

## 啟動 Airflow 服務
```bash
docker compose up -d
```

- 啟動成功
```bash
$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED              STATUS                             PORTS                    NAMES
61f01231cca1   airflow-airflow-webserver   "/usr/bin/dumb-init …"   58 seconds ago       Up 18 seconds (health: starting)   0.0.0.0:8080->8080/tcp   airflow-airflow-webserver-1
ef61ed4c3062   airflow-airflow-triggerer   "/usr/bin/dumb-init …"   58 seconds ago       Up 18 seconds (health: starting)   8080/tcp                 airflow-airflow-triggerer-1
8a8e6c7c0f06   airflow-airflow-worker      "/usr/bin/dumb-init …"   58 seconds ago       Up 19 seconds (health: starting)   8080/tcp                 airflow-airflow-worker-1
d2181e4dd859   airflow-airflow-scheduler   "/usr/bin/dumb-init …"   58 seconds ago       Up 18 seconds (health: starting)   8080/tcp                 airflow-airflow-scheduler-1
32a2bfd405d3   airflow-airflow-init        "/bin/bash -c 'funct…"   About a minute ago   Exited (0) 30 seconds ago                                   airflow-airflow-init-1
4a5acc516b1b   postgres:13                 "docker-entrypoint.s…"   About a minute ago   Up About a minute (healthy)        5432/tcp                 airflow-postgres-1
d43b48c2da80   redis:latest                "docker-entrypoint.s…"   About a minute ago   Up About a minute (healthy)        6379/tcp                 airflow-redis-1
```

## 可於 Docker Desktop 上查看運行狀況

![airflow runnimg on docker desktop.png](http://192.168.25.60:8000/files/file_storage/60b0bf4c.png)

## 登入 Airflow
- 訪問：`http://127.0.0.1:8080`
- 帳號：`airflow`
- 密碼：`airflow`

![airflow login.png](http://192.168.25.60:8000/files/file_storage/aa457970.png)

## 若遇到資料庫連線有問題須重新部屬時，須將 docker 中的 `volumn` 一起刪除
- docker 中的 `volumn`
	- airflow_postgres-db-volume
	- 70064156631cf33c87bda214f6fcc429ec434afbc7e4caf8d906d99ae8dc7a21
- 目前測試結果須指定安裝 `apache/airflow:2.6.1`
- 升版到 `apache/airflow:latest` 會有資料庫連線問題

![airflow docker mssql connect failed.png](http://192.168.25.60:8000/files/file_storage/bfa1d25f.png)

- 升版到最新再直接重新安裝 `apache/airflow:2.6.1` 會有問題，需將 docker 中的 `volumn` 一起刪除再重新安裝 `apache/airflow:2.6.1`

# 資料庫連線
## 新增資料庫連線
- Admin ➔ Connections ➔ 藍色 `+` 號(Add a new record)

![airflow add db connection.png](http://192.168.25.60:8000/files/file_storage/29dc8714.png)

- 填寫連線資訊
- 點選 `Test` 測試連線是否正常，若為正常，上方會出現綠色提示視窗 `Connection successfully tested`
- 點選 `Save` 儲存連線資訊

## airflow_postgres_conn
- 連接 Airflow 預設資料庫 Postgres
- 連線資訊
	- Connection Id：`airflow_postgres_conn`
  - Connection Type：選取 `Postgres`
  - Description：`airflow_postgres_conn`
  - Host：`postgres`
  - Schema：空白
  - Login：`airflow`
  - Password：`airflow`
  - Port：`5432`
  - Extra：空白

![airflow add db connection success.png](http://192.168.25.60:8000/files/file_storage/13a96d77.png)

```
2023-07-19 14:01:45 [2023-07-19T06:01:45.701+0000] {crypto.py:83} WARNING - empty cryptography key - values will not be stored encrypted.
2023-07-19 14:01:45 [2023-07-19T06:01:45.905+0000] {base.py:73} INFO - Using connection ID 'qCB5b0wu' for task execution.
2023-07-19 14:01:45 [2023-07-19T06:01:45.917+0000] {sql.py:375} INFO - Running statement: select 1, parameters: None
2023-07-19 14:01:45 [2023-07-19T06:01:45.918+0000] {sql.py:384} INFO - Rows affected: 1
2023-07-19 14:01:45 172.18.0.1 - - [19/Jul/2023:06:01:45 +0000] "POST /api/v1/connections/test HTTP/1.1" 200 68 "http://127.0.0.1:8080/connection/edit/2" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36"
2023-07-19 14:01:59 127.0.0.1 - - [19/Jul/2023:06:01:59 +0000] "GET /health HTTP/1.1" 200 141 "-" "curl/7.74.0"
```

## local_sql_server_conn
- 連接資料庫 SQL Server
- 連線資訊
	- Connection Id：`local_sql_server_conn`
  - Connection Type：選取 `JDBC Connection`
  - Description：`local_sql_server_conn`
  - Connection URL：`jdbc:jtds:sqlserver://192.168.25.21:1433/Leader`
  - Login：`sa`
  - Password：密碼
  - Driver Path：`/usr/local/airflow/jars/jtds-1.3.1.jar`
  - Driver Class：`net.sourceforge.jtds.jdbc.Driver`
  
![airflow add db JDBC connection success.png](http://192.168.25.60:8000/files/file_storage/971beaeb.png)

```
2023-07-24 08:23:10 [2023-07-24T00:23:10.700+0000] {base.py:73} INFO - Using connection ID 'cAS3xpZh' for task execution.
2023-07-24 08:23:13 127.0.0.1 - - [24/Jul/2023:00:23:13 +0000] "GET /health HTTP/1.1" 200 141 "-" "curl/7.74.0"
2023-07-24 08:23:15 [2023-07-24T00:23:15.684+0000] {sql.py:375} INFO - Running statement: select 1, parameters: None
2023-07-24 08:23:15 172.22.0.1 - - [24/Jul/2023:00:23:15 +0000] "POST /api/v1/connections/test HTTP/1.1" 200 68 "http://127.0.0.1:8080/connection/edit/2" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36"
```

## local_postgres_conn
- 連接資料庫 Postgres
- 連線資訊
	- Connection Id：`local_postgres_conn`
  - Connection Type：選取 `Postgres`
  - Description：`local_postgres_conn`
  - Host：`192.168.25.59`
  - Schema：`A_SY004`
  - Login：`11542`
  - Password：密碼
  - Port：`5432`
  - Extra：空白
  
![airflow add db Postgres connection success.png](http://192.168.25.60:8000/files/file_storage/f648366b.png)  
  
```
2023-07-21 15:00:14 [2023-07-21T07:00:14.989+0000] {base.py:73} INFO - Using connection ID 'Bv0n4uTH' for task execution.
2023-07-21 15:00:15 [2023-07-21T07:00:15.053+0000] {sql.py:375} INFO - Running statement: select 1, parameters: None
2023-07-21 15:00:15 [2023-07-21T07:00:15.069+0000] {sql.py:384} INFO - Rows affected: 1
2023-07-21 15:00:15 172.18.0.1 - - [21/Jul/2023:07:00:15 +0000] "POST /api/v1/connections/test HTTP/1.1" 200 68 "http://127.0.0.1:8080/connection/edit/3" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/115.0.0.0 Safari/537.36"
2023-07-21 15:00:18 127.0.0.1 - - [21/Jul/2023:07:00:18 +0000] "GET /health HTTP/1.1" 200 243 "-" "curl/7.74.0"
```
