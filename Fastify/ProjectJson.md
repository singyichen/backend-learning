---
title: 線上打卡系統前台 api
description: 
published: true
date: 2022-12-08T05:41:54.118Z
tags: 
editor: markdown
dateCreated: 2022-12-05T05:56:55.241Z
---

# 基礎 ( basic )
## 使用者登入 POST
### input
```json
{
    "user_id": "11542",
    "password": "qaz123"
}
```
### output
- 成功
```json
{
    "code": "00",
    "datetime": {
        "tw_date": "2022/12/05",
        "tw_time": "14:07:05",
        "tw_day": "(一)"
    },
    "data": {
      	"user_id": "11542",
        "user_name": "陳欣怡",
        "group_id": "11542",
        "login_day": "2022/12/05",
        "login_time": "14:07:05",
        "login_day": "(三)",
        "token_key": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJlbXBsb3llZV9pZCI6IjExNTQyIiwiaWF0IjoxNjcwMjIwNDI1LCJleHAiOjE2NzAyMjEwMjV9.uBSygdnfQDE1H-4INNeO11njPzs6XVecA1ayOa-_GtE"
    }
}
```
- 失敗
```json
{
    "statusCode": 401,
    "code": 40101,
    "datetime": {
        "tw_date": "2022/12/05",
        "tw_time": "14:19:54",
        "tw_day": "(一)"
    },
    "error": "Unauthorized",
    "message": "使用者不存在，未經核可，無法使用打卡系統"
}
```
```json
{
    "statusCode": 401,
    "code": 40103,
    "datetime": {
        "tw_date": "2022/12/05",
        "tw_time": "14:17:52",
        "tw_day": "(一)"
    },
    "error": "Unauthorized",
    "message": "登入失敗; AD帳號或密碼輸入錯誤"
}
```
## 使用者登出 POST
### input
```json
{
    "user_id": "11542"
}
```

### output
```json
{
    "code": "00",
    "datetime": {
        "tw_date": "2022/12/05",
        "tw_time": "14:11:34",
        "tw_day": "(一)"
    },
}
```
# 打卡 ( signin )
## 新增單筆打卡 POST
### input
```json
{
    "user_id": "11542",
    "group_id": "11542",
    "sign_date": "2022-10-19 10:23:54+08",
    "status": "上班"
}
```

### output
```json
{
    "code": "00",
    "datetime": {
        "tw_date": "2022/12/05",
        "tw_time": "13:33:18",
        "tw_day": "(一)"
    },
    "data": {
        "status": "上班",
        "first_time": "08:00:00",
        "last_time": "08:05:00",
    }
}
```

## 取得單筆打卡 GET
### input
```
user_id
```
### output
```json
{
    "code": "00",
    "datetime": {
        "tw_date": "2022/12/05",
        "tw_time": "13:33:24",
        "tw_day": "(一)"
    },
    "data": [
        {
            "status": "上班",
            "first_time": "08:00:00",
            "last_time": "08:05:00",
        },
        {
            "status": "下班",
            "first_time": "17:00:00",
            "last_time": "17:05:00",
        },
        {
            "status": "外出",
            "first_time": "08:30:00"
        },
        {
            "status": "外歸",
            "first_time": "08:00:00"
        }
    ]
}
```
# 打卡紀錄 ( signinDetail )
## 取得多筆打卡 GET
### input
```
user_id
started_date
ended_date
```
### output