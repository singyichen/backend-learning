---
title: MongoDB Install
description: 
published: true
date: 2023-07-12T03:54:06.635Z
tags: mongodb
editor: markdown
dateCreated: 2023-07-12T03:54:06.635Z
---

# MongoDB Install

# 安裝
## 初始化專案
- `.gitignore`：參閱 [toptal](https://www.toptal.com/developers/gitignore/api/python)，並新增 `logs`、`*.log`
- `.env`：
	- `MONGO_INITDB_ROOT_USERNAME=root`
	- `MONGO_INITDB_ROOT_PASSWORD=root`
	- `ME_CONFIG_MONGODB_ADMINUSERNAME=root`
	- `ME_CONFIG_MONGODB_ADMINPASSWORD=root`

## 新增 `docker-compose.yaml`
- ME_CONFIG_MONGODB_URL：`mongodb://<USERNAME>:<PASSWORD>@<MONGO-SERVICE-NAME>:<PORT>`
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


