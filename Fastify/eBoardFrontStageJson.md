---
title: 資訊看板前台 api
description: 
published: true
date: 2023-01-18T07:33:16.422Z
tags: 
editor: markdown
dateCreated: 2023-01-05T05:37:20.478Z
---

# 日期時間 ( dateTime )
## 取得台灣日期時間(NTP) GET
### input
```
none
```
### output
```json
{
    "tw_num": 1673485523000
}
```
# 天氣 ( weather )
## 取得單筆天氣 GET
### input
```
none
```
### output
> cwb 會固定更新資料，故呼叫 api 時都撈取 array 第一筆資料，表示最接近當時撈取時間之資料

- Wx：天氣現象
- Wx_value：天氣現象圖片對應 value
- AT：體感溫度
- T：溫度
- PoP6h：6小時降雨機率
```json
{
    "Wx": "晴",
  	"Wx_value": "01",
    "AT": "21",
    "T": "19",
    "PoP6h": "10"
}
```

```json
{
    "Wx": {
        "elementName": "Wx",
        "description": "天氣現象",
        "time": [
            {
                "startTime": "2023-01-05 12:00:00",
                "endTime": "2023-01-05 15:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-05 15:00:00",
                "endTime": "2023-01-05 18:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-05 18:00:00",
                "endTime": "2023-01-05 21:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-05 21:00:00",
                "endTime": "2023-01-06 00:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 00:00:00",
                "endTime": "2023-01-06 03:00:00",
                "elementValue": [
                    {
                        "value": "陰",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "07",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 03:00:00",
                "endTime": "2023-01-06 06:00:00",
                "elementValue": [
                    {
                        "value": "陰",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "07",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 06:00:00",
                "endTime": "2023-01-06 09:00:00",
                "elementValue": [
                    {
                        "value": "多雲",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "04",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 09:00:00",
                "endTime": "2023-01-06 12:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 12:00:00",
                "endTime": "2023-01-06 15:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 15:00:00",
                "endTime": "2023-01-06 18:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 18:00:00",
                "endTime": "2023-01-06 21:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 21:00:00",
                "endTime": "2023-01-07 00:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 00:00:00",
                "endTime": "2023-01-07 03:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 03:00:00",
                "endTime": "2023-01-07 06:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 06:00:00",
                "endTime": "2023-01-07 09:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 09:00:00",
                "endTime": "2023-01-07 12:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 12:00:00",
                "endTime": "2023-01-07 15:00:00",
                "elementValue": [
                    {
                        "value": "多雲",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "04",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 15:00:00",
                "endTime": "2023-01-07 18:00:00",
                "elementValue": [
                    {
                        "value": "多雲",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "04",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 18:00:00",
                "endTime": "2023-01-07 21:00:00",
                "elementValue": [
                    {
                        "value": "多雲",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "04",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 21:00:00",
                "endTime": "2023-01-08 00:00:00",
                "elementValue": [
                    {
                        "value": "多雲",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "04",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-08 00:00:00",
                "endTime": "2023-01-08 03:00:00",
                "elementValue": [
                    {
                        "value": "多雲",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "04",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-08 03:00:00",
                "endTime": "2023-01-08 06:00:00",
                "elementValue": [
                    {
                        "value": "多雲",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "04",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-08 06:00:00",
                "endTime": "2023-01-08 09:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            },
            {
                "startTime": "2023-01-08 09:00:00",
                "endTime": "2023-01-08 12:00:00",
                "elementValue": [
                    {
                        "value": "晴",
                        "measures": "自定義 Wx 文字"
                    },
                    {
                        "value": "01",
                        "measures": "自定義 Wx 單位"
                    }
                ]
            }
        ]
    },
    "AT": {
        "elementName": "AT",
        "description": "體感溫度",
        "time": [
            {
                "dataTime": "2023-01-05 12:00:00",
                "elementValue": [
                    {
                        "value": "24",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-05 15:00:00",
                "elementValue": [
                    {
                        "value": "25",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-05 18:00:00",
                "elementValue": [
                    {
                        "value": "23",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-05 21:00:00",
                "elementValue": [
                    {
                        "value": "22",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 00:00:00",
                "elementValue": [
                    {
                        "value": "21",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 03:00:00",
                "elementValue": [
                    {
                        "value": "20",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 06:00:00",
                "elementValue": [
                    {
                        "value": "19",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 09:00:00",
                "elementValue": [
                    {
                        "value": "19",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 12:00:00",
                "elementValue": [
                    {
                        "value": "23",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 15:00:00",
                "elementValue": [
                    {
                        "value": "25",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 18:00:00",
                "elementValue": [
                    {
                        "value": "22",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 21:00:00",
                "elementValue": [
                    {
                        "value": "21",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 00:00:00",
                "elementValue": [
                    {
                        "value": "20",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 03:00:00",
                "elementValue": [
                    {
                        "value": "19",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 06:00:00",
                "elementValue": [
                    {
                        "value": "19",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 09:00:00",
                "elementValue": [
                    {
                        "value": "20",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 12:00:00",
                "elementValue": [
                    {
                        "value": "24",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 15:00:00",
                "elementValue": [
                    {
                        "value": "25",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 18:00:00",
                "elementValue": [
                    {
                        "value": "24",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 21:00:00",
                "elementValue": [
                    {
                        "value": "22",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-08 00:00:00",
                "elementValue": [
                    {
                        "value": "21",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-08 03:00:00",
                "elementValue": [
                    {
                        "value": "21",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-08 06:00:00",
                "elementValue": [
                    {
                        "value": "20",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-08 09:00:00",
                "elementValue": [
                    {
                        "value": "21",
                        "measures": "攝氏度"
                    }
                ]
            }
        ]
    },
    "T": {
        "elementName": "T",
        "description": "溫度",
        "time": [
            {
                "dataTime": "2023-01-05 12:00:00",
                "elementValue": [
                    {
                        "value": "23",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-05 15:00:00",
                "elementValue": [
                    {
                        "value": "24",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-05 18:00:00",
                "elementValue": [
                    {
                        "value": "22",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-05 21:00:00",
                "elementValue": [
                    {
                        "value": "20",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 00:00:00",
                "elementValue": [
                    {
                        "value": "19",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 03:00:00",
                "elementValue": [
                    {
                        "value": "18",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 06:00:00",
                "elementValue": [
                    {
                        "value": "18",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 09:00:00",
                "elementValue": [
                    {
                        "value": "19",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 12:00:00",
                "elementValue": [
                    {
                        "value": "23",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 15:00:00",
                "elementValue": [
                    {
                        "value": "24",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 18:00:00",
                "elementValue": [
                    {
                        "value": "22",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-06 21:00:00",
                "elementValue": [
                    {
                        "value": "20",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 00:00:00",
                "elementValue": [
                    {
                        "value": "19",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 03:00:00",
                "elementValue": [
                    {
                        "value": "18",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 06:00:00",
                "elementValue": [
                    {
                        "value": "18",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 09:00:00",
                "elementValue": [
                    {
                        "value": "18",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 12:00:00",
                "elementValue": [
                    {
                        "value": "22",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 15:00:00",
                "elementValue": [
                    {
                        "value": "23",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 18:00:00",
                "elementValue": [
                    {
                        "value": "22",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-07 21:00:00",
                "elementValue": [
                    {
                        "value": "21",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-08 00:00:00",
                "elementValue": [
                    {
                        "value": "20",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-08 03:00:00",
                "elementValue": [
                    {
                        "value": "19",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-08 06:00:00",
                "elementValue": [
                    {
                        "value": "19",
                        "measures": "攝氏度"
                    }
                ]
            },
            {
                "dataTime": "2023-01-08 09:00:00",
                "elementValue": [
                    {
                        "value": "20",
                        "measures": "攝氏度"
                    }
                ]
            }
        ]
    },
    "PoP6h": {
        "elementName": "PoP6h",
        "description": "6小時降雨機率",
        "time": [
            {
                "startTime": "2023-01-05 12:00:00",
                "endTime": "2023-01-05 18:00:00",
                "elementValue": [
                    {
                        "value": "0",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-05 18:00:00",
                "endTime": "2023-01-06 00:00:00",
                "elementValue": [
                    {
                        "value": "0",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 00:00:00",
                "endTime": "2023-01-06 06:00:00",
                "elementValue": [
                    {
                        "value": "0",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 06:00:00",
                "endTime": "2023-01-06 12:00:00",
                "elementValue": [
                    {
                        "value": "0",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 12:00:00",
                "endTime": "2023-01-06 18:00:00",
                "elementValue": [
                    {
                        "value": "0",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-06 18:00:00",
                "endTime": "2023-01-07 00:00:00",
                "elementValue": [
                    {
                        "value": "10",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 00:00:00",
                "endTime": "2023-01-07 06:00:00",
                "elementValue": [
                    {
                        "value": "10",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 06:00:00",
                "endTime": "2023-01-07 12:00:00",
                "elementValue": [
                    {
                        "value": "10",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 12:00:00",
                "endTime": "2023-01-07 18:00:00",
                "elementValue": [
                    {
                        "value": "10",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-07 18:00:00",
                "endTime": "2023-01-08 00:00:00",
                "elementValue": [
                    {
                        "value": "10",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-08 00:00:00",
                "endTime": "2023-01-08 06:00:00",
                "elementValue": [
                    {
                        "value": "10",
                        "measures": "百分比"
                    }
                ]
            },
            {
                "startTime": "2023-01-08 06:00:00",
                "endTime": "2023-01-08 12:00:00",
                "elementValue": [
                    {
                        "value": "10",
                        "measures": "百分比"
                    }
                ]
            }
        ]
    }
}
```
# 休假 ( holidayAgent )
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
        "tw_date": "2023/01/05",
        "tw_time": "13:34:06",
        "tw_day": "(四)"
    },
    "data": [
        {
            "user_id": "11542",
            "agent_id": "11545",
            "part_type": "一日",
            "user_name": "陳欣怡",
            "agent_name": "龔玉惠"
        },
        {
            "user_id": "11548",
            "agent_id": "11202",
            "part_type": "上休",
            "user_name": "李文瀚",
            "agent_name": "孟國揚"
        }
    ]
}
```
# 飲水輪值 ( waterShift )
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
        "tw_date": "2023/01/05",
        "tw_time": "14:40:07",
        "tw_day": "(四)"
    },
    "data": {
        "user_now": "李文瀚",
        "user_next": "陳欣怡"
    }
}
```
# 機房輪值 ( engineShift )
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
        "tw_date": "2023/01/05",
        "tw_time": "14:46:26",
        "tw_day": "(四)"
    },
    "data": {
        "user_now": "李文瀚",
        "user_next": "陳欣怡"
    }
}
```