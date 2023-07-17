---
title: Docker Usage
description: Docker學習筆記
published: true
date: 2023-07-14T00:10:26.108Z
tags: docker, devops
editor: markdown
dateCreated: 2022-07-21T06:34:46.172Z
---

# Docker Usage
- [ ] [30 天與鯨魚先生做好朋友](https://ithelp.ithome.com.tw/articles/10238216)
- [ ] [Docker CLI CheatSheet](https://devhints.io/docker)
- [ ] [Dockerfile CheatSheet](https://devhints.io/dockerfile)
- [ ] [docker-compose CheatSheet](https://devhints.io/docker-compose)
- [ ] [image 指令](https://blog.yowko.com/docker-images-command/)
- [ ] [docker-compose](https://hackmd.io/@tienyulin/docker-compose)
- [ ] [用Docker-compose建立與實作Web專案](https://lufor129.medium.com/docker-%E5%85%AD-%E7%94%A8docker-compose%E5%BB%BA%E7%AB%8B%E8%88%87%E5%AF%A6%E4%BD%9Cweb%E5%B0%88%E6%A1%88-a709a1a24382)
- [ ] [The Compose Specification](https://github.com/compose-spec/compose-spec/blob/master/spec.md)
- [ ] [The Compose Specification - Build support](https://github.com/compose-spec/compose-spec/blob/master/build.md)
- [ ] [Docker Kubernetes](https://wiki.opskumu.com/)
- [ ] [Docker Compose 的基本使用方式](https://magiclen.org/docker-compose/)
- [ ] [Docker筆記 - 讓資料遠離Container，使用 Volume、Bind Mount 與 Tmpfs Mount](https://medium.com/alberthg-docker-notes/docker%E7%AD%86%E8%A8%98-%E8%AE%93%E8%B3%87%E6%96%99%E9%81%A0%E9%9B%A2container-%E4%BD%BF%E7%94%A8-volume-bind-mount-%E8%88%87-tmpfs-mount-6908da341d11)
- [ ] [Day9 該如何將Docker Run 指令，轉換成Docker-compose內容](https://ithelp.ithome.com.tw/articles/10218042)
- [ ] [Docker restart](https://quietbo.com/2022/07/27/docker-restart/)
- [ ] [Use a restart policy](https://docs.docker.com/config/containers/start-containers-automatically/#use-a-restart-policy)
- [ ] [Docker Extensions Every Developer Must Try](https://faun.pub/docker-extensions-every-developer-must-try-7ce0d7a6c292)

# Docker Cheat Cheet
## [docker cheat sheet 1.pdf](http://192.168.25.60:8000/files/file_storage/ec6c1788.pdf)

![docker cheat sheet 1.png](http://192.168.25.60:8000/files/file_storage/9e523c63.png)

## [docker cheat sheet 2.pdf](http://192.168.25.60:8000/files/file_storage/8e8dff8a.pdf)

![docker cheat sheet 2.png](http://192.168.25.60:8000/files/file_storage/5d239601.png)

## [docker cheat sheet 3.pdf](http://192.168.25.60:8000/files/file_storage/dc4ecae4.pdf)

![docker cheat sheet 3.png](http://192.168.25.60:8000/files/file_storage/ed97ff18.png)


# Docker 基礎指令
## 打包 Docker 映像檔
```bash
docker build -t <image-name> --no-cache . 
```
> - image-name 需全部**小寫**
> - 最後一定要記得加 **.** 

- example

```bash
docker build -t e_board_api:0.1.0 --no-cache .
```

## 執行映像檔
```bash
docker run -p <app-port>:<docker-port> --restart=always  -v <本機實體絕對路徑>：<container 資料夾絕對路徑> <image-name:tag>
```
- 跑在背景
```bash
docker run -d --name <app-name> -p <app-port>:<docker-port> --restart=<policy> -v <本機實體絕對路徑>：<container 資料夾絕對路徑>  <image-name:tag>
```

- example

```bash
docker run -d --name e_board_api -p 8115:8115 --restart=always -v /var/log/eboard:/src/logs -v /home/mandychen/formal/e_board_API/db/sqlite.db:/src/sqlite.db e_board_api:0.1.0
```

### `--restart=<policy>`
- `no`：默認, 不要自動重啟容器。
- `on-failure[:max-retries]`：如果容器因錯誤退出，則重新啟動容器，這表現為非零退出代碼。或者，使用該:max-retries選項限制 Docker 守護程序嘗試重新啟動容器的次數。
- `always`：無論退出狀態如何，始終重新啟動容器。
- `unless-stopped`：無論退出狀態如何，始終重啟容器，包括在守護進程啟動時，除非容器在 Docker 守護進程停止之前進入停止狀態。

### `-v <本機實體絕對路徑>：<container 資料夾絕對路徑>`
- 使用 **bind mount**，會將位於本機上的指定檔案或目錄讓容器掛載並做映射，會自動建立本機的資料夾，但還需做資料夾讀寫權限設定
- container 資料夾絕對路徑：預設會放置在 src 底下
- example
```
-v /var/log/eboard:/src/logs -v /data/eboard/db/sqlite.db:/src/sqlite.db
```

## 列出本機映像檔
```bash
docker images
```
```bash
docker image ls -a
```
## 檢視所有容器狀態
```bash
docker ps -a
```

## 停止容器
```bash
docker stop <container-ID>
```

## 啟動一個已停止的容器
```bash
docker start <container-ID>
```

## 刪除映像檔
```bash
docker rmi <image-name>
```
```bash
docker rmi <image-ID>
```

## 刪除容器
```bash
docker rm -f <container-ID>
```
```bash
docker rm <container-name>
```

## 查看磁盤使用情况
```bash
docker system df
```
```bash
df -h
```
## 清理沒有使用的數據，包括 image、已停止的 container 
```bash
docker system prune
```
```bash
docker system prune --all --force --volumes
```

## 進入到 container
```bash
docker exec -it <container-name> /bin/bash
```

- 離開 container

```bash
exit
```

# Dockerfile

[時區設定](https://ithelp.ithome.com.tw/articles/10205977)

```dockerfile
# Stage 1: Decide base image
# 決定基底映像檔
FROM node:14

# Stage 2: Set Timezone
# 設定容器時區
ENV TZ=Asia/Taipei
RUN cp /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Stage 3: Set the working directory
# 設置目前工作目錄
WORKDIR /src

# Stage 4: Install requirements
# 複製檔案至映像檔內
COPY . .
# 安裝 Dependencies
RUN npm install && npm cache clean --force
# 生成 Prisma Client
RUN npx prisma generate --schema=./src/prisma/schema.prisma

# Stage 5: Set container listen Port
# 設定 Docker 要開放的 Port
EXPOSE 10000

# Stage 6: Run app
# 啟動 Container 時要執行的指令
CMD [ "node", "src/app" ]
```


# Docker 佈署
## 檢查 package.json 是否有移除套件 nodemon
套件 nodemon 會消耗大量資源，故佈署時需進行移除

## 將專案複製一份到 ubuntu
### 方法一：git clone 專案到 ubuntu 
```bash
git clone http://192.168.25.60:9001/11542/WebSignin_API.git
```
### 方法二：使用 WinSCP 將專案傳輸到 ubuntu

![winscp 檔案傳輸.png](http://192.168.25.60:8000/files/file_storage/f5b1dc3c.png)

## 預設在和 Dockerfile 檔案同層的資料夾底下執行
```bash
cd WebSignin_API
```

## 打包 Docker 映像檔
```bash
docker build -t websignin_api:0.1.0 --no-cache . 
```

- 其他範例

```bash
docker build -t e_board_api:1.1.1 --no-cache .
```

![docker build.png](http://192.168.25.60:8000/files/file_storage/b529c34e.png)

打包成功會顯示以下訊息
```
Successfully built b7ce160a4f5f
Successfully tagged websignin_api:0.1.0
```

## 列出本機的映像檔
```bash
docker images
```

可查看到已經成功打包的映像檔

![docker images.png](http://192.168.25.60:8000/files/file_storage/66c1eed2.png)

## 執行映像檔
```bash
docker run -p 10000:10000 websignin_api:0.1.0
```
- 跑在背景
```bash
docker run -d --name websignin_api -p 10000:10000 websignin_api:0.1.0
```

- 其他範例
```bash
docker run -d --name e_board_api -p 8115:8115 --restart=always -v /var/log/eboard:/src/logs -v /Volume/eboard/db/sqlite.db:/src/sqlite.db -v /Volume/eboard/.env:/src/.env  e_board_api:1.1.1
```

成功執行映像檔

![docker run.png](http://192.168.25.60:8000/files/file_storage/bfe11cbc.png)

## 檢視所有容器狀態
```bash
docker ps -a
``` 

查看容器

![docker ps -a.png](http://192.168.25.60:8000/files/file_storage/32cc69b3.png)

## 設定 ubuntu 與本機 IP 網路

![ubuntu 新增連接埠傳送規則 docker app 與本機 IP.png](http://192.168.25.60:8000/files/file_storage/5f79c7f0.png)

## 測試專案是否成功運作
### 連接 http://192.168.25.180:10000/

### 使用 Postman 測試 API

# docker-compose 基礎指令
## 建立，背景執行
- 會直接掃描當前目錄下的 docker-compose.yml 檔並進行 build 所有的 image 並建立好 container
- `-d`：背景執行
```bash
docker-compose up -d
```

![docker-compose up -d.png](http://192.168.25.60:8000/files/file_storage/b6c6baa1.png)

執行完後會出現三個 container 包再一起，他們彼此網路是互相連接的，我們稱做 docker stack

## 停止
```bash
docker-compose stop  
```
## 啟動
```bash
docker-compose start  
```
## 重啟
```bash
docker-compose restart  
```
## 移除
```bash
docker-compose rm  
```
## 查看 log
```bash
docker-compose logs -f 
```
## 移除所有 container 與 image
```bash
docker compose down --volumes --rmi all
```
## docker 與 docker-compose 比較

|       用途       |                    指令                  |  常用可選參數 |      類似指令      |
|:---------------:|:----------------------------------------:|:-----------:|:------------------:|
| 建立並前景執行容器 |            `docker-compose up`           | `-d`、`-f`  |    `docker run`    |
|    查看容器運作   |            `docker-compose ps`           |    `-f`     |    `docker ps`     |
|     執行容器     |            `docker-compose start`        |    `-f`     |   `docker start`   |
|     停止容器     |            `docker-compose stop`         |    `-f`     |   `docker stop`    |
|     刪除容器     |            `docker-compose rm`         |    `-f`     |     `docker rm`    |
|     印出log     |            `docker-compose logs`         |    `-f`     |    `docker logs`   |
|     進入容器     |  `docker-compose exec <service> bash`    |    `-f`     |   `docker exec -it <id` or `container-name> bash`   |

# Docker Compose
> - Docker Compose 是用來組合多個 container 成為一個完整服務的工具。

- Docker Compose 是一種用來規劃以及管理多容器開發的工具，透過 docker-compose 所需要的 yaml 檔來進行服務的設定，接著利用 compose 的指令即可啟動我們所需的所有 Containers 並設定好其服務，除此之外，也可用 compose 來區分不同 environments (dev, test, uat, stg, prd)，得以配合團隊的軟體開發流程。
- 先前在說明如何連結 container 時，已經有示範過連結 container 的基本方法。雖然可行，但要執行非常多指令才能把 container 串起來。
- Docker Compose 不只可以做到一樣的事，而且它使用 YAML 描述檔定義 container 的關係，簡化定義的過程，同時也實現了 IaC，讓 container 的關係可以簽入版本控制。
- Docker Compose 最終結果是啟動 container，底層一樣會使用 docker run 指令，因此 Docker Compose 的設定參數會與 docker run 的選項和參數非常相似。

![docker compose slogan.png](http://192.168.25.60:8000/files/file_storage/32b7b9b7.png)

- Docker Compose 你可以如封面，理解成一個章魚小幫手，幫你把許許多多的 Container 建立起來。當我們程式修改後執行 docker-compose 命令可以自動抓到哪一個 image 改變，並專門為其重新 build 、 run ，大大增加了部屬的效率。

![docker compose usage.png](http://192.168.25.60:8000/files/file_storage/127a9492.png)

## 使用步驟
1.利用 Dockerfile 定義應用程式的環境，以便於在任何地方重新複製 ( reproduce anywhere )。
2.利用 docker-compose.yml 定義組成應用程式的服務，使它們能在隔離的環境中一起被執行。
3.執行 `docker-compose up` 開始執行整個應用程式。

# docker-compose.yml
> - 撰寫 docker-compose.yml 來設定你的 services 該指向哪些 image、Port 對應、該走哪些任務
> - docker-compose.yml 使用 YAML 格式，縮排很重要，在撰寫時必須注意

- 範例

```yaml
version: "3.9"
services:
  frontend:
    build: ./html
    image: myweb:latest
    ports:
      - "8085:80"
    container_name: run_myweb
  backend:
    build: ./app
    image: mynode:latest
    ports:
      - "4200:3000"
    container_name: run_mynode
  db:
    image: postgres
    ports:
      - "5432:5432"
    container_name: my_postgres
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: password
      PGDATA: /var/lib/postgresql/data/pgdata
    volumes:
      - pgdata:/var/lib/postgresql/data/
volumes:
  pgdata:
```

## version
- 指定 yml 使用哪個版本的 compose

## build
- 建立映像檔的相關設定
- 指定 Dockerfile 所在檔案的路徑（可以是絕對路徑，或者相對 docker-compose.yml 文件的路徑）
- Compose 將會利用它自動建立這個 image，然後使用這個 image 

### context
- 指令指定 Dockerfile 所在文件夾的路徑

### dockerfile
- 指定 Dockerfile 文件名

## image
- 指定為映像檔名稱或映像檔 ID
- 如果映像檔在本地不存在，Compose 將會嘗試拉取這個映像檔或依照 dockerfile 建立 image
- `image:version`：可指定 image 版本號
## container_name
- 指定容器名稱。默認將會使用 項目名稱_服務名稱_序號 這樣的格式
- `container_name：docker-web-container`

> 注意：指定容器名稱之後，該服務將無法進行擴展（scale），因為 Docker 不允許多個容器聚有相同的名稱
{.is-warning}

## ports
- Port 對應，或者僅指定容器的 Port ( Host 將會隨機選擇 Port )都可以
- `Host Port：Container Expose Port`

```yaml
ports:
 - "3000"
 - "8000:8000"
 - "49100:22"
 - "127.0.0.1:8001:8001"
```

 > 注意：當使用 `Host:Container` 格式來映射 Port 時，如果你使用的容器 Port 小於 60，並且沒有放到 "" 中，可能會得到錯誤訊息，因為 yaml 會自動解析 xx:yy 這種數字格式為 60 進制。為避免出現這種問題，建議數字串都採用 "" 括起來的字符串格式
{.is-warning}

## volumes
- 使用 **bind mount**，會將位於本機上的指定檔案或目錄讓容器掛載並做映射，會自動建立本機的資料夾，但還需做資料夾讀寫權限設定
- `本機實體絕對路徑：container 資料夾絕對路徑`
- container 資料夾絕對路徑：預設會放置在 src 底下

```yaml
    volumes:
      - "/var/log/logs/websignin_fastify_admin_api_logs/logs:/src/logs"
```

- 查看是否映射成功可查看 container log 是否有錯誤訊息

```bash
# 查看 container log
docker logs -f websignin_fastify_admin_api 
```

- 以下錯誤訊息表示本機資料夾沒有讀寫權限，故無法寫入資料

![docker logs -f container id.png](http://192.168.25.60:8000/files/file_storage/f5a67bcc.png)

### 資料夾讀寫權限設定 777，owner，群組用戶及其他用戶可讀/寫/執行 (在最底層的資料夾建權限就好)
- 到本機實體絕對路徑底下

```bash
cd /var/log/logs/websignin_fastify_admin_api_logs/logs
```

- 手動建立兩個資料夾，放置 log 使用
```bash
sudo mkdir error info
```

- 資料夾讀寫權限設定 777，原本會是 755 ( owner 可讀/寫/執行, 群組用戶及其他用戶可讀/執行)
```
sudo chmod 777 error info
```

![winscp 查看資料夾屬性權限.png](http://192.168.25.60:8000/files/file_storage/4339cd13.png)

- 執行 `docker-compose up -d` 後再查看資料夾是否有產生資料

![winscp docker bind mount 映射出 log 檔案.png](http://192.168.25.60:8000/files/file_storage/f341d4c4.png)

## depends_on
- 設定啟動順序，docker compose 可以一次啟動多個 Container，但是有些 Container 之間是有關連性的就必須有順序性的啟動。

```yaml
    depends_on:
      - websignin_fastify_core_api
```

# 使用 docker-compose 佈署
## 在專案的根目錄中，建立名為 docker-compose.yml 的檔案。
- docker-compose.yml

```yaml
# 使用 3.9 版的設定檔，通常新版本會有新的功能，並支援新的設定參數
version: "3.9"
# 定義 service 的區塊，一個 service 設定可以用來啟動多個 container
services:
  # 共用函式庫
  websignin_fastify_core_api:
    build:
      context: ./WebSignin_Fastify_Core_API
      dockerfile: dockerfile
    image: websignin_fastify_core_api
    container_name: websignin_fastify_core_api
    ports:
      - "10003:10003"
    volumes:
      - "/var/log/logs/websignin_fastify_core_api_logs/logs:/src/logs"
  # 線上打卡系統前台
  websignin_fastify_api:
    build:
      context: ./WebSignin_Fastify_API
      dockerfile: dockerfile
    image: websignin_fastify_api
    container_name: websignin_fastify_api
    ports:
      - "10001:10001"
    volumes:
      - "/var/log/logs/websignin_fastify_api_logs/logs:/src/logs"
    depends_on:
      - websignin_fastify_core_api
      - redis
  # 線上打卡系統後台  
  websignin_fastify_admin_api:
    build:
      context: ./WebSignin_Fastify_Admin_API
      dockerfile: dockerfile
    image: websignin_fastify_admin_api
    container_name: websignin_fastify_admin_api
    ports:
      - "10004:10004"
    volumes:
      - "/var/log/logs/websignin_fastify_admin_api_logs/logs:/src/logs"
    depends_on:
      - websignin_fastify_core_api
      - redis
  # 線上打卡系統後台(正式)  
  websignin_fastify_formal_admin_api:
    build:
      context: ./WebSignin_Fastify_Formal_Admin_API
      dockerfile: dockerfile
    image: websignin_fastify_formal_admin_api
    container_name: websignin_fastify_formal_admin_api
    ports:
      - "10005:10005"
    volumes:
      - "/var/log/logs/websignin_fastify_formal_admin_api_logs/logs:/src/logs"
    depends_on:
      - websignin_fastify_core_api
      - redis
  # 線上打卡系統前台(正式)  
  websignin_fastify_formal_api:
    build:
      context: ./WebSignin_Fastify_Formal_API
      dockerfile: dockerfile
    image: websignin_fastify_formal_api
    container_name: websignin_fastify_formal_api
    ports:
      - "10006:10006"
    volumes:
      - "/var/log/logs/websignin_fastify_formal_api_logs/logs:/src/logs"
    depends_on:
      - websignin_fastify_core_api
      - redis
  # line bot
  openai_line_bot:
    build:
      context: ./OpenAI_Line_Bot
      dockerfile: dockerfile
    image: openai_line_bot
    container_name: openai_line_bot
    ports:
      - "10007:10007"
    volumes:
      - "/var/log/logs/openai_line_bot_logs/logs:/src/logs"
    depends_on:
      - websignin_fastify_core_api
      - redis
  # 資訊看板系統後台
  information_board_admin_api:
    build:
      context: ./Information_Board_Admin_API
      dockerfile: dockerfile
    image: information_board_admin_api
    container_name: information_board_admin_api
    ports:
      - "10008:10008"
    volumes:
      - "/var/log/logs/information_board_admin_api_logs/logs:/src/logs"
    depends_on:
      - websignin_fastify_core_api
      - redis
  # 資訊看板系統前台
  information_board_api:
    build:
      context: ./Information_Board_API
      dockerfile: dockerfile
    image: information_board_api
    container_name: information_board_api
    ports:
      - "10009:10009"
    volumes:
      - "/var/log/logs/information_board_api_logs/logs:/src/logs"
      - "/data/sqlite_data/sqlite.db:/src/sqlite.db"
    depends_on:
      - websignin_fastify_core_api
      - redis
  redis:
    restart: always
    container_name: redis
    image: redis:alpine
    ports:
      - "6379:6379"
    volumes:
      - "/data/redis_data:/data"
```

## 執行 docker-compose
- 到有 docker-compose.yml 路徑下，才能下指令

![docker-compose up -d.png](http://192.168.25.60:8000/files/file_storage/b6c6baa1.png)

- 建立，背景執行
```bash
docker-compose up -d
```

# Docker 異常處理
1. 當 Docker 容器出現異常，無法重啟、移除或暫停時，Docker 容器狀態為 create，請使用下列指令重啟 Docker
```shell
sudo service docker restart
```
2. 然後再將有異常的 Docker 容器停止或移除，若是以 Docker Compose 方式啟動容器的話，在 Docker 服務重啟後，再使用下列指令移除異常 Docker 容器
``` shell
sudo docker-compose down
``` 
