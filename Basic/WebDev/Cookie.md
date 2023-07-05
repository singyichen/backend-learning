---
title: Cookie
description: 
published: true
date: 2023-05-17T03:58:27.126Z
tags: cookie
editor: markdown
dateCreated: 2023-01-10T00:39:31.057Z
---

# Cookie
- [ ] [cookie, session 與 jwt-token](https://medium.com/@paulyang1234/cookie-session-%E8%88%87-jwt-token-%E5%AE%89%E5%85%A8%E6%80%A7%E5%95%8F%E9%A1%8C-8945a8a579ac)
- [ ] [傻傻分不清之 Cookie、Session、Token、JWT](https://juejin.cn/post/6844904034181070861)
- [ ] [Cookie、Session、Token、JWT 看一篇就夠了](https://blog.51cto.com/u_15300443/3167467)
- [ ] [[教學] Cookie 是什麼？如何用 JavaScript get/set document.cookie?](https://shubo.io/cookies/)
- [ ] [前端三十｜27. [WEB] Cookie & Session 是什麼？](https://medium.com/schaoss-blog/%E5%89%8D%E7%AB%AF%E4%B8%89%E5%8D%81-27-web-cookie-session-%E6%98%AF%E4%BB%80%E9%BA%BC-83f9747caf23)
- [ ] [5.2 餅乾(cookie)出賣了我們的隱私, 但你可以這樣做...](https://docs.f5ezcode.in/cs-basic/di-wu-zhang-it-ni-gen-mo-sheng-ren-zhi-zhi-ge-liu-ren/5.2-qian-cookies-chu-le-wo-de-si-dan-ni-ke-yi-zuo-...)

# 網站很懂我耶！
常聽到有人說：「我搜尋了一次牛仔褲後，我所有的廣告都變成牛仔褲，好像大家都知道我想買牛仔褲一樣」，到底為什麼網站會記得我們的喜好呢？這一切可都是 Cookie 透漏的！

![cookie concept.png](http://192.168.25.60:8000/files/file_storage/ec82b774.jpg)

# Cookie 是什麼
> - Cookie又稱為「小甜餅」，他是一個「小型文字檔案」。儲存在使用者瀏覽器中，使瀏覽器能記下一些資訊，以便未來的使用。而根據儲存位置，可分為記憶體Cookie和硬碟Cookie。
> - 儲存於用戶本機端的數據，放置於瀏覽器某資料夾下，利用 `key-value` 去存取

- 記憶體 Cookie：由瀏覽器維護，儲存在記憶體中，當瀏覽器被關閉時就消失了。存在時間是很短暫的，為非持久Cookie。
- 硬碟 Cookie：儲存在硬碟裡，有一個過期時間。若不是用戶手動清理或到了過期時間，硬碟Cookie不會被刪除。存在時間是長期的，為持久Cookie。
- HTTP 是無狀態的協議（對於事務處理沒有記憶能力，每次客戶端和伺服器端會話完成時，伺服器端不會保存任何會話資訊）：每個請求都是完全獨立的，伺服器端無法確認當前訪問者的身份資訊，無法分辨上一次的請求傳送者和這一次的傳送者是不是同一個人。所以伺服器與瀏覽器為了進行會話跟蹤（知道是誰在訪問我），就必須主動的去維護一個狀態，這個狀態用於告知伺服器端前後兩個請求是否來自同一瀏覽器。而這個狀態需要通過 cookie 或者 session 去實現。
- cookie 儲存在客戶端： cookie 是伺服器傳送到使用者瀏覽器並保存在本地的一小塊資料，它會在瀏覽器下次向同一伺服器再發起請求時被攜帶並行送到伺服器上。
- cookie 是不可跨域的： 每個 cookie 都會繫結單一的域名，無法在別的域名下獲取使用，一級域名和二級域名之間是允許共享使用的（靠的是 domain）。

# Cookie 的使用
因為HTTP協定是無狀態性的，他不會記住你上一次操作，所以我們需要透過Cookie來取得這些信息，常見的Cookie使用如下：

- 網路購物：當你在商品選購頁面點選了一個餅乾後，想再買一罐飲料，此時瀏覽器會在傳送網頁時傳送一段Cookie給伺服器，讓伺服器知道你之前買了餅乾，並在買飲料後，在原本的Cookie追加新的商品資訊。最後結帳時，伺服器讀取Cookie就能得知所有要購買的東西了！
- 自動登入：當你打了一次帳號密碼後，可以勾選「下次自動登入」，下次打開同一個網站時，會發現你沒輸入帳號密碼就已經登入。這是因為第一次登入時伺服器傳送了其Cookie到用戶的硬碟上，如果第二次登入時，你的Cookie還沒到期的話，瀏覽器就會傳送這個帳號密碼的Cookie，就不必再輸入了。
- 廣告投放：廣告商（第三方網站）會在很多大型網站上刊登廣告，而在這些網站上，你連上的每一頁網址都會被Cookie記錄下來，為第三方Cookie。所以如果把各大網站得到的資料綜合起來，追蹤你的網站使用行為，就不難投放出為你量身打造的廣告！

> 第三方網站是什麼？
第一方網站為你目前正在瀏覽的網站，
第三方網站則是第一方網站上的其他網站廣告（如：快顯視窗或橫幅廣告）
{.is-info}


# Cookie 重要的屬性
## name=value
> 鍵值對，設定 Cookie 的名稱及相對應的值，都必須是字串類型

- 如果值為 Unicode 字元，需要為字元編碼。
- 如果值為二進制資料，則需要使用 BASE64 編碼。

## domain
> 指定 cookie 所屬域名，默認是當前域名

## path
> 指定 cookie 在哪個路徑（路由）下生效，默認是 `'/'`。

如果設定為 /abc，則只有 /abc 下的路由可以訪問到該 cookie，如：/abc/read。

## maxAge
> cookie 失效的時間，單位秒。

- 如果為整數，則該 cookie 在 maxAge 秒後失效。
- 如果為負數，該 cookie 為臨時 cookie ，關閉瀏覽器即失效，瀏覽器也不會以任何形式保存該 cookie 。
- 如果為 0，表示刪除該 cookie 。默認為 -1。
- 比 expires 好用。

## expires
> 過期時間，在設定的某個時間點後該 cookie 就會失效。

一般瀏覽器的 cookie 都是默認儲存的，當關閉瀏覽器結束這個會話的時候，這個 cookie 也就會被刪除

## secure
> 該 cookie 是否僅被使用安全協議傳輸。安全協議有 HTTPS，SSL 等，在網路上傳輸資料之前先將資料加密。默認為 false 。

當 secure 值為 true 時，cookie 在 HTTP 中是無效，在 HTTPS 中才有效。

## httpOnly
> 如果給某個 cookie 設定了 httpOnly 屬性，則無法通過 JS 指令碼 讀取到該 cookie 的資訊，但還是能通過 Application 中手動修改 cookie，所以只是在一定程度上可以防止 XSS 攻擊，不是絕對的安全


# Cookie 的安全性
聽到這邊，你可能會認為Cookie也太危險了吧！其實在大多數的情況下，Cookie沒那麼危險，它能記載的資料有限，網站不可能經由 Cookie 獲得你的 email 地址或是其他私人資料，更沒有辦法透過 Cookie 來存取你的電腦。不過遇到惡意的攻擊時，這些資料可能就無法倖免。那要如何防範Cookie被盜取呢？
## 清除 Cookie
當要離開一台公共電腦時，你可以清除Cookie，不讓電腦的擁有者或有心人士竊取你使用的軌跡。以 Windows 系統的Chrome 瀏覽器為例，清除Cookie的方式為：
1. 點擊瀏覽器右上角「更多」的圖示（為直的三個圓點）
2. 點選「設定」中的「進階」
3. 選擇「隱私權與安全性」
4. 在「清除瀏覽資料」中清除Cookie

![清除 Cookie.png](http://192.168.25.60:8000/files/file_storage/ab966e2a.png)

## 關閉 Cookie
如果你想要更加安全，你可以選擇關閉Cookie，這麼一來瀏覽器便不會記錄。
以Windows 系統的Chrome瀏覽器為例，關閉Cookie的方式為：
1. 點擊右上角的「更多」圖示（為直的三個圓點）。
2. 點選「設定」中的「進階」
3. 選擇「隱私權與安全性」中的「內容設定」
4. 按一下 「Cookie」，並將其關閉

![關閉 Cookie.png](http://192.168.25.60:8000/files/file_storage/69d16a1c.png)

# 使用 Cookie 時需要考慮的問題
- 因為儲存在客戶端，容易被客戶端篡改，使用前需要驗證合法性
- 不要儲存敏感資料，比如使用者密碼，帳戶餘額
- 使用 httpOnly 在一定程度上提高安全性
- 儘量減少 cookie 的體積，能儲存的資料量不能超過 4kb
- 設定正確的 domain 和 path，減少資料傳輸
- cookie 無法跨域
- 一個瀏覽器針對一個網站最多存 20 個Cookie，瀏覽器一般只允許存放 300 個Cookie
- 移動端對 cookie 的支援不是很好，而 session 需要基於 cookie 實現，所以移動端常用的是 token