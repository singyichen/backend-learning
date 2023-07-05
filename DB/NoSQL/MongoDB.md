---
title: MongoDB
description: 
published: true
date: 2023-05-29T00:35:11.567Z
tags: nosql, mongodb
editor: markdown
dateCreated: 2023-05-18T06:05:19.280Z
---

# MongoDB
- [ ] [MongoDB](https://www.mongodb.com/)
- [ ] [從入門到精通 MongoDB](https://ithelp.ithome.com.tw/users/20130448/ironman/3618)
- [ ] [MongoDB是什麼?MongoDB介紹及應用、架構、優點](https://www.webcomm.com.tw/blog/mongodb/)
- [ ] [MongoDB – 最受歡迎的 NOSQL 資料庫](https://www.omniwaresoft.com.tw/mongodb/)
- [ ] [使用Docker建立Mongodb加上Mongo Express](https://104.es/2022/07/05/docker-compose-mongodb-mongo-express/)



# 安裝
## 初始化專案
- `.gitignore`：參閱 [toptal](https://www.toptal.com/developers/gitignore/api/python)，並新增 `logs`、`*.log`
- `.env`：
	- `MONGO_INITDB_ROOT_USERNAME=root`
	- `MONGO_INITDB_ROOT_PASSWORD=root`
	- `ME_CONFIG_MONGODB_ADMINUSERNAME=root`
	- `ME_CONFIG_MONGODB_ADMINPASSWORD=root`

## 新增 `docker-compose.yaml`
```yaml
version: "3.9"
services:
  mongo:
    image: mongo
    restart: always
    env_file: ./.env
    environment:
      MONGO_INITDB_ROOT_USERNAME: $MONGO_INITDB_ROOT_USERNAME
      MONGO_INITDB_ROOT_PASSWORD: $MONGO_INITDB_ROOT_PASSWORD
    ports:
      - 27017:27017
    volumes:
      - mongodb:/data/db

  mongo-express:
    image: mongo-express
    restart: always
    env_file: ./.env
    ports:
      - 8081:8081
    environment:
      ME_CONFIG_MONGODB_ADMINUSERNAME: $ME_CONFIG_MONGODB_ADMINUSERNAME
      ME_CONFIG_MONGODB_ADMINPASSWORD: $ME_CONFIG_MONGODB_ADMINPASSWORD
      ME_CONFIG_MONGODB_URL: mongodb://$ME_CONFIG_MONGODB_ADMINUSERNAME:$ME_CONFIG_MONGODB_ADMINPASSWORD@mongo:27017/
volumes:
  mongodb:
```

## 啟動 MongoDB 服務
```bash
docker compose up -d
```

## 使用 MongoDB
- http://127.0.0.1:8081


