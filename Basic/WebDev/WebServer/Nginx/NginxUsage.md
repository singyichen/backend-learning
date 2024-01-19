---
title: Nginx Usage
description: Nginx 使用
published: true
date: 2024-01-19T03:16:39.646Z
tags: web server
editor: markdown
dateCreated: 2024-01-18T00:23:12.057Z
---

# Nginx Usage
- [ ] [Nginx 是什麼？有哪些用途？](https://www.explainthis.io/zh-hant/swe/why-nginx)
- [ ] [Why is Nginx called a “reverse” proxy?](https://blog.bytebytego.com/p/ep25-proxy-vs-reverse-proxy?utm_source=profile&utm_medium=reader2)
- [ ] [[基礎觀念系列] Web Server & Nginx — (1)](https://medium.com/starbugs/web-server-nginx-1-cf5188459108)
- [ ] [[基礎觀念系列] Web Server & Nginx — (2)](https://medium.com/starbugs/web-server-nginx-2-bc41c6268646)
- [ ] [[教學][Ubuntu 架站] 在 Ubuntu 22.04 安裝 Nginx 網頁伺服器，並架設多個網站（多網域）](https://ui-code.com/archives/667)
- [ ] [認識NGINX](https://vocus.cc/article/62a01388fd89780001e4656b)
- [ ] [使用Ubuntu Server架設Nginx伺服器](https://magiclen.org/ubuntu-server-nginx/)
- [ ] [advanced-nginx-cheatsheet](https://virtubox.github.io/advanced-nginx-cheatsheet/)
- [ ] [[Nginx] 設定](https://hackmd.io/@winnienotes/SymsGlMfj)
- [ ] [用Nginx架設local端https應用](https://hackmd.io/@warrenpig/create-self-signed-https-nginx-app)

# Nginx 資料夾結構
## Nginx 檔案和目錄
### 內容

- `/var/www/html`：實際的網頁內容，預設只包含你之前看到的預設 Nginx 頁面，是由 `/var/www/html` 目錄提供的。這個可以透過修改 Nginx 的配置檔案來改變。

### 伺服器配置
nginx 重要的伺服器配置檔案會放在 `/etc/nginx` 底下

![nginx file structure.png](http://192.168.25.60:8000/files/file_storage/c96ed650.png)

- `/etc/nginx`：Nginx 的配置目錄。所有的 Nginx 配置檔案都位於這裡。
- `/etc/nginx/nginx.conf`：主要的 Nginx 配置檔案。這個檔案可以修改，用來變更 Nginx 的全域配置。
- `/etc/nginx/sites-available/`：這個目錄可以儲存每個網站的伺服器區塊配置。除非這些配置檔案連結到了 sites-enabled 目錄，Nginx 不會使用這個目錄下的配置檔案。通常，所有伺服器區塊的配置都在這個目錄中完成，然後透過連結到其他目錄來啟用。
- `/etc/nginx/sites-enabled/`：這個目錄儲存已啟用的每個網站伺服器區塊。通常，這些配置是通過連結到 sites-available 目錄中的檔案來創建的。
- `/etc/nginx/snippets`：這個目錄包含可以在其他 Nginx 配置中引用的配置片段。可能需要重複使用的配置段落適合重構為 snippets。
- `/etc/nginx/conf.d`：這個目錄是用於存放自定義的 Nginx 配置檔案的地方。這些配置檔案可以包含額外的服務器塊、代理設定、緩存設置等。這樣的組織方式讓您可以將不同的配置內容分開，以增加可讀性和管理性。

在 conf.d 目錄中，您可以創建多個以 .conf 為後綴的配置檔案，例如 example.conf 。這些檔案可以包含獨立的服務器塊或其他配置項，並且它們將被Nginx自動載入並應用。

請注意，`/etc/nginx/conf.d` 目錄中的配置檔案不會自動載入，您需要在主要的 Nginx 配置檔案(/etc/nginx/nginx.conf)中包含相應的 include 語句，以將這些配置檔案包含進來。

以下是一個示例，演示如何在 nginx.conf 中包含 conf.d 目錄下的配置檔案：這樣，Nginx 將在啟動時自動載入和應用 conf.d 目錄中的所有 .conf 檔案中的配置項。

使用 conf.d 目錄的好處是可以將各個配置項分開，方便管理和維護，並且可以方便地添加或禁用特定的配置。

```
http {
    # 其他設定...
    include /etc/nginx/conf.d/*.conf;
}
```

### 伺服器記錄

- `/var/log/nginx/access.log`：每一個對你的網頁伺服器的請求都會被記錄在這個日誌檔案中，除非 Nginx 配置為不這麼做。
- `/var/log/nginx/error.log`：任何 Nginx 的錯誤都會被記錄在這個日誌中。


# Nginx 設定檔結構
- nginx 設定檔由不同模組組成，通過模組化的方式實現不同的功能。
- 組態指令分為簡單指令和塊指令。一個簡單的指令，包括名稱，用空格分隔參數，並用分號（;）結束。
- 一個塊指令由一個或多個簡單具有相同的結構簡單指令組合而成，使用一組用`{}`括弧括起來表示塊結束。
- 上下文(context)：一個塊的指令包含有大括弧其他指令，它被稱為上下文（例如：事件，HTTP，伺服器，和位置）。
- 放置在設定檔中的任何上下文以外的指令都被認為是在主上下文。
- 主要結構如下：

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
## 基本的主設定檔 `nginx.conf` 
```
user  www-data;          # 設定 Nginx 伺服器的執行使用者為 www-data。這是一個標準的使用者，用於執行伺服器進程。
worker_processes  auto;  # 自動設定工作進程的數量。auto 表示根據系統的 CPU 適當地設定工作進程的數量。

error_log  logs/error.log;        # 組態錯誤日誌檔位置及日誌記錄級別
error_log  logs/error.log  notice;# 可用於 main、http、server 及 location 上下文中
error_log  logs/error.log  info;  # 語法格式為 error_log file |stderr [debug|info|notice|warn|error|crit|alert|emerg]

pid        /run/nginx.pid;          # 指定 Nginx 進程 ID 的記錄位置。   

# 定義 Nginx 伺服器的事件處理器。
events {
    worker_connections  1024; # 每個 worker 進程所能夠回應的最大併發請求數；
}

# 定義 Nginx 伺服器的 HTTP 配置。
http {

		##
		# Basic Settings
		##
    
    include       mime.types;
    # 其他設定...
    include /etc/nginx/conf.d/*.conf;
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
    
    
# 設定 Nginx 作為郵件伺服器的功能，例如 POP3 和 IMAP。
#mail {
#	# See sample authentication script at:
#	# http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
# 
#	# auth_http localhost/auth.php;
#	# pop3_capabilities "TOP" "USER";
#	# imap_capabilities "IMAP4rev1" "UIDPLUS";
# 
#	server {
#		listen     localhost:110;
#		protocol   pop3;
#		proxy      on;
#	}
# 
#	server {
#		listen     localhost:143;
#		protocol   imap;
#		proxy      on;
#	}

}
```

# Nginx 代理 socket io http api 
## socket io http server 寫法
- 跨網域設定 nginx 的 server ip：`http://192.168.25.180`
- 設定 connect path 為：`/socket/socket.io`

```js
const io = new socketIO(server, {
  cors: {
    origin: ['http://192.168.25.180'], // nginx server ip
  },
  path: '/socket/socket.io', // socket io path
});
```
- 使用 docker 部屬 api

## 設置伺服器區塊（類似於 Apache 的虛擬主機）
- 在 `/etc/nginx/conf.d` 新增一個 `socket.conf`

```shell
sudo nano /etc/nginx/conf.d/socketio.conf
```

- 設定 nginx 伺服器的名稱為本機 ip：`192.168.25.180`
- 設定 nginx 監聽的 port：`10000`
- 設定 一個 location url 位置：`/socket/`
- 設定請求轉發到目標地址 proxy_pass：`http://192.168.25.180:3000/;`

> proxy_pass 的 url 最後一定要加 `/`
{.is-warning}

![nginx with http socket io api conf example.png](http://192.168.25.60:8000/files/file_storage/4f517e86.png)

```
server {
  listen 10000; 													# 指定 nginx 監聽的端口號
  server_name 192.168.25.180;             # 指定 nginx 伺服器的名稱，用於匹配請求的主機名
  root        /usr/share/nginx/html;      # 指定 nginx 伺服器的根目錄，在該目錄下查找文件來回應請求

  location / {                            # 定義一個 url 位置，匹配所有的請求路徑
        index     index.html index.htm;   # 指定預設的索引文件
        try_files $uri $uri/ /index.html; # 嘗試按照給定的順序查找文件，如果找不到則重定向到 /index.html
  }  

  location /socket/ {                                                   # 定義一個 url 位置，匹配以 `/socket/` 開頭的請求路徑
        proxy_pass         http://192.168.25.180:3000/;                 # 將請求轉發到目標地址
        proxy_set_header   Host $host;                                  # 設置轉發請求時的 Host 頭部字段為原始請求的 Host 頭部字段
        proxy_http_version 1.1;                                         # 指定代理使用的 http 協議版本為 1.1
        proxy_set_header   X-Real-IP $remote_addr;                      # 設置轉發請求時的 X-Real-IP 頭部字段為原始請求的客戶端 IP 地址
        proxy_set_header   X-Forwarded-For $proxy_add_x_forwarded_for;  # 設置轉發請求時的 X-Forwarded-For 頭部字段為原始請求的客戶端 IP 地址
        proxy_set_header   X-Scheme $scheme;                            # 設置轉發請求時的 X-Scheme 頭部字段為原始請求的協議（http 或 https）
        proxy_set_header   Upgrade $http_upgrade;                       # 設置轉發請求時的 Upgrade 頭部字段為原始請求的 Upgrade 頭部字段
        proxy_set_header   Connection "upgrade";                        # 設置轉發請求時的 Connection 頭部字段為"upgrade"
        proxy_connect_timeout 600;                                      # 設置與目標服務器建立連接的超時時間
        proxy_read_timeout 600;                                         # 設置從目標服務器讀取響應的超時時間
        proxy_send_timeout 600;                                         # 設置向目標服務器發送請求的超時時間
        proxy_buffer_size 64k;                                          # 設置代理緩衝區的大小
        proxy_buffers 4 32k;                                            # 設置代理緩衝區的數量為 4 個，每個緩衝區的大小為 32 KB
        proxy_busy_buffers_size 64k;                                    # 設置代理緩衝區的工作大小
        proxy_temp_file_write_size 64k;                                 # 設置代理臨時文件的寫入大小
  }  

}
```
- 在主設定檔自動載入配置檔案

![nginx with http socket io api conf should alter nginx main conf.png](http://192.168.25.60:8000/files/file_storage/8da7ff63.png)

```shell
sudo nano /etc/nginx/nginx.conf
```

- 在 http 設定區塊：新增 `include /etc/nginx/conf.d/*.conf;`

```
http {
    # 其他設定...
    include /etc/nginx/conf.d/*.conf;
}
```

- 測試 Nginx 檔案中沒有語法錯誤

```shell
sudo nginx -t
```

- 驗證成功

```
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

- 如果沒有出現任何問題，重新啟動 Nginx 以使變更生效

```shell
sudo systemctl restart nginx
```

- 使用以下命令檢查目標地址上的端口是否正在運行
```
sudo lsof -i :10000
```

```
COMMAND   PID USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
nginx   20909 root    6u  IPv4 154702      0t0  TCP *:webmin (LISTEN)
nginx   20910 root    6u  IPv4 154702      0t0  TCP *:webmin (LISTEN)
```

## 檢查網絡連接是否正常
- 使用 ping 命令檢查連接：在終端機中輸入以下命令以檢查是否可以與該 IP 位址進行連接

```shell
ping 192.168.25.180
```

```
PING 192.168.25.180 (192.168.25.180) 56(84) bytes of data.
64 bytes from 192.168.25.180: icmp_seq=1 ttl=127 time=0.846 ms
64 bytes from 192.168.25.180: icmp_seq=2 ttl=127 time=0.921 ms
```

- 使用 telnet 命令檢查端口連接：在終端機中輸入以下命令以檢查是否可以與指定的 IP 位址和端口進行連接

```shell
telnet 192.168.25.180 10000
```

```
Trying 192.168.25.180...
Connected to 192.168.25.180.
Escape character is '^]'.
```

## 檢查防火牆設定
- 須設定防火牆讓 port 10000 允許訪問
- 可以使用以下兩種正在使用的防火牆軟體檢查
### 使用 iptables
```shell
sudo iptables -L
```

- 檢查是否允許通過目標主機上的 10000 端口的傳入連接
```shell
sudo iptables -L INPUT -n -v --line-numbers | grep "10000"
```

- 如果顯示類似於以下的輸出，表示允許通過 10000 端口的傳入連接

```
num  pkts bytes target     prot opt in     out     source               destination         
XX    XX    XX    ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0            tcp dpt:10000
```

- 如果未顯示任何輸出或顯示其他規則，則可能需要添加一個允許通過 10000 端口的規則。可以使用以下命令添加一個規則：
```shell
sudo iptables -A INPUT -p tcp --dport 10000 -j ACCEPT
```

### 使用 ufw
- 可以使用以下命令檢查和添加規則

```
sudo ufw allow 10000
```

```shell
sudo ufw status
```

```
Status: active

To                         Action      From
--                         ------      ----
OpenSSH                    ALLOW       Anywhere
Nginx HTTP                 ALLOW       Anywhere
10000                      ALLOW       Anywhere
OpenSSH (v6)               ALLOW       Anywhere (v6)
Nginx HTTP (v6)            ALLOW       Anywhere (v6)
10000 (v6)                 ALLOW       Anywhere (v6)
```

## 使用 postman 測試
- 建立一個 socket io test
- 設定 request url 為代理的 url：`http://192.168.25.180:10000`
- 設定 settings 中的 Handshake path：`/socket/socket.io` (原本預設為 `/socket.io`)
- 設定 Events 監聽事件：`echo`
- 設定 Messsge 事件名稱：`echo`

![nginx with http socket io api postman test handshake path setting.png](http://192.168.25.60:8000/files/file_storage/67851cf7.png)

- 測試可成功透過 nginx 代理拿到資料

![nginx with http socket io api postman test successfully.png](http://192.168.25.60:8000/files/file_storage/d3d5f903.png)

## 使用 socket io client 測試
- 使用套件 `socket.io-client`
- 設定 request url 為代理的 url：`http://192.168.25.180:10000`
- 設定 connect path 為：`/socket/socket.io`

```js
const { io } = require('socket.io-client');
const URL = 'http://192.168.25.180:10000';
const createClient = () => {
  const transports = ['websocket'];
  const socket = io.connect(URL, {
    'reconnection delay': 0,
    'reopen delay': 0,
    'force new connection': true,
    // secure: true,
    reconnect: true,
    rejectUnauthorized: false,
    transports,
    forceNew: true,
    path: '/socket/socket.io',
  });
};
```

- 測試可成功透過 nginx 代理拿到資料

![nginx with http socket io api socket io client test successfully.png](http://192.168.25.60:8000/files/file_storage/e517cd2b.png)








