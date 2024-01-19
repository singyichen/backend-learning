---
title: Nginx Install
description: Nginx 安裝
published: true
date: 2024-01-18T00:33:49.832Z
tags: web server
editor: markdown
dateCreated: 2024-01-18T00:20:12.388Z
---

# Nginx Install
- [ ] [Nginx 是什麼？有哪些用途？](https://www.explainthis.io/zh-hant/swe/why-nginx)
- [ ] [Why is Nginx called a “reverse” proxy?](https://blog.bytebytego.com/p/ep25-proxy-vs-reverse-proxy?utm_source=profile&utm_medium=reader2)
- [ ] [[基礎觀念系列] Web Server & Nginx — (1)](https://medium.com/starbugs/web-server-nginx-1-cf5188459108)
- [ ] [[基礎觀念系列] Web Server & Nginx — (2)](https://medium.com/starbugs/web-server-nginx-2-bc41c6268646)
- [ ] [[教學][Ubuntu 架站] 在 Ubuntu 22.04 安裝 Nginx 網頁伺服器，並架設多個網站（多網域）](https://ui-code.com/archives/667)
- [ ] [認識NGINX](https://vocus.cc/article/62a01388fd89780001e4656b)
- [ ] [使用Ubuntu Server架設Nginx伺服器](https://magiclen.org/ubuntu-server-nginx/)
- [ ] [advanced-nginx-cheatsheet](https://virtubox.github.io/advanced-nginx-cheatsheet/)
- [ ] [[Nginx] 設定](https://hackmd.io/@winnienotes/SymsGlMfj)


# Nginx 安裝

在這個指南中，我們將討論如何在您的 Ubuntu 22.04 伺服器上安裝 Nginx，調整防火牆，管理 Nginx 進程，並設定伺服器區塊，從單個伺服器上託管多個域名。

## 安裝 Nginx
因為 Nginx 在 Ubuntu 預設的軟體庫有提供，所以我們可以透過 apt 套件管理系統從這些軟體庫中安裝它。

既然這是我們在這次對話中第一次使用 apt 套件管理系統，我們要先更新一下我們本地的套件清單，這樣就能取得最新的套件資訊。然後，我們就可以開始安裝 nginx 了：

```shell
sudo apt update && sudo apt install nginx
```

當系統詢問你是否確認要安裝時，按下「`Y`」。

如果系統詢問是否要重新啟動任何服務，直接按下「`ENTER`」接受預設設定並繼續。

apt 會將 Nginx 及其所需的相依套件安裝到你的伺服器上。

## 設置防火牆（Configure Firewall）
在測試 Nginx 之前，需要設定防火牆軟體，以允許訪問該服務。Nginx 在安裝時會在 ufw 中註冊自己作為一個服務，因此很容易允許 Nginx 訪問。

輸入以下指令列出 ufw 知道如何處理的應用程式組態：

```shell
sudo ufw app list
```

你應該會看到一個應用程式組態的清單：

```
Available applications:
  Nginx Full
  Nginx HTTP
  Nginx HTTPS
  OpenSSH
```

如輸出所示，Nginx 有三個可用的組態檔案：

- Nginx Full：這個組態開啟了 80 號埠口（普通、未加密的網頁流量）和 443 號埠口（TLS/SSL 加密流量）
- Nginx HTTP：這個組態僅開啟 80 號埠口（普通、未加密的網頁流量）
- Nginx HTTPS：這個組態僅開啟 443 號埠口（TLS/SSL 加密流量）

建議你啟用最限制的組態，只要還能允許你所設定的流量。目前，我們只需要允許 80 號埠口上的流量。

我們將從為 SSH 新增防火牆規則開始，因為如果您遠程組態伺服器，您肯定不希望在啟用防火牆時被鎖定在外！

```shell
sudo ufw allow OpenSSH
```

你可以透過輸入以下指令來啟用 Nginx HTTP：

```shell
sudo ufw allow 'Nginx HTTP'
```

現在啟用防火牆

```shell
sudo ufw enable
```

如果看到`「Command may disrupt existing ssh connections. Proceed with operation (y|n)?」`，請按 `y`。

你可以透過輸入以下指令來驗證變更：

```shell
sudo ufw status
```

```
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
```

## 測試 Nginx
在安裝過程結束時，Ubuntu 22.04 會啟動 Nginx。網頁伺服器應該已經運行起來了。

我們可以使用 systemd 啟動系統來檢查服務是否正在運行，只需輸入以下指令：
```shell
systemctl status nginx
```

```
● nginx.service - A high performance web server and a reverse proxy server
     Loaded: loaded (/lib/systemd/system/nginx.service; enabled; vendor preset: enabled)
     Active: active (running) since Thu 2024-01-11 13:53:43 CST; 1min 51s ago
       Docs: man:nginx(8)
   Main PID: 3370 (nginx)
      Tasks: 2 (limit: 11562)
     Memory: 5.1M
     CGroup: /system.slice/nginx.service
             ├─3370 nginx: master process /usr/sbin/nginx -g daemon on; master_process on;
             └─3371 nginx: worker process

Jan 11 13:53:43 mandychen systemd[1]: Starting A high performance web server and a reverse proxy server...
Jan 11 13:53:43 mandychen systemd[1]: Started A high performance web server and a reverse proxy server.
```

從這個輸出可以確認服務已經成功啟動了。不過，最好的方式是實際從 Nginx 要求一個網頁。

你可以通過訪問預設的 Nginx 登陸頁面，來確認軟體運行正常。只需前往你伺服器的 IP 地址。如果你不知道伺服器的 IP 地址，你可以使用 icanhazip.com 工具來查找，它會給你從互聯網的其他位置接收到的公共 IP 地址：

```shell
curl -4 icanhazip.com
```

```
122.147.204.116
```
當你取得了伺服器的 IP 地址，就將它輸入到你瀏覽器的地址欄中：

`http://122.147.204.116`

你應該會看到預設的 Nginx 登陸頁面：


如果你看到這個頁面，代表你的伺服器運行正常，已經可以開始進行管理了。

## 管理 Nginx 進程（Nginx Process）
現在你已經讓你的網頁伺服器運行起來了，讓我們來回顧一些基本的管理指令。

- 要停止你的網頁伺服器，輸入以下指令：

```shell
sudo systemctl stop nginx
```

- 如果網頁伺服器停止了，要重新啟動它，請輸入以下指令：

```shell
sudo systemctl start nginx
```

- 如果要先停止再重新啟動服務，請輸入以下指令：

```shell
sudo systemctl restart nginx
```

- 如果你只是在進行設定更改，通常 Nginx 可以在不斷開連線的情況下重新載入。要這麼做，輸入以下指令：

```shell
sudo systemctl reload nginx
```

- 預設情況下，Nginx 設定為在伺服器啟動時自動啟動。如果你不希望這樣，可以透過輸入以下指令來停用這個功能：

```shell
sudo systemctl disable nginx
```

- 若要重新啟用服務在開機時自動啟動，請輸入以下指令：

```shell
sudo systemctl enable nginx
```

你現在已經學會了基本的管理指令，準備好配置網站以托管多個域名了。




