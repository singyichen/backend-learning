---
title: Separate Project Method
description: 使用共用函式庫
published: true
date: 2023-05-17T01:46:19.113Z
tags: git, docker, fastify
editor: markdown
dateCreated: 2022-09-07T07:44:21.419Z
---

# Separate Project Method
- [ ] [Three Ways to Share Node.js Modules Across Multiple Projects](https://reverentgeek.com/3-ways-to-share-nodejs-modules-across-multiple-projects/)
- [ ] [npm-install](https://docs.npmjs.com/cli/v7/commands/npm-install)
- [ ] [使用 SSH 金鑰與 GitHub 連線](https://hackmd.io/@CynthiaChuang/Generating-a-Ssh-Key-and-Adding-It-to-the-Github)
- [ ] [GitHub - 使用 SSH 來 push commit 吧!](https://ithelp.ithome.com.tw/articles/10254127)
- [ ] [Set up an SSH key](https://support.atlassian.com/bitbucket-cloud/docs/set-up-an-ssh-key/)

# Install From a Git Repository
## 將共用函式庫獨立放置在一個新專案中
### 將專案進行拆分
- 前台 api 專案：WebSignin_Fastify_API
- 共用函式庫專案：WebSignin_Fastify_Core_API
### 評估需要拆分到共用函式庫專案的部分如下：
- BaseController：基礎 controller 模組
- SuccessResponse：成功的 res 數據模型
- ErrorResponse：失敗的 res 數據模型
- SuccessResult：成功的 result 數據模型
- ErrorResult：失敗的 result 數據模型
- DatetimeService：從資料庫取得台灣日期時間 Service

### 共用函式庫專案：WebSignin_Fastify_Core_API
- 在 app.js 中 exports 各共用函式庫模組
```javascript
// app.js
module.exports.BaseController = require('./src/base/base.controller');
module.exports.SuccessResponse = require('./src/base/base.response');
module.exports.ErrorResponse = require('./src/base/base.response');
module.exports.SuccessResult = require('./src/base/base.result');
module.exports.ErrorResult = require('./src/base/base.result');
module.exports.DatetimeService = require('./src/service/datetimeService');
```
### 前台 api 專案：WebSignin_Fastify_API
- 從 共用函式庫專案 import 共用函式庫
```javascript
// 引用共用函式庫
const websignin_fastify_core_api = require('websignin_fastify_core_api');
const BaseController = websignin_fastify_core_api.BaseController;
const SuccessResponse = websignin_fastify_core_api.SuccessResponse.SuccessResponse;
const ErrorResponse = websignin_fastify_core_api.ErrorResponse.ErrorResponse;
const SuccessResult = websignin_fastify_core_api.SuccessResult.SuccessResult;
const ErrorResult = websignin_fastify_core_api.ErrorResult.ErrorResult;
const DatetimeService = websignin_fastify_core_api.DatetimeService;
```
## 使用 git+http 方式
### 在 WebSignin_Fastify_Core_API 中的 package.json 設定 repository
- 使用 git+http 的 url 需另外加上 **使用者帳號:密碼@** 才能有權限取得資源
```
// 原本的 url
http://192.168.25.60:9001/11542/WebSignin_Fastify_Core_API.git
```
```
// 加上 使用者帳號:密碼@ 的 url
http://11542:abc123@192.168.25.60:9001/11542/WebSignin_Fastify_Core_API.git
```
- 在 package.json 中新增 repository 設定
```json
// package.json
  "repository": {
    "type": "git",
    "url": "git+http://11542:abc123@192.168.25.60:9001/11542/WebSignin_Fastify_Core_API.git"
  },
```
### 在 WebSignin_Fastify_API 中進行 install WebSignin_Fastify_Core_API
```shell
npm install git+http://11542:abc123@192.168.25.60:9001/11542/WebSignin_Fastify_Core_API.git
```
- install 成功可以在 package.json 中的 dependencies 看到 WebSignin_Fastify_Core_API
```json
// package.json
  "dependencies": {
    "websignin_fastify_core_api":"git+ssh://git@192.168.25.60:9000/11542/WebSignin_Fastify_Core_API.git"
  },
```
![fastify separate project by npm install with git http.png](http://192.168.25.60:8000/files/file_storage/bb7b844f.png)

### Docker 部屬
- 共用函式庫專案：不須部屬
- 部屬 前台 api 專案：WebSignin_Fastify_API
```shell
docker build -t websignin_fastify_api --no-cache .
docker run -p 10001:10001 websignin_fastify_api
```
- 當 docker 空間不足時可清空 docker
```shell
docker system prune --all --force --volumes
```

## 使用 git+ssh 方式 (從本機端產製)
### 產生 SSH key：使用 ssh-keygen 產生公鑰/私鑰
- 以系統管理員身分使用 git bash 製作，路徑預定在 C 槽
- 確認電腦是否已有 SSH 密鑰存在
```shell
ls -al ~/.ssh
```
- 使用 ssh-keygen 產生公鑰/私鑰
```shell
ssh-keygen -t rsa -b 4096 -C "mandy.chen@hokia.com.tw"
```
- 產製出的公鑰 id_rsa 會是以下形式
```
-----BEGIN OPENSSH PRIVATE KEY-----
...
-----END OPENSSH PRIVATE KEY-----
```
> 此方式產製的 公鑰/私鑰 部屬到 docker 會有 key invalid format 的問題
> 需改成 ```ssh-keygen -m PEM -t rsa -b 4096 -C "mandy.chen@hokia.com.tw"```
{.is-danger}

- 在 ssh-keygen 中常用參數如下：
`-t`：指定金鑰的加密演算法，預設使用 SSH2d 的 rsa。
`-f`：指定金鑰的檔名，預設檔名會隨演算法而變動，例如使用 rsa 加密時，其檔名預設為 id_rsa（私鑰 id_rsa ，公鑰 id_rsa.pub ）。這階段沒改沒關係，等等還會在詢問。
`-P`：提供舊密碼，空表示不需要密碼（-P ' '）
`-N`：提供新密碼，空表示不需要密碼 ( -N ' ' )
`-b`：指定金鑰長度（bits）。
`-C`：提供一個新標籤。

![ssh-keygen generate public and private rsa key.png](http://192.168.25.60:8000/files/file_storage/61976735.png)

```shell
Generating public/private rsa key pair.
Enter file in which to save the key (/c/Users/11542/.ssh/id_rsa): # 儲存金鑰路徑和檔名，使用預設直接 Enter 
/c/Users/11542/.ssh/id_rsa already exists.
Overwrite (y/n)? y                                                # id_rsa 已經存在，是否覆寫，填入 y
Enter passphrase (empty for no passphrase):                       # 不建立密碼，直接 Enter
Enter same passphrase again:                                      # 密碼確認，直接 Enter
Your identification has been saved in /c/Users/11542/.ssh/id_rsa  # Private Key 存放路徑
Your public key has been saved in /c/Users/11542/.ssh/id_rsa.pub  # Public Key 存放路徑
The key fingerprint is:
SHA256:lSGt2sQH914RzMa2+a+kNJhbK3YDcFqb+vUNWoPJiCI mandy.chen@hokia.com.tw
The key's randomart image is:
+---[RSA 4096]----+
|        ...  +.. |
|        ..oo  B  |
|       . +o. o + |
|        =.+ . +  |
|       +S* + . . |
|      . o.+=.o  .|
|    E . ..+.O = .|
|     . .. o=oO +.|
|         ooo+.o..|
+----[SHA256]-----+

```
- 預設檔案會放置在 C 槽的資料夾 .ssh 中 
```
C:\Users\11542\.ssh
```

![ssh-keygen generate public and private rsa key in C.png](http://192.168.25.60:8000/files/file_storage/383f1979.png)

- 私鑰 ( Private Key )：**id_rsa** ➔ 這把則是放在自己電腦的金鑰，它等同於你的密碼，不可外洩。
- 公鑰 ( Public Key )：**id_rsa.pub** ➔ 這把是對外公開的金鑰，也就是我們要上傳到 git 上面的金鑰。

### 將 SSH key 加入代理
- 建立 SSH key 後要將其加入代理，好讓之後可以使用
- 確認 **ssh-agent** 是否正常運行
```shell
eval $(ssh-agent -s)
```
- 若有回復以下內容則為正常運行
```shell
Agent pid 4097
```
- 將密鑰加入 ssh-agent 內
```shell
ssh-add ~/.ssh/id_rsa
```
- 若有回復以下內容則為正常運行
```shell
Identity added: /c/Users/11542/.ssh/id_rsa (mandy.chen@hokia.com.tw)
```

![ssh-agent and ssh-add.png](http://192.168.25.60:8000/files/file_storage/7e840aff.png)

### 設定 Git 的 SSH key
- 到 Git 右上角個人資料 ➔ 設定 ➔ SSH/GPG 金鑰
- 新增金鑰：將公鑰 id_rsa.pub 複製到 Git 上

![git 新增 SSH 金鑰.png](http://192.168.25.60:8000/files/file_storage/aaa4964b.png)

### 測試是否有權限取得資源
- 到專案中進行以下測試
- 測試成功會建立 known_hosts
```shell
ssh -Tv -i ~/.ssh/id_rsa -p 9000 git@192.168.25.60
```

![ssh -Tv.png](http://192.168.25.60:8000/files/file_storage/cb08b109.png)

### 在 WebSignin_Fastify_Core_API 中的 package.json 設定 repository
- 使用 git+ssh 的 url 需修正 port 才能有權限取得資源
```
// 原本的 url
git@192.168.25.60:11542/WebSignin_Fastify_Core_API.git
```
- 在此參考 Docker主機相關資訊設定 中 Gitea 的 port 為 9000 進行調整
> [Docker主機相關資訊設定](/軟體開發/系統設定/Docker主機相關資訊設定)
{.is-info}
```
// 調整為 port/使用者帳號 的 url
git@192.168.25.60:9000/11542/WebSignin_Fastify_Core_API.git
```
- 
```json
// package.json
  "repository": {
    "type": "git",
    "url": "git+ssh://git@192.168.25.60:9000/11542/WebSignin_Fastify_Core_API.git"
  },
```
### 在 WebSignin_Fastify_API 中進行 install WebSignin_Fastify_Core_API
```shell
npm install git+ssh://git@192.168.25.60:9000/11542/WebSignin_Fastify_Core_API.git
```
- install 成功可以在 package.json 中的 dependencies 看到 WebSignin_Fastify_Core_API
```json
// package.json
  "dependencies": {
    "websignin_fastify_core_api":"git+ssh://git@192.168.25.60:9000/11542/WebSignin_Fastify_Core_API.git"
  },
```
![fastify separate project by npm install with git ssh.png](http://192.168.25.60:8000/files/file_storage/4799192a.png)

## 使用 git+ssh 方式 (從 docker container 產製)
### 從 Docker Hub 上 pull image
- 在此使用 node 14 
- 指定版本
```shell
docker pull node:14
```
- 不指定抓取最新版
```shell
docker pull node
```

### 查看 images
```shell
docker images
docker image ls
```

### image 跑起來變成 container
- -it 是為了讓我們進到這個 container 的 shell 下指令
- 會進到 container 裡面，預設是 root
```shell
docker run -it node:14 bash
```

![docker run -it image bash.png](http://192.168.25.60:8000/files/file_storage/a60f5373.png)

### 更新套件
```shell
apt-get update
```

### 離開 container
```shell
exit
```

### 進入到 container
```shell
docker exec -it <container-name> /bin/bash
```

### 產生 SSH key：使用 ssh-keygen 產生公鑰/私鑰
- 使用 -m PEM 產製 RSA PRIVATE KEY
- 預設會放在 container 的 /root/.ssh/id_rsa 底下
```shell
ssh-keygen -m PEM -t rsa -b 4096 -C "mandy.chen@hokia.com.tw"
```
> [ssh-keygen does not create RSA private key](https://serverfault.com/questions/939909/ssh-keygen-does-not-create-rsa-private-key)
{.is-info}
- 產製出的公鑰 id_rsa 會是以下形式
```
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

- 到放置 key 的資料夾
```shell
cd ~/.ssh
```
- 查看私鑰並**複製一份到專案當中**
```shell
cat id_rsa
```
- 查看公鑰並**複製一份到專案當中**
```shell
cat id_rsa.pub
```

### 將 SSH key 加入代理
- 確認 ssh-agent 是否正常運行
```shell
eval $(ssh-agent -s)
```
- 將私鑰加入 ssh-agent 內
```shell
ssh-add /root/.ssh/id_rsa
```

### 設置 id_rsa 權限僅擁有者可讀寫，其他人不可讀寫執行
```shell
chmod 600 ~/.ssh/id_rsa
```
> 未設定權限會有以下錯誤
> npm ERR! Permissions 0664 for '/root/.ssh/id_rsa' are too open.
> npm ERR! It is required that your private key files are NOT accessible by others.
{.is-danger}

### 設定 Git 的 SSH key
- 到 Git 右上角個人資料 ➔ 設定 ➔ SSH/GPG 金鑰
- 新增金鑰：將公鑰 id_rsa.pub 複製到 Git 上

### 測試是否有權限取得資源
- 測試成功會建立 known_hosts，**將 known_hosts 複製一份到專案當中**
```shell
ssh -Tv -i ~/.ssh/id_rsa -p 9000 git@192.168.25.60
```
```shell
cat known_hosts
```

### 確認 id_rsa、id_rsa.pub、known_hosts 有放到專案裡面
- 須將 id_rsa、id_rsa.pub、known_hosts 放到 .gitignore 中
```
# id_rsa
id_rsa

# id_rsa.pub
id_rsa.pub

# known_hosts
known_hosts
```

### 將 id_rsa、id_rsa.pub、known_hosts 複製到本機
- 本機 ssh key 放置資料夾
```
C:\Users\11542\.ssh
```
### 測試是否有權限取得資源
- 到專案中進行以下測試
```shell
ssh -Tv -i ~/.ssh/id_rsa -p 9000 git@192.168.25.60
```
### 在 WebSignin_Fastify_API 中進行 install WebSignin_Fastify_Core_API
```shell
npm install git+ssh://git@192.168.25.60:9000/11542/WebSignin_Fastify_Core_API.git
```
- install 成功可以在 package.json 中的 dependencies 看到 WebSignin_Fastify_Core_API
```json
// package.json
  "dependencies": {
    "websignin_fastify_core_api":"git+ssh://git@192.168.25.60:9000/11542/WebSignin_Fastify_Core_API.git"
  },
```

### 在 dockerfile 新增指令
- 在 npm install 之前新增指令如下
```dockerfile
# 將 SSH 公私鑰及 known_hosts 複製到 docker
COPY ./id_rsa /root/.ssh/id_rsa
COPY ./id_rsa.pub /root/.ssh/id_rsa.pub
COPY ./known_hosts /root/.ssh/known_hosts
# 確認 ssh-agent 是否正常運行
RUN eval $(ssh-agent -s)
# 設置 id_rsa 權限僅擁有者可讀寫，其他人不可讀寫執行
RUN chmod 600 ~/.ssh/id_rsa
```
- 完整版 dockerfile
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
# 將 SSH 公私鑰及 known_hosts 複製到 docker
COPY ./id_rsa /root/.ssh/id_rsa
COPY ./id_rsa.pub /root/.ssh/id_rsa.pub
COPY ./known_hosts /root/.ssh/known_hosts
# 確認 ssh-agent 是否正常運行
RUN eval $(ssh-agent -s)
# 設置 id_rsa 權限僅擁有者可讀寫，其他人不可讀寫執行
RUN chmod 600 ~/.ssh/id_rsa
# 安裝 Dependencies
RUN npm install && npm cache clean --force
# 生成 Prisma Client
RUN npx prisma generate --schema=./src/prisma/schema.prisma
# 建立 log 資料夾
RUN mkdir -p /src/logs/error && mkdir -p /src/logs/info

# Stage 5: Set container listen Port
# 設定 Docker 要開放的 Port
EXPOSE 10001

# Stage 6: Run app
# 啟動 Container 時要執行的指令
CMD [ "node", "app" ]
```

### Docker 部屬
- 部屬 前台 api 專案：WebSignin_Fastify_API
```shell
docker build -t websignin_fastify_api --no-cache .
docker run -p 10001:10001 -d --name websignin_fastify_api --restart=always
```

# API method
## 將共用函式庫獨立放置在一個新專案中
### 將專案進行拆分
- 前台 api 專案：WebSignin_Fastify_API
- 共用函式庫專案：WebSignin_Fastify_Core_API
### 評估需要拆分到共用函式庫專案的部分如下：
- DatetimeService：從資料庫取得台灣日期時間 Service

### 共用函式庫專案：WebSignin_Fastify_Core_API
- 新增 取得 datetime 的 api：test、router、controller、service、logger、dockerfile、.dockerignore

### 前台 api 專案：WebSignin_Fastify_API
- 使用 axios 呼叫 WebSignin_Fastify_Core_API 中的 datetime api
- 新增一個資料夾 api 放置 datetimeApi 檔案
```javascript
// src/api/datetimeApi.js
const axios = require('axios');
const request = axios.create({
  // baseURL: 'http://127.0.0.1:10003', // 本機 url
  baseURL: 'http://192.168.25.180:10003', // 部屬到 docker 的 url
});

const datetimeApi = async () => {
  return request.get('/api/public/datetime');
};
module.exports = datetimeApi;
```
- 呼叫 api 使用方法
```javascript
const datetimeApi = require('../../api/datetimeApi');
const twDateTimeData = (await dateTimeApi()).data;
const twDate = twDateTimeData.tw_date;
const twTime = twDateTimeData.tw_time;
const twDay = twDateTimeData.tw_day;
```

## Docker 部屬
- 部屬 共用函式庫專案：WebSignin_Fastify_Core_API
```shell
docker build -t websignin_fastify_core_api --no-cache .
docker run -p 10003:10003 -d --name websignin_fastify_core_api --restart=always
```

- 部屬 前台 api 專案：WebSignin_Fastify_API
```shell
docker build -t websignin_fastify_api --no-cache .
docker run -p 10001:10001 -d --name websignin_fastify_api --restart=always
```