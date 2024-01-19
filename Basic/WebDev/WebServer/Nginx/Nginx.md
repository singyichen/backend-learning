---
title: Nginx
description: 
published: true
date: 2024-01-18T00:26:01.434Z
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
- [ ] [[Nginx] 設定](https://hackmd.io/@winnienotes/SymsGlMfj)

![forward proxy and a reverse proxy.png](http://192.168.25.60:8000/files/file_storage/afc85ebc.png)

# Nginx 介紹
> Nginx 是全球最受歡迎的網頁伺服器之一，負責託管互聯網上一些最大和最高流量的網站。它是一個輕量級的選擇，可用作網頁伺服器或反向代理。

![nginx logo.png](http://192.168.25.60:8000/files/file_storage/3421c13f.png)

Nginx 是一個非同步框架的網頁伺服器，可以做到

- 正向代理 (Forward Proxy)
- 反向代理 (Reverse Proxy)
- 負載均衡 (Load Balancing)
- http 快取 (HTTP Server)
- 郵件伺服器 (Mail Server)

Nginx 比起 Apache 屬於輕量級且高併發、處理靜態檔案的效率較高、耗費的記憶體較少、負載效能好，很適合做前端伺服器使用。

## 反向代理 Reverse Proxy
反向代理的好處在於能夠將 Client 不需知道 Application Server 的真實位置，僅需要透過 Nginx 反向代理的方式就能夠向後面的 Application Server 發送請求，而 Application Server 也不需要知道是哪一個 Client 的 Request，僅需回傳 Response 即可。(如下圖所示）

![Nginx 反向代理.png](http://192.168.25.60:8000/files/file_storage/8da5fc89.png)

## 負載均衡 Load Balance
為了因應大流量，一台 Application Server 是無法應付的，因此會需要同時開多個 Application Server 。而 Nginx 能夠自動的將 Client 的 Request 分送到不同 Application Server 上，而分送的演算法可以自己設計，最常使用的是 Round Robin 演算法，而其他的演算法也包含 Least Connections 、Least Time 、IP Hash 等。

![Nginx Load Balance 機制.png](http://192.168.25.60:8000/files/file_storage/1096fc9f.png)

## HTTP 快取 HTTP Server
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




