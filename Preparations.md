---
title: Preparations
description: 準備事項(技能樹、軟體安裝、環境建置)
published: true
date: 2023-11-03T01:40:52.483Z
tags: 
editor: markdown
dateCreated: 2022-04-28T00:51:37.605Z
---

# Skill Tree
- [ ] [roadmap](https://roadmap.sh/)
- [ ] [developer-roadmap](https://github.com/kamranahmedse/developer-roadmap)
- [ ] [developer-roadmap-chinese](https://github.com/goodjack/developer-roadmap-chinese)
- [ ] [backend](http://192.168.25.60:8000/files/file_storage/210d883f.pdf)
- [ ] [ExplainThis](https://www.explainthis.io/zh-hant/about)
- [ ] [IT-Tools](https://it.jason.tools/)
- [ ] [Essential skills for a web developer](https://learningdaily.dev/essential-skills-for-a-web-developer-5dbc052cea89)
## 簡介 intro

![web developer skills.png](http://192.168.25.60:8000/files/file_storage/bceac928.png)

![intro.png](http://192.168.25.60:8000/files/file_storage/cafd78d5.png)

![software engineering in nutshell.png](http://192.168.25.60:8000/files/file_storage/ab0af574.png)

## 後端 backend

![backend.png](http://192.168.25.60:8000/files/file_storage/c5079943.png)

![Backend burger.png](http://192.168.25.60:8000/files/file_storage/496fc46b.png)

## 開發維運 devops

![devops.png](http://192.168.25.60:8000/files/file_storage/d27312da.png)

## 前端 frontend

![frontend.png](http://192.168.25.60:8000/files/file_storage/e1425fd3.png)

# Install Software

## Visual Studio Code
### Version
```shell
 code -v
```
### Extension Module
- Chinese (Traditional) Language Pack for Visual Studio Code
- Prettier - Code formatter
- ESLint
```shell
npm install -g eslint
eslint --init
```
- Prettier ESLint
- vscode-icons
- Path Intellisense
- Prisma (ORM)
- Git History
- gitignore
- GitLens — Git supercharged
- Jest Runner
- TODO Highlight
- Todo Tree
- Git Graph 
> [usage info](https://ithelp.ithome.com.tw/articles/10267759)
{.is-info}
- CodeSnap (可以輕鬆生成高解析度，精美的程式碼圖片)
> [usage info](https://tw511.com/a/01/43207.html)
{.is-info}
- Code Spell Checker
- live server 
- SQLite (搭配 SQLite 使用)：檢視 ➔ 命令選擇區 ➔ 輸入 SQLite: Open Database ➔ 左下方會出現 SQLITE EXPLORER ➔ 可查看 table 及其資料
- Codeium (自動化生成程式碼。它支援超過 40 種語言，且仍在不斷擴充中。)
> [usage info](https://codeium.com/vscode_tutorial)
{.is-info}
- code runner
- AWS Toolkit
> [usage info](https://aws.amazon.com/tw/codewhisperer/)
{.is-info}
- New Relic CodeStream
- Postman
- CodiumAI (Test writing)
## [Postman](https://www.postman.com/)
### Learning
> [[POSTMAN] 該知道的都知道，不知道的慢慢瞭解 - 與波斯麵三十天的感情培養](https://ithelp.ithome.com.tw/articles/10289954)
{.is-info}
### Set Environment
> [set environment info](https://blog.yowko.com/postman-parameter-test/)
{.is-info}
### Pre-request Scripts
> [Pre-request Scripts info](https://learning.postman.com/docs/writing-scripts/script-references/postman-sandbox-api-reference/)
{.is-info}

## Discord
## SQL Server
> [SQL Server download info](https://docs.microsoft.com/zh-tw/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)
{.is-info}
- DB : sql0802-srv

## DBeaver
> [DBeaver download info](https://dbeaver.io/download/)
{.is-info}

## SourceTree (Option)
> [SourceTree download info](https://www.sourcetreeapp.com/)
{.is-info}
- set developer info in SourceTree for each project with the following steps
1. Settings
2. Advanced
3. uncheck the option : Use global user settings
4. then you can see developer info in Author when commit 

![sourcetree setting user info.png](http://192.168.25.60:8000/files/file_storage/6f2b4f9b.png)

## Sumatra PDF
> [Sumatra PDF download info](https://www.sumatrapdfreader.org/free-pdf-reader)

## Notepad++
- set in the direction `C:\Program Files (x86)`

## Ngrok (Option)
> - [快速讓外網連接本機的利器 — ngrok](https://medium.com/%E4%BC%81%E9%B5%9D%E4%B9%9F%E6%87%82%E7%A8%8B%E5%BC%8F%E8%A8%AD%E8%A8%88/%E5%BF%AB%E9%80%9F%E8%AE%93%E5%A4%96%E7%B6%B2%E9%80%A3%E6%8E%A5%E6%9C%AC%E6%A9%9F%E7%9A%84%E5%88%A9%E5%99%A8-ngrok-ac92f792e1f0)
> - [架設臨時伺服器不求人，用 ngrok 一個指令搞定！（含 Windows + MacOS 環境變數教學）](https://israynotarray.com/other/20230210/1090666501/)
{.is-info}


### Download and use ngrok.exe
> [Ngrok download info](https://ngrok.com/download)
{.is-info}

### Login Account
https://dashboard.ngrok.com/get-started/setup

- get `add-authtoken`

![ngrok login account to get add-authtoken.png](http://192.168.25.60:8000/files/file_storage/9418413e.png)

### Usage
path：`C:\Program Files (x86)\ngrok-v3-stable-windows-amd64\ngrok.exe`

### Connect your account
```shell
ngrok config add-authtoken 20MMLwQdaSofUxU14BZFFBqDcFv_2mhM2W7XfAds39MJv4cr8
```

![ngrok config add-authtoken.png](http://192.168.25.60:8000/files/file_storage/acc8c650.png)

### Fire it up
```shell
// listen port
ngrok http 8110
```

![ngrok http port.png](http://192.168.25.60:8000/files/file_storage/f514581b.png)

### Gain ngrok url
- random url will change when ngrok restart

https://a7ab-122-147-204-116.jp.ngrok.io

![ngrok access local resource.png](http://192.168.25.60:8000/files/file_storage/be74f518.png)

### Access local resource

https://a7ab-122-147-204-116.jp.ngrok.io/swagger/WebSignin_API

![ngrok access local resource swagger.png](http://192.168.25.60:8000/files/file_storage/3159178e.png)

## cmder
> [cmder download info](https://cmder.en.softonic.com/?ex=CORE-1224.1)
{.is-info}

### Usage
[介紹好用工具：Cmder ( 具有 Linux 溫度的 Windows 命令提示字元工具 )](https://blog.miniasp.com/post/2015/09/27/Useful-tool-Cmder)

## WinSCP
> [WinSCP download info](https://winscp.net/eng/downloads.php)
{.is-info}

## Lightshot
> [Lightshot download info](https://app.prntscr.com/zh-cn/index.html)
{.is-info}

## TryCloudflare (Option)
> [使用 Cloudflare Tunnel 的 TryCloudflare 取代 ngrok](https://blog.miniasp.com/post/2023/08/25/Cloudflare-Tunnel-TryCloudflare)
{.is-info}


# Build Environment
| 工具 | 功能|
|---|:--:|
|nvm|管理 Node.js 版本|
|npm|管理 Node.js 套件|
|node|Node.js 執行程式|

## nvm
> nvm 是用來管理 Node.js 版本的工具。它可以讓我們在同一台電腦上安裝和切換不同的 Node.js 版本。

> [nvm download info](https://github.com/coreybutler/nvm-windows)
{.is-info}

> cmd 使用管理員身分執行
{.is-warning}

> 安裝時將 nvm 及 node 都安裝在 `D:\SDK`
{.is-warning}

- install nvm 
```bash
nvm install
```
- get nvm version list
```
nvm list
```
- show now using node.js version
```bash
nvm version
```
- location：D:\SDK

## npm
> npm 是用來管理 Node.js 套件的工具。npm 可以讓我們下載、安裝、更新和卸載 Node.js 套件。

- install
```
npm install -g npm
npm install -g npm@10.2.2
```
- get package list
```
npm list
```
## node
> node 是 Node.js 的執行程式。當我們執行 node 命令時，它將啟動 Node.js 執行環境，並允許我們執行 JavaScript 程式碼。
> [node releases](https://nodejs.org/en/about/previous-releases)

- install this node.js version
```bash
nvm install 14.18.0
nvm install 18.17.0
nvm install 20.9.0
```
- use this node.js version
```bash
nvm use 14.18.0
nvm use 18.17.0
nvm use 20.9.0
```
- check node.js version
```bash
node -v
```

![install node.png](http://192.168.25.60:8000/files/file_storage/94f4088b.png)

- 資料夾 `SDK` 會出現兩個資料夾 `nvm` 與 `nodejs`

![nvm install node.png](http://192.168.25.60:8000/files/file_storage/5bd76ea7.png)

- 設定 `nvm` 與 `nodejs` 的環境變數
- `NVM_HOME`：`D:\SDK\nvm`
- `NVM_SYMLINK`：`D:\SDK\nodejs`

![node 環境變數設定.png](http://192.168.25.60:8000/files/file_storage/91d290b2.png)
 
# SonarQube 程式碼品質分析工具
## Install jdk
https://www.oracle.com/java/technologies/downloads/#jdk18-windows
## Download sonar-scanner
https://binaries.sonarsource.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-4.7.0.2747-windows.zip
## Set environment variables
https://www.cnblogs.com/Uni-Hoang/p/15207178.html
## Check installation is successful
```shell
sonar-scanner -v
```
```shell
INFO: Scanner configuration file: C:\Program Files\sonar-scanner-4.7.0.2747-windows\bin\..\conf\sonar-scanner.properties
INFO: Project root configuration file: NONE
INFO: SonarScanner 4.7.0.2747
INFO: Java 11.0.14.1 Eclipse Adoptium (64-bit)
INFO: Windows 10 10.0 amd64
```
```shell
sonar-scanner -version
```
```shell
ERROR: Unrecognized option: -version
INFO:
INFO: usage: sonar-scanner [options]
INFO:
INFO: Options:
INFO:  -D,--define <arg>     Define property
INFO:  -h,--help             Display help information
INFO:  -v,--version          Display version information
INFO:  -X,--debug            Produce execution debug output
```
![sonar-scanner install successful.png](http://192.168.25.60:8000/files/file_storage/d04cdf0d.png)
## Add sonar-scanner config in the project root
C:\Program Files\sonar-scanner-4.7.0.2747-windows\conf
copy sonar-scanner.properties to project

![sonar-scanner config.png](http://192.168.25.60:8000/files/file_storage/aafa691b.png)
## Excute sonar-scanner
```
// cd to project
D:\mandy\Project\Koa\WebSignin_API>
```
```shell
// excute sonar-scanner
sonar-scanner.bat 
-D"sonar.projectKey=WebSignin_API" 
-D"sonar.sources=." 
-D"sonar.host.url=http://192.168.25.59:9005" 
-D"sonar.login=d9951190925904006e8e49454bc0588d9b9661ff"
```
![sonar-scanner excute.png](http://192.168.25.60:8000/files/file_storage/02cf248c.png)
## Login
http://192.168.25.59:9005/
```
帳號：11542
密碼：qaz123
```

