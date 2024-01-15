---
title: Nginx
description: 
published: true
date: 2024-01-12T00:40:10.897Z
tags: web server
editor: markdown
dateCreated: 2023-05-18T07:41:06.982Z
---

# Nginx
- [ ] [Nginx 是什麼？有哪些用途？](https://www.explainthis.io/zh-hant/swe/why-nginx)
- [ ] [Why is Nginx called a “reverse” proxy?](https://blog.bytebytego.com/p/ep25-proxy-vs-reverse-proxy?utm_source=profile&utm_medium=reader2)
- [ ] [[基礎觀念系列] Web Server & Nginx — (1)](https://medium.com/starbugs/web-server-nginx-1-cf5188459108)
- [ ] [[基礎觀念系列] Web Server & Nginx — (2)](https://medium.com/starbugs/web-server-nginx-2-bc41c6268646)
- [ ] [[教學][Ubuntu 架站] 在 Ubuntu 22.04 安裝 Nginx 網頁伺服器，並架設多個網站（多網域）](https://ui-code.com/archives/667)
- [ ] [認識NGINX](https://vocus.cc/article/62a01388fd89780001e4656b)
- [ ] [使用Ubuntu Server架設Nginx伺服器](https://magiclen.org/ubuntu-server-nginx/)
- [ ] [advanced-nginx-cheatsheet](https://virtubox.github.io/advanced-nginx-cheatsheet/)

![forward proxy and a reverse proxy.png](http://192.168.25.60:8000/files/file_storage/afc85ebc.png)

# Nginx 介紹
> Nginx 是全球最受歡迎的網頁伺服器之一，負責託管互聯網上一些最大和最高流量的網站。它是一個輕量級的選擇，可用作網頁伺服器或反向代理。

![nginx logo.png](http://192.168.25.60:8000/files/file_storage/3421c13f.png)

Nginx 是一個非同步框架的網頁伺服器，可以做到

- 反向代理
- 負載均衡
- http 快取

Nginx 比起 Apache 屬於輕量級且高併發、處理靜態檔案的效率較高、耗費的記憶體較少、負載效能好，很適合做前端伺服器使用。

## 反向代理 Reverse Proxy
反向代理的好處在於能夠將 Client 不需知道 Application Server 的真實位置，僅需要透過 Nginx 反向代理的方式就能夠向後面的 Application Server 發送請求，而 Application Server 也不需要知道是哪一個 Client 的 Request，僅需回傳 Response 即可。(如下圖所示）

![Nginx 反向代理.png](http://192.168.25.60:8000/files/file_storage/8da5fc89.png)

## 負載均衡 Load Balance
為了因應大流量，一台 Application Server 是無法應付的，因此會需要同時開多個 Application Server 。而 Nginx 能夠自動的將 Client 的 Request 分送到不同 Application Server 上，而分送的演算法可以自己設計，最常使用的是 Round Robin 演算法，而其他的演算法也包含 Least Connections 、Least Time 、IP Hash 等。

![Nginx Load Balance 機制.png](http://192.168.25.60:8000/files/file_storage/1096fc9f.png)

## HTTP 快取
為了能夠提高效能，Nginx 會利用 http 快取的機製做優化。流程如下：

1. Client 發出 Request ，Nginx 會將 Request 的資訊做 hash，並判斷此 hash key 是否存在於記憶體中：① → ②
1-1. 若 hash key 不存在於記憶體中：Nginx 會向 Application Server 索取檔案位置，再去索取檔案。 ③ → ④ → ⑤
1-2. 若存在於記憶體中：Nginx 會直接索取檔案。 ③ → ⑤
2. 將檔案回傳給 Client

![Nginx Cache 機制.png](http://192.168.25.60:8000/files/file_storage/74af2a73.png)


# Nginx 和 Apache 的差異
Nginx 和 Apache 都是非常流行的 Web 服務器軟體，被全世界的許多網站所使用。儘管他們都能提供 Web 服務器的核心功能，但他們在許多細節上存在一些不同

## 性能
Nginx 因其事件驅動架構而著名，能夠更好地處理大量的同時連線，尤其是靜態內容的傳輸，所以在高流量的情況下，Nginx 通常能提供較高的性能。而 Apache 則使用基於進程或線程的模型，這可能導致在高流量情況下性能下降。

## 模塊和靈活性
Apache 的模塊系統可以讓開發者根據需求來擴展其功能。許多功能，如 SSL 加密、URL 重寫等都是以模塊的形式存在。這使得 Apache 在靈活性和可客製化方面有較大的優勢。雖然 Nginx 也提供模塊，但其模塊必須在編譯 Nginx 的時候就加入，無法像Apache一樣動態載入。

## 組態和管理
Apache 的 .htaccess 檔案讓網站管理員可以對單個目錄進行詳細的組態，這對於共享主機或有許多獨立用戶的情況特別有用。但這也可能導致性能下降，因為 Apache 需要為每個請求檢查並處理 .htaccess 檔案。另一方面， Nginx 不支援這樣的功能，但其組態檔案通常被認為更簡潔和直觀。

## 對 PHP 的支援
Apache 可以透過 mod_php 模塊直接處理 PHP ，這使得 Apache 對 PHP 的支援更好。雖然 Nginx 也可以通過 FastCGI 與 PHP 進程通訊來處理 PHP ，但這需要更多的組態。

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

## 設置伺服器區塊（類似於 Apache 的虛擬主機）
待補充



## 熟悉重要的 Nginx 檔案和目錄

你應該花一些時間來熟悉幾個重要的目錄和檔案。

### 內容

`/var/www/html`：實際的網頁內容，預設只包含你之前看到的預設 Nginx 頁面，是由 `/var/www/html` 目錄提供的。這個可以透過修改 Nginx 的配置檔案來改變。

### 伺服器配置

`/etc/nginx`：Nginx 的配置目錄。所有的 Nginx 配置檔案都位於這裡。
`/etc/nginx/nginx.conf`：主要的 Nginx 配置檔案。這個檔案可以修改，用來變更 Nginx 的全域配置。
`/etc/nginx/sites-available/`：這個目錄可以儲存每個網站的伺服器區塊配置。除非這些配置檔案連結到了 sites-enabled 目錄，Nginx 不會使用這個目錄下的配置檔案。通常，所有伺服器區塊的配置都在這個目錄中完成，然後透過連結到其他目錄來啟用。
`/etc/nginx/sites-enabled/`：這個目錄儲存已啟用的每個網站伺服器區塊。通常，這些配置是通過連結到 sites-available 目錄中的檔案來創建的。
`/etc/nginx/snippets`：這個目錄包含可以在其他 Nginx 配置中引用的配置片段。可能需要重複使用的配置段落適合重構為 snippets。

### 伺服器記錄

`/var/log/nginx/access.log`：每一個對你的網頁伺服器的請求都會被記錄在這個日誌檔案中，除非 Nginx 配置為不這麼做。
`/var/log/nginx/error.log`：任何 Nginx 的錯誤都會被記錄在這個日誌中。


# Nginx 設定檔結構
nginx 設定檔由不同模組組成，通過模組化的方式實現不同的功能。
組態指令分為簡單指令和塊指令。一個簡單的指令，包括名稱，用空格分隔參數，並用分號（;）結束。
一個塊指令由一個或多個簡單具有相同的結構簡單指令組合而成，使用一組用`{}`括弧括起來表示塊結束。
上下文(context)：一個塊的指令包含有大括弧其他指令，它被稱為上下文（例如：事件，HTTP，伺服器，和位置）。
放置在設定檔中的任何上下文以外的指令都被認為是在主上下文。
主要結構如下：

```
# Events 用於組態 IO 模型，如 epoll、kqueue、select 或 poll 等，它們是必備模組。
events {       
    ...
}

# http 上下文專用於組態用於 http 的各模組
http {
      # 包括用戶端類指令，檔IO類指令，hash 類指令，通訊端類指令等 
      ...
      server {
          # 用於定義虛擬伺服器相關的屬性，常見的指令有 backlog、rcvbuf、bind 及 sndbuf 等
           ...
      }
      server {
            ...
      }
    ...
}
```
## 基本的nginx.conf組態描述
```
user  www-data;        # 指定運行 worker 進程的使用者和組
worker_processes  auto;  # worker 執行緒的個數；通常應該為物理 CPU 核心個數減1；

error_log  logs/error.log;        # 組態錯誤日誌檔位置及日誌記錄級別
error_log  logs/error.log  notice;# 可用於 main、http、server 及 location 上下文中
error_log  logs/error.log  info;  # 語法格式為 error_log file |stderr [debug|info|notice|warn|error|crit|alert|emerg]

pid        /run/nginx.pid;          # 指定 pid 存放路徑   

events {
    worker_connections  1024; # 每個 worker 進程所能夠回應的最大併發請求數；
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    # 此部分用於設置訪問日誌的格式及位置
    #access_log  logs/access.log  main;

    sendfile        on;    # 檔案發送
    #tcp_nopush     on;

    keepalive_timeout  65;  # 保持連接的超時時長，默認為 65 s

    #gzip  on;   # 是否開啟 gzip 壓縮

    server {
        # 定義 server 的網址，可定義多個, 匹配順序如下
        # 1. exact name 
        # 2. Longest wildcard starting with an asterisk, such as *.example.org
        # 3. Longest wildcard ending with an asterisk, such as mail.*
        # 4. First matching regular expression
        server_name  localhost par.cse.nsysu.edu.tw;

        # 定義監聽的埠
        listen       80;        

        # 定義字元集
        charset koi8-r;        

        access_log  logs/host.access.log  main;  # 訪問日誌檔存放路徑

        # location 通常用於 server 上下文中，用於設定某 URI 的訪問屬性。location 可以巢狀。 
        # location 有兩種設定法 prefix string 與 regular expression.
        # match 順序如下：
        # 1. Test the URI against all prefix strings.
        # 2. The = (equals sign) modifier defines an exact match of the URI and a prefix string. If the exact match is found, the search stops.
        # 3. If the ^~ (caret-tilde) modifier prepends the longest matching prefix string, the regular expressions are not checked.
        # 4. Store the longest matching prefix string.
        # 5. Test the URI against regular expressions.
        # 6. Break on the first matching regular expression and use the corresponding location.
        # 7. If no regular expression matches, use the location corresponding to the stored prefix string.

        # 以路徑描述的為 prefix string 如下：
        location / {     
            # 預設首頁檔位置，此處當前為相對路徑，/etc/nginx/html
            root   html;  

            # 首頁檔順序，如果找不到 index.html,則找 index.htm
            index  index.html index.htm; 
        }

        # 以 tilde(~)符號開頭的為大小寫有關的 regular expression
        # 而 tilde star(~*)符號為大小寫無關的 regular expression
        location ~ \.html? {
            ...
        }   

        # 以下部分根據 http 狀態碼重新導向錯誤頁面
        #error_page  404              /404.html;

        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }
    }
}
```


