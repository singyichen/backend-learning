---
title: SSL
description: 
published: true
date: 2024-01-26T00:15:10.501Z
tags: ssl, 憑證
editor: markdown
dateCreated: 2022-08-08T00:52:01.350Z
---

# SSL ( Secure Sockets Layer )
- [ ] [什麼是 SSL、TLS 以及 HTTPS？](https://www.websecurity.digicert.com/zh/tw/security-topics/what-is-ssl-tls-https)
- [ ] [SSL憑證是什麼？有哪些種類？](https://www.net-chinese.com.tw/nc/index.php/MenuLink/Index/SSLCertificates#ssl_introduction)
- [ ] [使用 OpenSSL 製作萬用字元 SSL 憑證](https://blog.darkthread.net/blog/issue-wildcard-ssl-cert-with-openssl/)
- [ ] [如何使用 OpenSSL 建立開發測試用途的自簽憑證 (Self-Signed Certificate)
](https://blog.miniasp.com/post/2019/02/25/Creating-Self-signed-Certificate-using-OpenSSL)
- [ ] [HTTP與HTTPS的差異，哪一種對對網站比較好呢?](https://www.sun-exp.com/blog01?id=20)
- [ ] [SSL 與 TLS：您需要知道的一切](https://nordvpn.com/zh-tw/blog/ssl-tls-bijiao/)
- [ ] [[week 4] 網路基礎概論 - HTTP 協定、TCP/IP、API](https://hackmd.io/@Heidi-Liu/note-net101)
- [ ] [HTTP 教學](https://notfalse.net/http-series)
- [ ] [用Nginx架設local端https應用](https://hackmd.io/@warrenpig/create-self-signed-https-nginx-app)
- [ ] [如何使用 OpenSSL 建立開發測試用途的自簽憑證 (Self-Signed Certificate)](https://blog.miniasp.com/post/2019/02/25/Creating-Self-signed-Certificate-using-OpenSSL)
- [ ] [如何使用 PowerShell 建立開發測試用途的自簽憑證 (Self-Signed Certificate)](https://blog.miniasp.com/post/2018/04/24/Using-PowerShell-to-build-Self-Signed-Certificate)
- [ ] [使用證書（Certificates）](https://postman.xiniushu.com/docs/sending-requests/certificates)
- [ ] [什麼是 SSL？ | SSL 定義](https://www.cloudflare.com/zh-tw/learning/ssl/what-is-ssl/)
- [ ] [Linux 自簽名 CA 本地主機 IP，安全 SSL 證書 for HTTPS](https://footmark.com.tw/news/linux/ca-localip-ssl-https/)

# HTTP 與 HTTPS

[02從HTTP協定開始講起](/軟體開發/學習心得/11542/Basic/WebDev/網路基礎概論/02從HTTP協定開始講起)

# SSL 
## 什麼是 SSL 憑證？
SSL 的全名是 Secure Sockets Layer，即安全通訊端層，簡而言之，是一種資訊傳輸的加密技術，用於保持網際網路連線安全以及防止在兩個系統之間發送的所有敏感資料被罪犯讀取及修改任何傳輸的資訊，包括潛在的個人詳細資料。兩個系統可以是伺服器與用戶端 (例如購物網站與瀏覽器)，或者伺服器至伺服器 (例如，含有個人身份資訊或含有薪資資訊的應用程式)。 

這樣做是為了確保使用者與網站、或兩個系統之間傳輸的任何資料保持無法被讀取的狀態。此技術可使用加密演算法以混淆輸送中的資料，防止駭客在資料透過連線發送時讀取資料。此資訊可能是任何敏感或個人資訊，包括信用卡號與其他財務資訊、姓名與地址。

TSL ( Transport Layer Security ，傳輸層安全性 ) 是更新、更安全的 SSL 版本。我們仍將安全性憑證稱為 SSL，因為這是較常用的詞彙

HTTPS ( Hyper Text Transfer Protocol Secure ，超級文字傳輸協議安全) 會在網站受到 SSL 憑證保護時在網址中出現。該憑證的詳細資料包括發行機構與網站擁有人的企業名稱，可以透過按一下瀏覽器列上的鎖定標記進行檢視。

經過 SSL 憑證加密成功的網站，網址會由 http 變成 https。如下圖所示，第一個網站因為有安裝 SSL，在網址前面有「鎖頭」標記；第二個網站因沒有安裝 SSL ，網址前面則顯示「不安全」。

![SSL https example.png](http://192.168.25.60:8000/files/file_storage/51b2ef69.png)

![SSL http example.png](http://192.168.25.60:8000/files/file_storage/43071915.png)

![SSL http-vs-https.png](http://192.168.25.60:8000/files/file_storage/e06375ef.png)

![SSL http and htpps.png](http://192.168.25.60:8000/files/file_storage/88b71812.png)


## SSL 憑證是如何運作的？
SSL / TLS 加密可分為兩個階段：SSL / TLS 握手協議（SSL handshake）和 SSL / TLS 紀錄層，以下我們會詳細說明。

基本原則是，當您在伺服器與連線至伺服器的瀏覽器上安裝 SSL 憑證時，SSL 憑證的存在會觸發 SLL (或 TLS ) 協議，這將加密伺服器與瀏覽器之間 (或伺服器之間) 發送的資訊；詳細資料顯然更加複雜一些。

SSL 直接在傳輸控制協議之上 ( TCP ) 運作，像安全感毛毯一樣有效。其允許最高的協議層不變，同時仍然提供安全連線。所以在 SSL 層下，其他協議層可以照常運作。

若 SSL 憑證使用正確，所有攻擊者將看到哪個 IP 與通訊埠已連線以及大概在發送多少資料。他們或許可以終止連線，但是伺服器與使用者可以知道這是由第三方造成的。但是，他們無法攔截任何資料，所以使攻擊從根本上變得無效。

駭客或許能夠瞭解使用者正在連線至哪個主機，但最重要的是，無法瞭解詳細的網址。因為連線被加密，重要的資料仍將受到保護。

![SSL concept.png](http://192.168.25.60:8000/files/file_storage/2d4c9644.png)

1. SSL 在 TCP 連線建立後開始運作，啟動 SSL 信號交換。
2. 伺服器發送憑證至使用者，同時附上大量規格 (包括哪個版本的 SSL / TLS 以及使用哪個加密方法等。)
3. 然後使用者檢查憑證的有效性，並選擇雙方都支援的最高等級的加密，並使用這些方法啟動安全的工作階段。有很多組方法可用，優勢各異 ─ 它們被稱為密碼組。
4. 為了確保所有傳輸訊息的完整性與真實性，SSL 與 TLS 協議也包括使用訊息驗證程式碼的驗證程序。所有這些方法聽上去冗長又複雜，但在現實生活中是瞬間實現的。


## SSL 握手協議
SSL 握手是客戶端和伺服器之間的一種通訊形式。在這種通訊形式中，客戶端和伺服器會決定如何進行進一步的通訊。以下是 SSL 握手的方式：

1. 客戶端發出「hello」請求，並將客戶端支援的 SSL 版本和加密演算法發送給網頁伺服器。
2. 伺服器收到客戶端的「hello」請求，然後伺服器回傳「hello」到客戶端，並傳送 SSL 證書和公鑰，這裡的客戶端和伺服器用非對稱加密來交換安全資訊。這表示客戶端需要伺服器的公鑰來加密訊息，而伺服器需要公鑰和私鑰來解密訊息。任何窺探流量的人都無法破解傳輸的加密訊息。
3. 然後，客戶端使用伺服器的公鑰建造 pre-master secret，這是一個亂數，並將其發送到伺服器，以建造對話金鑰（session key），這會將通訊提升為對稱加密。目前這兩端只使用私鑰，對稱加密讓通訊速度更快，而且使用更少的資源。
4. 伺服器解密 pre-master secret，使用它建立金鑰並與客戶端交換。藉由對稱加密的建立，雙方現在可以交換加密訊息，網站的傳輸是安全的。

![SSL 握手是什麼.png](http://192.168.25.60:8000/files/file_storage/83547b63.png)

## SSL/TLS 紀錄層

這個階段會傳輸加密資料，數據從用戶應用程式發送並加密。根據加密演算法的不同，數據可能會被壓縮。然後加密訊息會被發送到網路傳輸層，該層會決定如何將數據發送到目標設備。

## SSL 憑證有哪些類型？
有幾種不同類型的 SSL 憑證。一個憑證可以應用於一個或多個網站，具體取決於類型：
- 單一網域：單一網域 SSL 憑證僅適用於一個網域 (「網域」是網站的名稱，例如 www.cloudflare.com)。
- 萬用字元：與單一網域憑證一樣，萬用字元 SSL 憑證僅適用於一個網域。但是，這也適用於該網域的子網域。例如，萬用字元憑證可以涵蓋 www.cloudflare.com、blog.cloudflare.com，和 developers.cloudflare.com，而單一網域憑證只適用於第一個。
- 多網域：顧名思義，多網域 SSL 憑證可以應用於多個不相關的網域。

SSL 憑證還具有不同的驗證級別。驗證級別就像背景檢查一樣，並且級別會根據檢查的徹底性而變化。
- 網域驗證：這是最嚴格的驗證級別，也是最便宜的級別。企業只需要證明他們控制著網域。
- 組織驗證：這是一個需要親手操作的過程：CA 直接聯繫請求憑證的人員或企業。這些憑證更受使用者信賴。
- 擴展驗證：在發出 SSL 憑證之前，需要對組織進行全面的後台檢查。

![generate ssl flow.png](http://192.168.25.60:8000/files/file_storage/b50eb706.png)

# 在 Windows 主機使用 OpenSSL 建立 CA 憑證並簽發萬用字元憑證
```bash
cd /d D:
```
## 1. 建置一個資料夾放置產生的憑證
```bash
D:\mandy\Project\SSL
```

## 2. 產生根憑證 rootCA.key，過程需設定密碼，稍後會用到
```bash
openssl genrsa -des3 -out rootCA.key 4096
```
```
λ openssl genrsa -des3 -out rootCA.key 4096
Generating RSA private key, 4096 bit long modulus (2 primes)
.....................................................................++++
.............................................................++++
e is 65537 (0x010001)
Enter pass phrase for rootCA.key: 輸入密碼(在此設定 qaz123)
Verifying - Enter pass phrase for rootCA.key: 確認密碼(在此設定 qaz123)
```

![SSL 產生 rootCA key.png](http://192.168.25.60:8000/files/file_storage/d82ac189.png)

- 產生 rootCA.key

## 3. 產生 CA 憑證 rootCA.crt
```bash
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 36500 -out rootCA.crt
```
```
Enter pass phrase for rootCA.key: 輸入密碼(在此設定 qaz123)
You are about to be asked to enter information that will be incorporated
into your certificate request.
What you are about to enter is what is called a Distinguished Name or a DN.
There are quite a few fields but you can leave some blank
For some fields there will be a default value,
If you enter '.', the field will be left blank.
-----
Country Name (2 letter code) [AU]:TW
State or Province Name (full name) [Some-State]:Sanmin
Locality Name (eg, city) []:Kaohsiung
Organization Name (eg, company) [Internet Widgits Pty Ltd]:hokia
Organizational Unit Name (eg, section) []:IT
Common Name (e.g. server FQDN or YOUR name) []:192.168.25.180
Email Address []:mandy.chen@hokia.com.tw
```

![SSL 產生 CA 憑證 rootCA crt.png](http://192.168.25.60:8000/files/file_storage/1f6b9507.png)

- 產生 rootCA

## 4. 決定萬用字元憑證的域名
在這裡以自己電腦 IP 為域名：192.168.25.180
或公司網域：*.hokia.com.tw

## 5. 產生萬用字元的憑證金鑰
```bash
openssl genrsa -out 192.168.25.180.key 2048
```
```
Generating RSA private key, 2048 bit long modulus (2 primes)
..........................................+++++
.................................+++++
e is 65537 (0x010001)
```

![SSL 產生萬用字元的憑證金鑰.png](http://192.168.25.60:8000/files/file_storage/bfb9ee4f.png)

- 產生 192.168.25.180.key

## 6. 準備憑證要求的設定檔
建立 192.168.25.180.cnf
```bash
touch 192.168.25.180.cnf
```
新增設定如下
```
[req]
default_bits = 2048
prompt = no
default_md = sha256
distinguished_name = dn

[dn]
C=TW
ST=Sanmin
L=Kaohsiung
O=hokia
OU=IT
emailAddress=mandy.chen@hokia.com.tw
CN=192.168.25.180
```
## 7. 建立憑證請求 
建立 192.168.25.180.csr
```bash
openssl req -new -sha256 -nodes -key 192.168.25.180.key -out 192.168.25.180.csr -config 192.168.25.180.cnf
```

![SSL 建立憑證請求.png](http://192.168.25.60:8000/files/file_storage/4d923927.png)

- 產生 192.168.25.180.csr

## 8. 準備 V3 設定檔
建立 192.168.25.180-v3.ext
```bash
touch 192.168.25.180-v3.ext
```
新增設定如下
```
authorityKeyIdentifier=keyid,issuer
basicConstraints=CA:FALSE
keyUsage = digitalSignature, nonRepudiation, keyEncipherment, dataEncipherment
subjectAltName = @alt_names

[alt_names]
DNS.1 = 192.168.25.180
```

## 9. 建立憑證 
建立憑證 192.168.25.180.crt，過程需要使用步驟 2 設定的密碼
```bash
openssl x509 -req -in 192.168.25.180.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out 192.168.25.180.crt -days 36500 -sha256 -extfile 192.168.25.180-v3.ext
```
```
Signature ok
subject=C = TW, ST = Sanmin, L = Kaohsiung, O = hokia, OU = IT, emailAddress = mandy.chen@hokia.com.tw, CN = 192.168.25.180
Getting CA Private Key
Enter pass phrase for rootCA.key:輸入密碼(在此設定 qaz123)
```

![SSL 建立憑證.png](http://192.168.25.60:8000/files/file_storage/4e990ac8.png)

## 10. 要使用於 IIS，需將 .crt 轉為包含私鑰的 .pfx 格式
```bash
openssl pkcs12 -export -out 192.168.25.180.pfx -inkey 192.168.25.180.key -in 192.168.25.180.crt -certfile rootCA.crt
```

執行後得到 192.168.25.180.pfx，過程需要設定一組密碼，稍後在 IIS 安裝憑證時會用到。

```
Enter Export Password:輸入密碼(在此設定 qaz123)
Verifying - Enter Export Password:確認密碼(在此設定 qaz123)
```

![SSL 要使用於 IIS，需將 crt 轉為包含私鑰的 pfx 格式.png](http://192.168.25.60:8000/files/file_storage/a968e8bf.png)

## 11. SSL 檔案

![SSL 檔案.png](http://192.168.25.60:8000/files/file_storage/5274c293.png)

## 12. 於程式中設定 fastify.cert、fastify.key
- fastify.cert：192.168.25.180.crt
- fastify.key：192.168.25.180.key
```js
const { readFileSync } = require('fs');
const path = require('path');
  const app = fastify({
    // 使用 logger plugin
    logger: fastifyLoggerPlugin,
    https: {
      key: readFileSync(path.join(__dirname, 'src/ssl', '192.168.25.180.key')),
      cert: readFileSync(path.join(__dirname, 'src/ssl', '192.168.25.180.crt')),
    },
  });
```
## 13. 使用 Chrome 實測

https://192.168.25.180:10001/swagger/WebSignin_API/static/index.html

> 若想共用該 CA 憑證簽發其他伺服器憑證，記得妥善保存相關檔案及密碼
{.is-warning}

> 以下流程為 IIS 部分可忽略
{.is-info}

## 14. 匯入自簽憑證到「受信任的根憑證授權單位」
請以「**系統管理員身分**」執行以下命令，即可將憑證匯入到 Windows 的憑證儲存區之中：
```bash
// D:\mandy\Project\SSL
certutil -addstore -f "ROOT" 192.168.25.180.crt
```
```
ROOT "受信任的根憑證授權單位"
相關的憑證:

完全相符:
元素 10:
序號: 43e6c4b50fdc28f1410b045cb4a0312dcf93563a
簽發者: E=mandy.chen@hokia.com.tw, CN=192.168.25.180, OU=IT, O=hokia, L=Kaohsiung, S=Sanmin, C=TW
 NotBefore: 2024/1/24 下午 03:22
 NotAfter: 2123/12/31 下午 03:22
主體: CN=192.168.25.180, E=mandy.chen@hokia.com.tw, OU=IT, O=hokia, L=Kaohsiung, S=Sanmin, C=TW
不是根憑證
Cert 雜湊(sha1): 460135c3398cca4b914b2c2b8a77a66e699f3048

憑證 "192.168.25.180" 已在存放區中。
CertUtil: -addstore 命令成功完成。
```

![SSL 匯入自簽憑證到受信任的根憑證授權單位.png](http://192.168.25.60:8000/files/file_storage/5fff7115.png)

## 15. 開啟 IIS 管理介面，找到「伺服器憑證」，使用匯入功能匯入 192.168.25.180-net.pfx

### 開啟 IIS 管理介面
https://blog.xuite.net/yasin/scc007/366461513-Windows+10+%E9%96%8B%E9%97%9C+IIS
https://dotblogs.com.tw/jackder_soho/2016/04/22/013952

![SSL IIS.png](http://192.168.25.60:8000/files/file_storage/71ea70f4.png)

### 選取伺服器憑證

![SSL IIS 伺服器憑證.png](http://192.168.25.60:8000/files/file_storage/4b8d5aea.png)

### 匯入憑證
- 憑證檔案：192.168.25.180-net.pfx
- 密碼：qaz123

![SSL IIS 伺服器憑證匯入.png](http://192.168.25.60:8000/files/file_storage/b9d1af3c.png)

## 16. 設定 HTTPS 繫結時就可以選取新做好的憑證
- 類型：https
- IP 位址：192.168.25.180
- SSL 憑證：192.168.25.180-net

![SSL IIS 設定 HTTPS 繫結選取新作好的憑證.png](http://192.168.25.60:8000/files/file_storage/a13634e3.png)





