---
title: RESTful API
description: 
published: true
date: 2024-01-15T01:10:27.391Z
tags: api, restful
editor: markdown
dateCreated: 2022-11-22T02:59:49.680Z
---

# RESTful API
- [ ] [什麼是REST? 認識 RESTful API 路由語義化設計風格](https://tw.alphacamp.co/blog/rest-restful-api)
- [ ] [API概念與通訊協定](https://hackmd.io/@cynote/ByxHOWjBv)
- [ ] [API 到底是什麼？ 用白話文帶你認識](https://medium.com/codingbar/api-%E5%88%B0%E5%BA%95%E6%98%AF%E4%BB%80%E9%BA%BC-%E7%94%A8%E7%99%BD%E8%A9%B1%E6%96%87%E5%B8%B6%E4%BD%A0%E8%AA%8D%E8%AD%98-95f65a9cfc33)
- [ ] [API是什麼？淺談API概念與運用案例](https://www.tsg.com.tw/blog-detail4-120-0-api.htm)
- [ ] [什麼是 API（應用程式設計介面）？](https://www.tibco.com/zh-hant/reference-center/what-is-an-api-application-program-interface)
- [ ] [Why is RESTful API so popular?](https://blog.bytebytego.com/p/why-is-restful-api-so-popular?utm_source=profile&utm_medium=reader2)
- [ ] [What Is REST API? Examples And How To Use It: Crash Course System Design](https://www.youtube.com/watch?v=-mN3VyJuCjM&t=28s&ab_channel=ByteByteGo&loop=0)
- [ ] [Principles & Best practices of REST API Design](https://blog.devgenius.io/best-practice-and-cheat-sheet-for-rest-api-design-6a6e12dfa89f)
- [ ] [Design Effective and Secure REST APIs](https://blog.bytebytego.com/p/design-effective-and-secure-rest?utm_source=profile&utm_medium=reader2)
- [ ] [Top 7 Ways to 10x Your API Performance](https://www.youtube.com/watch?v=zvWKqUiovAM&ab_channel=ByteByteGo&loop=0)
- [ ] [EP85: Like LeetCode, but for improving debugging skills](https://blog.bytebytego.com/p/ep85-top-9-http-request-methods?utm_source=profile&utm_medium=reader2)
- [ ] [EP94: REST API Cheatsheet](https://blog.bytebytego.com/p/ep94-rest-api-cheatsheet?utm_source=profile&utm_medium=reader2)

# 什麼是 REST ?

![RESTful slogan.png](http://192.168.25.60:8000/files/file_storage/d639dc10.png)

![restful api.png](http://192.168.25.60:8000/files/file_storage/675bc30f.png)

REST 是 Representational State Transfer 的縮寫，可譯為「具象狀態傳輸」。由 Roy Fielding 博士在 2000 年的博士論文中所提出。他同時也是 HTTP 規範的主要作者之一。符合 REST 風格的網站架構可以稱為 RESTful。

REST 是一種軟體架構風格（並非標準），目的是幫助在世界各地不同軟體、程式在網際網路中能夠互相傳遞訊息。每一個網頁都可視為一個資源（resource）提供使用者使用，而你可以透過 URL（Uniform Resource Locator），也就是這些資源的地址，來取得這些資源並在你的瀏覽器上使用。

# 為什麼要認識 REST？
當 Roy Fielding 最一開始在打造人們今天所看到的網際網路的時候，其中最大的一個問題是：要如何跟世界各地的任何一台電腦溝通？因此有了 REST 的出現。然而這個問題在現今經常被忽略，甚至人們會覺得 REST 是一個新的概念，但其實在最初 HTTP 誕生的時候，REST 就也跟著問世了。電腦溝通的方法與規則其實很簡單，就像人們平時使用的文法一樣，在這裡電腦使用的是 HTTP 動詞（Verb），列舉如下：

- `GET`：讀取資源
- `PUT`：替換資源
- `PATCH`：更換資源部分內容
- `DELETE`：刪除資源
- `OPTIONS`：回傳該資源所支援的所有 HTTP 請求方法
- `CONNECT`：將連線請求轉換至 TCP/IP 隧道
- `POST`：新增資源

在透過 URL 對特定資源請求動作的時候，首先你要知道你的意圖。用日常生活的語言來說，URL 就好比是一個物品，譬如「杯子」，當你想要對這個「杯子」做點動作、而又必須請人幫忙的時候，你就得明確的說出想對那個杯子進行的動作，表明你的意圖，這樣他人才能順利幫你完成工作，像是「拿杯子」、或是「丟杯子」。在網際網路上也是一樣，只是這裡你將使用電腦可以理解的語言，像是 "GET" this URL 或是 "DELETE" this URL。

![透過 URL 對特定資源請求動作.png](http://192.168.25.60:8000/files/file_storage/a24fe545.png)


若想透過網際網路，跟其他電腦（伺服器）取得資訊，就得使用 HTTP 動詞 "GET"，若想要增加資訊，就使用 "POST"，若想要更新資訊就使用 "PUT"。到這裡，你已經了解到電腦是如何透過共通的語言，在網際網路上進行溝通。

1. **伺服器/客戶端分離（Client-Server）**	
	- 客戶端-伺服器結構限制的目的是將客戶端和伺服器端的關注點分離。將使用者介面所關注的邏輯和資料儲存所關注的邏輯分離開來有助於提高使用者介面的跨平台的可移植性。通過簡化伺服器模組也有助於伺服器模組的可延伸性。

2. **無狀態（Stateless）**
	- 伺服器不能儲存客戶端的資訊，每一次從客戶端傳送的請求中，要包含所有的必須的狀態資訊，對談資訊由客戶端儲存，伺服器端根據這些狀態資訊來處理請求。

	- 伺服器可以將對談狀態資訊傳遞給其他服務，比如資料庫服務，這樣可以保持一段時間的狀態資訊，從而實現認證功能。

	- 當客戶端可以切換到一個新狀態的時候傳送請求資訊。當一個或者多個請求被傳送之後，客戶端就處於一個狀態變遷過程中，每一個應用的狀態描述可以被客戶端用來初始化下一次的狀態變遷。

3. **分層（Layered System）**
	- 客戶端一般不知道是否直接連接到了最終的伺服器，或者是路徑上的中間伺服器。中間伺服器可以通過負載均衡和共享快取的機制提高系統的可延伸性,這樣可也便於安全策略的部署。

4. **可快取（Cacheable）**
	- 如同全球資訊網一樣，客戶端和中間的通訊傳遞者可以將回覆快取起來。回覆必須明確的或者間接的表明本身是否可以進行快取，這可以預防客戶端在將來進行請求的時候得到陳舊的或者不恰當的資料。管理良好的快取機制可以減少客戶端-伺服器之間的互動，甚至完全避免客戶端-伺服器互動，這進一步提了高效能和可延伸性。

5. **統一操作介面（Uniform Interface）**
這是 RESTful 系統設計的基本出發點。它簡化了系統架構，減少了耦合性，可以讓所有模組各自獨立的進行改進。包括下列四個限制：

	- (1) 請求中包含資源的 ID ( Resource identification in requests )
				請求中包含了各種獨立資源的標識，例如，在 Web 服務中的 URIs。資源本身和傳送給客戶端的標識是獨立。例如，伺服器可以將自身的資料庫資訊以 HTML XML 或者 JSON 的方式傳送給客戶端，但是這些可能都不是伺服器的內部記錄方式。

	- (2) 資源通過標識來操作 ( Resource manipulation through representations )
				當客戶端擁有一個資源的標識，包括附帶的元資料，則它就有足夠的資訊來刪除這個資源。

	- (3) 訊息的自我描述性 ( Self-descriptive messages )
				每一個訊息都包含足夠的資訊來描述如何來處理這個資訊。例如，媒體類型 ( midia-type ) 就可以確定需要什麼樣的剖析器來分析媒體資料。

	- (4) 用超媒體驅動應用狀態 ( Hypermedia as the engine of application state ( HATEOAS ) )
				同用戶存取 Web 伺服器的 Home 頁面相似，當一個 REST 客戶端存取了最初的 REST 應用的 URI 之後，REST 客戶端應該可以使用伺服器端提供的連結，動態的發現所有的可用的資源和可執行的操作。隨著存取的進行，伺服器在回應中提供文字超連結，以便客戶端可以得到當前可用的操作。客戶端無需用確定的編碼的方式記錄下伺服器端所提供的動態應用的結構資訊。

朝著這樣的理念所設計出來的系統可稱為 RESTful

## Router, Routing, Route
在談網路基礎建設時，路由器 ( router ) 是某種實體設備，負責幫每個資料封包 ( packet ) 選擇傳輸路徑，扮演類似交通指揮的角色。而「路由器幫資料封包們選擇路徑，引導資料按指定路徑移動」的這個過程，被稱為「routing」。

### Web 開發裡的路由
轉移到 web app 網路應用程式開發的領域，路由不再是實體設備，而是指 URL 的處理程序。這組處理程序會把 HTTP 動詞、URL、和相關的程式碼連接起來。

我們會在路由系統裡，定義「收到什麼 HTTP request，就執行什麼動作」。

舉例來說，如果今天收到一支 HTTP request，格式為 GET http://www.example.com/todos ，路由系統會調度出對應的資源，也就是和「瀏覽全部待辦事項」相關的樣板、邏輯、資料等等。

因為有了路由系統，網站對外開放的網址，不需要對應到實際的專案目錄。

### Express.js 與路由
Web 開發框架通常會內建路由系統，學習框架的第一件事，就是要學習如何使用框架提供的路由系統。

在以下的程式碼片段中，我們設定了一條路由：

```javascript
router.get('/about', function (req, res) {
 // do something
})
```

這條路由的意思是，如果收到符合 GET /about 格式的 request，就會執行函式內容。

## 如何設計路由？
我們會採用一套 REST 的架構風格來設計路由，RESTful 的設計以「資源」為中心，再搭配 HTTP method 的動詞，以及 CRUD 等資料操作：

![設計路由.png](http://192.168.25.60:8000/files/file_storage/02c77a25.png)

對資料的操作，不外乎：新增 ( create )、讀取 ( read )、更新 ( update )、刪除 ( delete ) 四種動作，通常會簡稱為 CRUD。

而 HTTP method 是有意義的動詞，如 `GET`、`POST`、`PATCH`、`DELETE` 等，因此 RESTful 的表意設計運用動詞的組合，來表達出對資源的操作方式。

例如我們看到 GET http://www.example.com/posts/1 ， 就會立刻可以理解他要取得 www.example.com 網站中，編號 1、名為 posts 的資源。

## 路由語義化：RESTful 風格應用
### 什麼是 RESTful
運用 HTTP 來表達語義的路由設計風格稱為 RESTful API。所謂的 API 是應用程式介面 ( application programming interface )，網址也是一種應用程式的「介面」，故稱為 API 。

RESTful 風格的網址設計強調**從路由結構就能看出要對什麼資料、進行什麼操作**。舉例來說，如果資料是 todos，那麼 RESTful 風格的 CRUD 路由就會這樣寫：

![RESTful 風格的 CRUD 路由.png](http://192.168.25.60:8000/files/file_storage/f6a2f8de.png)

從網址上，你會看出要操作的資源叫做 todos，這邊習慣用複數名詞。然後固定結構是：

- 瀏覽全部資料：GET + 資源名稱
- 瀏覽特定資料：GET + 資源名稱 + :id
- 新增一筆資料：POST + 資源名稱
- 修改特定資料：PUT + 資源名稱 + :id
- 刪除特定資料：DELETE + 資源名稱 + :id

總之，如果需要處理特定資料，就需要加上 :id，其他情況都用 HTTP 動詞來做變化。從網址上的複數名詞可以看出操作的對象，這就是 RESTful 的精神。

當然，一個專案裡不可能所有的路由都在操作資料，因此不會每條路由都能完美對齊 RESTful，例如「登入」的路由通常設定為 POST /login，就完全是以動詞為中心。身為專案的架構者，有時候還是需因地制宜，風格只是參考，不是阻礙。

總之，RESTful 這種「和 CRUD 對齊」的特性會帶來一些溝通的便利性，客戶端只需要知道可用資源，就能依照約定俗成的邏輯，推測出相關的 API。

這種「約定俗成」的設計感也可以讓工程師規劃路由時減少一些煩惱。

![HTTP Verbs.png](http://192.168.25.60:8000/files/file_storage/bcd98816.png)

![Status Codes.png](http://192.168.25.60:8000/files/file_storage/03d9edee.png)

### 舉個例子，我們在專案中實際的配置可能是
RESTful 是一個非常流行的路由設計的參考風格，當然，在實際開發專案時，可能還是會有一些因時制宜的考量。

下圖對照了「完全採用 RESTful 風格」 和實際的路由規劃做對照，用 * 標記出了不同的地方。

![RESTful 風格和實際的路由規劃.png](http://192.168.25.60:8000/files/file_storage/692d32a4.png)

- new 頁面和 edit 頁面：這兩條路由並不是「對 todos 進行資料操作」，但也是很常出現的頁面。因為本質仍然是瀏覽，所以動詞用 GET，另外在 URL 加上 new 和 edit 來表達功能屬性。
- 首頁 v.s. 瀏覽全部路由：在這裡我們選擇著重了首頁的語義，因此只配置路由 GET /，而沒有配置 GET /todos。

如果是部落格網站，可能會有叫做 posts 的資源，再搭配 HTTP 動詞和新增／瀏覽／修改／刪除等操作，就會出現如下變化：

![部落格網站路由.png](http://192.168.25.60:8000/files/file_storage/7552426c.png)

RESTful 的網址變化很有規律，也很簡潔，因此近年有愈來愈多人採用 RESTful 設計風格來架構網站，(注意，它是風格，而不是標準，意思是偶爾出現特例沒關係)。

# REST API Design

![REST API Design.png](http://192.168.25.60:8000/files/file_storage/d059e45e.png)

# API Architectural Styles

![API Architectural Styles Comparison.png](http://192.168.25.60:8000/files/file_storage/79773843.png)

