---
title: Command Line
description: 
published: true
date: 2023-05-17T02:01:28.624Z
tags: gui, cli
editor: markdown
dateCreated: 2022-11-14T00:11:05.585Z
---

# 2-1 什麼是 Command Line?
## GUI ( Graphical User Interface ) 圖形使用者介面
> 指採用圖形方式顯示的電腦操作使用者介面

![GUI example.png](http://192.168.25.60:8000/files/file_storage/60dd0736.png)

## CLI ( Command Line Interface ) 命令列介面
> 在圖形使用者介面得到普及之前使用最為廣泛的使用者介面，它通常不支援滑鼠，使用者通過鍵盤輸入指令，電腦接收到指令後，予以執行
### 為什麼要用文字跟電腦溝通?
1. 有些功能只能用 CLI
2. 有時候用文字比較快
### 如何呼叫 CLI ?
1. Mac：終端機 Terminal.app
2. Windows：命令提示字元 cmd.exe
### 使用 [bellard](https://bellard.org/jslinux/) 模擬 CLI
> 在瀏覽器上模擬簡單的作用系統

# 2-2 基本指令練習，用文字來操作檔案
## pwd ( print working directory )
> 印出現在在哪裡
```shell
pwd
```

## ls ( list segment )
> 印出所有檔案
```shell
ls
```
```shell
ls -l
```

## cd ( change directory )
> 切換資料夾
```shell
cd 
```
- 切換到 home 目錄
```shell
cd ~
```
- 切換到 根目錄
```shell
cd /
```

## touch
> 碰一下檔案，若無此檔案則新增檔案
```shell
touch
```

## mkdir ( make directory )
> 新增資料夾
```shell
mkdir 資料夾
```

## rm ( remove )
> 刪除檔案
```shell
rm 檔案
```

## cp ( copy )
> 複製檔案
```shell
cp 原本檔案名稱 複製檔案名稱
```

## mv ( move )
> 移動檔案或改檔案名稱
```shell
mv 原本檔案或目錄 目的檔案或目錄
```

- 改檔案名稱
```shell
mv 檔案名稱 更改的檔案名稱
```

## man ( manual )
> 使用說明
```shell
man 功能
```

- 離開使用說明
```shell
q
```

# 2-3 更多常用 command line 指令
## date
> 印出現在時間
```shell
date
```

## top ( table of processes )
> 印出所有 processes
```shell
top
```

## cat ( catenate )
> 連接檔案或查看檔案內容
```shell
cat 檔案
```

## less
> 分頁式印出檔案
```shell
less 檔案
```

## grep
> 抓取特定關鍵字
```shell
grep 關鍵字 檔案
```

## echo
> 印出字串
```shell
echo 字串
```

# 2-4 發揮更大的力量--指令的組合技
## | ( pipe )
> - 串接指令
> - 把前面的輸出變成後面的輸入
```shell
cat file.txt | grep hi
```

## > ( redirect )
> 重新導向
- 將 hello 寫入 test.txt
```shell
echo hello > test.txt
```
- 將 date 寫入 time.txt
```shell
date > time.txt
```


