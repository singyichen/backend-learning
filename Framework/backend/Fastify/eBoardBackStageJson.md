---
title: 資訊看板後台 api
description: 
published: true
date: 2023-03-10T07:24:36.343Z
tags: 
editor: markdown
dateCreated: 2023-01-03T01:01:28.867Z
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
    "dateTime": {
        "tw_date": "2023/01/03",
        "tw_time": "10:08:43",
        "tw_day": "(二)"
    },
    "data": {
        "user_id": "11542",
        "user_name": "陳欣怡",
        "token_id": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiMTE1NDIiLCJpYXQiOjE2NzI3MTE3MjQsImV4cCI6MTY3Mjc5ODEyNH0.DAm9CtLus2HhDdNkLLwa3wOJeBavfX6NcWDY219b_14",
        "login_date": "2023/01/03",
        "login_day": "(二)",
        "login_time": "10:08:43"
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
## 驗證 JWT Token POST
### input
```json
{
    "token_id": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiMTE1NDIiLCJpYXQiOjE2NzI5OTcwNzgsImV4cCI6MTY3MzA4MzQ3OH0.1XSLZMtv5gWZDXXHQyxbcgtV3AWb1Kcv9Px42wx9xrE"
}
```
### output
- 成功
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/06",
        "tw_time": "17:24:44",
        "tw_day": "(五)"
    },
    "data": {
        "token_id": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VyX2lkIjoiMTE1NDIiLCJpYXQiOjE2NzI5OTcwNzgsImV4cCI6MTY3MzA4MzQ3OH0.1XSLZMtv5gWZDXXHQyxbcgtV3AWb1Kcv9Px42wx9xrE"
    }
}
```
- 失敗
```json
{
    "code": 40105,
    "dateTime": {
        "tw_date": "2023/01/06",
        "tw_time": "17:03:53",
        "tw_day": "(五)"
    },
    "statusCode": 401,
    "error": "Unauthorized",
    "message": "連線逾時，Token 簽章不合法"
}
```
# 人員 ( user )
## 新增單筆人員 POST
### input
```json
{
    "user_id":"11542",
    "user_name":"陳欣怡"
}
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/03",
        "tw_time": "08:57:45",
        "tw_day": "(二)"
    },
    "data": {
        "user_id": "11551",
        "user_name": "11551",
        "token_id": ""
    }
}
```

## 取得多筆人員 GET
### input
```
none
```
### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/03",
        "tw_time": "08:57:59",
        "tw_day": "(二)"
    },
    "data": [
        {
            "user_id": "11542",
            "user_name": "陳欣怡"
        },
        {
            "user_id": "11545",
            "user_name": "龔玉惠"
        },
        {
            "user_id": "11202",
            "user_name": "孟國揚"
        },
        {
            "user_id": "11548",
            "user_name": "李文瀚"
        },
        {
            "user_id": "11551",
            "user_name": "11551"
        }
    ]
}
```
## 刪除單筆人員 POST
### input
```json
{
    "user_id":"11551"
}
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/03",
        "tw_time": "08:58:34",
        "tw_day": "(二)"
    },
    "data": {
        "user_id": "11551",
        "user_name": "11551",
        "token_id": ""
    }
}
```

## 驗證刪除單筆人員 POST
### input
```json
{
    "user_id":"11542"
}
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/03",
        "tw_time": "08:58:34",
        "tw_day": "(二)"
    },
    "data": {}
}
```
# 休假 ( holidayAgent )
## 新增單筆休假 POST
### input
```json
{
    "user_id":"11545",
    "agent_id":"11542",
    "holiday":"2023-01-04",
    "part_type":"一日"
}
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/03",
        "tw_time": "09:00:15",
        "tw_day": "(二)"
    },
    "data": {
        "user_id": "11545",
      	"user_name": "龔玉惠",
        "agent_id": "11542",
        "agent_name": "陳欣怡",
        "holiday": "2023-01-04",
        "part_type": "一日"
    }
}
```

## 取得多筆休假 GET
### input
```
none
```
### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/03",
        "tw_time": "09:00:18",
        "tw_day": "(二)"
    },
    "data": [
        {
            "user_id": "11545",
            "agent_id": "11542",
            "holiday": "2023/01/03(二)",
            "part_type": "一日",
            "user_name": "龔玉惠",
            "agent_name": "陳欣怡"
        },
        {
            "user_id": "11545",
            "agent_id": "11542",
            "holiday": "2023/01/04(三)",
            "part_type": "一日",
            "user_name": "龔玉惠",
            "agent_name": "陳欣怡"
        }
    ]
}
```
## 刪除單筆休假 POST
### input
```json
{
    "user_id":"11542",
    "holiday":"2023-01-04",
    "agent_id":"11543"
}
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/03",
        "tw_time": "09:01:13",
        "tw_day": "(二)"
    },
    "data": {
        "count": 0
    }
}
```
## 取得多筆代理人員 GET
### input
- 要排除的人員 ID
```
"user_id":"11542"
```

```
none
```
### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/02/15",
        "tw_time": "15:05:16",
        "tw_day": "(三)"
    },
    "data": [
        {
            "user_id": "11545",
            "user_name": "龔玉惠"
        },
        {
            "user_id": "11548",
            "user_name": "李文瀚"
        },
        {
            "user_id": "11238",
            "user_name": "歐陽美君"
        },
        {
            "user_id": "11399",
            "user_name": "蔡銘麒"
        },
        {
            "user_id": "11165",
            "user_name": "杜華青"
        },
    ]
}
```
# 飲水輪值 ( waterShift )
## 新增單筆飲水輪值人員 POST
### input
```json
{
    "user_id":"11548"
}
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "15:10:43",
        "tw_day": "(三)"
    },
    "data": {
        "user_id": "11548",
        "queue_number": 3,
        "active": 0
    }
}
```

## 取得多筆飲水輪值人員 GET
### input
```
none
```
### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/05",
        "tw_time": "13:32:44",
        "tw_day": "(四)"
    },
    "data": [
        {
            "user_id": "11542",
            "queue_number": 1,
            "active": 1,
            "next_id": "11548",
            "user_name": "陳欣怡"
        },
        {
            "user_id": "11548",
            "queue_number": 2,
            "active": 0,
            "next_id": "11542",
            "user_name": "李文瀚"
        }
    ]
}
```
## 刪除單筆飲水輪值人員 POST
### input
```json
{
    "user_id":"10000",
    "next_id":"11545",
    "active":0
}
```

```json
{
    "user_id":"10000"
}
```
### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "11:56:27",
        "tw_day": "(三)"
    },
    "data": {
        "user_id": "10000",
        "queue_number": 4,
        "active": 0
    }
}
```
## 編輯單筆飲水輪值人員 PATCH
### input
```json
{
    "user_id":"11542",
    "queue_number":3
}
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "13:53:10",
        "tw_day": "(三)"
    },
    "data": {
        "user_id": "11542",
        "queue_number": 3,
        "active": 1
    }
}
```
## 取得單筆飲水輪值當班人員 GET
### input
```
none
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "11:14:13",
        "tw_day": "(三)"
    },
    "data": [
        {
            "user_id": "11542",
            "queue_number": 1,
            "active": 1,
            "user_name": "陳欣怡"
        },
        {
            "user_id": "11545",
            "queue_number": 2,
            "active": 0,
            "user_name": "龔玉惠"
        }
    ]
}
```
## 編輯單筆飲水輪值當班人員 PATCH
### input
```json
{
    "user_id":"11548",
  	"user_name":"李文瀚",
    "queue_number": 2,
    "plus_or_minus":1
}
```
```json
{
    "user_id":"11548",
    "user_name":"李文瀚",
    "queue_number": 2,
    "plus_or_minus":-1
}
```
### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "14:17:13",
        "tw_day": "(三)"
    },
    "data": {
        "user_id": "11548",
        "queue_number": 1,
        "active": 1
    }
}
```
## 取得多筆飲水紀錄 GET
### input
```
none
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "09:02:37",
        "tw_day": "(三)"
    },
    "data": [
        {
            "user_id": "11545",
            "call_date": "2023/01/03",
            "quantity": 4,
            "user_name": "龔玉惠"
        },
        {
            "user_id": "11548",
            "call_date": "2023/01/04",
            "quantity": 4,
            "user_name": "李文瀚"
        },
        {
            "user_id": "11202",
            "call_date": "2023/01/04",
            "quantity": 4,
            "user_name": "孟國揚"
        },
        {
            "user_id": "11542",
            "call_date": "2023/01/04",
            "quantity": 4,
            "user_name": "陳欣怡"
        }
    ]
}
```
# 機房輪值 ( engineShift )
## 新增單筆機房輪值人員 POST
### input
```json
{
    "user_id":"11548"
}
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "15:10:43",
        "tw_day": "(三)"
    },
    "data": {
        "user_id": "11548",
        "queue_number": 3,
        "active": 0
    }
}
```

## 取得多筆機房輪值人員 GET
### input
```
none
```
### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "17:08:30",
        "tw_day": "(三)"
    },
    "data": [
        {
            "user_id": "11542",
            "queue_number": 1,
            "active": 1,
            "next_id": "11545",
            "user_name": "陳欣怡"
        },
        {
            "user_id": "11545",
            "queue_number": 2,
            "active": 0,
            "next_id": "11548",
            "user_name": "龔玉惠"
        },
        {
            "user_id": "11548",
            "queue_number": 3,
            "active": 0,
            "next_id": "11542",
            "user_name": "李文瀚"
        }
    ]
}
```
## 刪除單筆機房輪值人員 POST
### input
```json
{
    "user_id":"10000",
    "next_id":"11545",
    "active":0
}
```

```json
{
    "user_id":"10000"
}
```
### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "11:56:27",
        "tw_day": "(三)"
    },
    "data": {
        "user_id": "10000",
        "queue_number": 4,
        "active": 0
    }
}
```
## 編輯單筆機房輪值人員 PATCH
### input
```json
{
    "user_id":"11542",
    "queue_number":3
}
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "13:53:10",
        "tw_day": "(三)"
    },
    "data": {
        "user_id": "11542",
        "queue_number": 3,
        "active": 1
    }
}
```
## 取得單筆機房輪值當班人員 GET
### input
```
none
```

### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "11:14:13",
        "tw_day": "(三)"
    },
    "data": [
        {
            "user_id": "11542",
            "queue_number": 1,
            "active": 1,
            "user_name": "陳欣怡"
        },
        {
            "user_id": "11545",
            "queue_number": 2,
            "active": 0,
            "user_name": "龔玉惠"
        }
    ]
}
```
## 編輯單筆機房輪值當班人員 PATCH
### input
```json
{
    "user_id":"10001",
    "queue_number": 2,
    "plus_or_minus":1
}
```
```json
{
    "user_id":"10001",
    "queue_number": 2,
    "plus_or_minus":-1
}
```
### output
```json
{
    "code": "00",
    "dateTime": {
        "tw_date": "2023/01/04",
        "tw_time": "14:17:13",
        "tw_day": "(三)"
    },
    "data": {
        "user_id": "10000",
        "queue_number": 1,
        "active": 1
    }
}
```
