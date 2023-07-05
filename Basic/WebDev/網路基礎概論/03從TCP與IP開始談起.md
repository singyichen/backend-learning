---
title: 03.從TCP與IP開始談起
description: 
published: true
date: 2023-06-26T01:18:05.257Z
tags: tcp, ip
editor: markdown
dateCreated: 2022-11-15T06:18:30.805Z
---

# 03.從TCP與IP開始談起
- [ ] [[第六週] 網路基礎 — TCP/ IP](https://miahsuwork.medium.com/%E7%AC%AC%E5%85%AD%E9%80%B1-%E7%B6%B2%E8%B7%AF%E5%9F%BA%E7%A4%8E-tcp-ip-f89cc09f1f36)
- [ ] [TCP/IP vs OSI，網際網路中的協議模型](https://ithelp.ithome.com.tw/articles/10266168)
- [ ] [TCP/IP，網際網路的基礎通訊架構](https://ithelp.ithome.com.tw/articles/10267704)
- [ ] [OSI 模型，教科書中的框架](https://ithelp.ithome.com.tw/articles/10268407)
- [ ] [什麼是OSI的7層架構？和常聽到的Layer 7有關？](https://ithelp.ithome.com.tw/articles/10000021)
- [ ] [TCP vs. UDP: 7 Differences You Should Know](https://blog.bytebytego.com/p/ep54-cache-systems-every-developer?utm_source=profile&utm_medium=reader2)
- [ ] [Network Protocols Run the Internet](https://blog.bytebytego.com/p/network-protocols-run-the-internet?utm_source=profile&utm_medium=reader2)
- [ ] [Everything You Always Wanted to Know About TCP But Too Afraid to Ask](https://blog.bytebytego.com/p/everything-you-always-wanted-to-know?utm_source=profile&utm_medium=reader2)

# TCP ( Transmission Control Protocol ) 傳輸控制協議
## TCP/IP 四層架構
> - Application Layer（應用層）
> - Transport Layer（傳輸層）
> - Internet Layer（網際網路層、網路層）
> - Network Access Layer（網路存取層）

![TCPIP Encapsulation.png](http://192.168.25.60:8000/files/file_storage/8c279c95.png)

## 網路的層級 
Internet 中要經過非常多道手續才能順利將資訊傳遞到另一端，因此就有組織將手續整理後明確分層變成一個模型，分層的好處就是只要處理那個層級的事情就好，可以把每一個層級想像成是一個「關卡」，傳遞的資訊時必須層層破關（通過協定），才能將資訊傳遞到另一端。

最有名的模型有兩種：TCP/IP 通訊協定、OSI 七層模型

### TCP/IP 通訊協定
TCP/IP Protocol Suite
目前為網際網路的基礎通訊架構。＃戰勝其他一些網路協定的方案像是 OSI 模型。

目前主流的模型為 「TCP/IP 四層模型」，作為互連網路的標準框架：
當從瀏覽器發送網路請求（Request）到 Internet 後，會從「應用層」到達「實體層」傳送出去，而當另一端收到請求後，再反向經由「實體層」到達「應用層」解讀訊息。

假設你用 Line 傳了一則訊息出去，應用層的 Line 會把你的訊息丟給傳輸層的程式，加上「連接埠」等資訊傳到下面網際網路層的程式。在這層，資料會被加上「IP 位置」等資訊，再被傳到下面的網路存取層，加上「MAC 位置」等等，最後被轉換成訊號送走。

![TCPIP各層處理的資料.png](http://192.168.25.60:8000/files/file_storage/0ac088cd.png)

### OSI 七層模型
Open System Interconnection Model；中文：開放式系統互聯模型。
國際標準化組織（ISO）制定。

OSI 的模型共有以下七層

- Application Layer（應用層）

應用層是最接近終端用戶的層級。大多數應用程序都位於此層。我們可以在不需要了解數據傳輸細節的情況下從後端服務器請求數據。此層中的協議包括HTTP、SMTP、FTP、DNS等，我們稍後會進行介紹。

- Presentation Layer（表示層）

此層處理數據編碼、加密和壓縮，為應用層準備數據。例如，HTTPS利用TLS（傳輸層安全）實現客戶端和服務器之間的安全通信。

- Session Layer（會話層）

此層打開和關閉兩個設備之間的通信。如果數據大小較大，會話層會設置檢查點以避免從頭重新發送。

- Transport Layer（傳輸層）

此層處理兩個設備之間的端到端通信。發送方將數據分段，接收方在接收時重新組裝。此層中有流量控制，以防止擁塞。此層中的關鍵協議是TCP和UDP，我們稍後會進行討論。

- Network Layer（網路層）

此層實現不同網絡之間的數據傳輸。它進一步將段或數據報拆分為較小的數據包，並使用IP地址找到最優路徑到達最終目的地。這個過程稱為路由。

- Data Link Layer（資料連結層）

此層允許在同一個網絡上的設備之間進行數據傳輸。數據包被拆分為幀，幀被限制在本地區域網絡內。

- Physical Layer（實體層）

此層通過電纜和交換機發送比特流，因此與設備之間的物理連接密切相關。

![OSI Model.png](http://192.168.25.60:8000/files/file_storage/e5525614.png)

![OSI及TCPIP架構映射.png](http://192.168.25.60:8000/files/file_storage/963ffc8e.png)

和 TCP/IP 模型不同的是，OSI 模型提出更全面和詳細的分層架構，若兩者做比較的話，可以說 OSI 的應用、表達及會議層算是 TCP/IP 的應用層，而資料連結層和實體層對應到 TCP/IP 的網路存取層。

其中表達層，規範數據的傳送與接收格式，例如使用 ASCII 還是 UTF-8 來編碼；會議層則規範一次會議（兩者間的連線）的機制，例如做身分認證、會議的重新連線等等。

至於實體層，規範了通訊設備間的通訊規格，例如電壓、傳輸媒介（線材、連接方式）等等。
