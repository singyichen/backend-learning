---
title: Redis
description: Redis 簡介
published: true
date: 2023-10-20T01:06:52.174Z
tags: db, redis
editor: markdown
dateCreated: 2022-12-14T00:06:36.235Z
---

# Redis
- [ ] [redis 官網](https://redis.io/)
- [ ] [[Node.js] 使用 Redis 內存來存取本地資料](https://medium.com/10coding/node-js-%E4%BD%BF%E7%94%A8-redis-%E5%85%A7%E5%AD%98%E4%BE%86%E5%AD%98%E5%8F%96%E6%9C%AC%E5%9C%B0%E8%B3%87%E6%96%99-9557660196f4)
- [ ] [Node.Js 使用 Redis 優化效能](https://codinghero.netlify.app/redis-in-node-js/)
- [ ] [node-redis](https://github.com/redis/node-redis)
- [ ] [@fastify/redis](https://github.com/redis/@fastify/redis)
- [ ] [ioredis](https://github.com/luin/ioredis)
- [ ] [Windows下安裝Redis圖文教學](https://blog.csdn.net/chen15369337607/article/details/119334531)
- [ ] [Redis-windows-1.安裝、啟用、存入取出資料、使用圖形化介面](https://www.dotblogs.com.tw/YiruAtStudio/2021/01/13/111530)
- [ ] [Redis 快速的開放原始碼記憶體內資料存放區，可做為資料庫、快取、訊息代理程式和佇列使用](https://aws.amazon.com/tw/redis/)
- [ ] [資料庫的好夥伴：Redis](https://blog.techbridge.cc/2016/06/18/redis-introduction/)
- [ ] [在 Node.js 使用 Redis 來 Cache API 的回傳資料，以減少查詢資料的回傳時間](https://medium.com/dean-lin/%E5%9C%A8-node-js-%E4%BD%BF%E7%94%A8-redis-%E4%BE%86-cache-api-%E7%9A%84%E5%9B%9E%E5%82%B3%E8%B3%87%E6%96%99-%E4%BB%A5%E6%B8%9B%E5%B0%91%E6%9F%A5%E8%A9%A2%E8%B3%87%E6%96%99%E7%9A%84%E5%9B%9E%E5%82%B3%E6%99%82%E9%96%93-4eeeb677b81e)
- [ ] [[Redis] 使用 Docker 安裝 Redis](https://marcus116.blogspot.com/2019/02/how-to-run-redis-in-docker.html)
- [ ] [Redis 教學](https://www.runoob.com/redis/redis-tutorial.html)
- [ ] [Nodejs IOREDIS: how to set expire time for a key?](https://stackoverflow.com/questions/41237001/nodejs-ioredis-how-to-set-expire-time-for-a-key)
- [ ] [Commands SET](https://redis.io/commands/set/)
- [ ] [六角鼠年鐵人賽 Week 19 - Redis - 快取伺服器介紹](https://hackmd.io/@KaiChen/H1yauUCjU)
- [ ] [Redis 學習筆記(3)-部署單機 Redis](https://ithelp.ithome.com.tw/articles/10285491)
- [ ] [Setup Redis with Docker Compose & use Redis CLI within a container](https://www.youtube.com/watch?v=nG8ex99AbTw&ab_channel=JasonCheung&loop=0)
- [ ] [Redis Cache Implementation with NodeJs](https://medium.com/globant/redis-cache-implementation-with-nodejs-6925f29eb70e)
- [ ] [Why is redis so fast?](https://blog.bytebytego.com/p/why-is-redis-so-fast?utm_source=profile&utm_medium=reader2)
- [ ] [Redis vs Memcached](https://blog.bytebytego.com/p/redis-vs-memcached?utm_source=profile&utm_medium=reader2)
- [ ] [Build a simple chat application](https://blog.bytebytego.com/p/ep18-build-a-chat-application-also?utm_source=profile&utm_medium=reader2)
- [ ] [EP19: live streaming, visa payment, Redis and more](https://blog.bytebytego.com/p/ep19-live-streaming-visa-payment?utm_source=profile&utm_medium=reader2)
- [ ] [Top 5 Uses of Redis](https://www.youtube.com/watch?v=a4yX7RUgTxI&ab_channel=ByteByteGo&loop=0)
- [ ] [Distributed Locking with Redis](https://varunkruthiventi.medium.com/distributed-locking-with-redis-af4e7d80b503)
- [ ] [Understanding Redis High-Availability Architectures](https://semaphoreci.medium.com/understanding-redis-high-availability-architectures-2b544fe3ec1e)
- [ ] [Building Scalable Microservice Architecture with Next.js, Node.js, Redis](https://mostafizur99.medium.com/building-scalable-microservice-architecture-with-next-js-node-js-redis-bec0e737f758)
- [ ] [Redis Unleashed: Powering Up with Node.js! ](https://smit90.medium.com/redis-unleashed-powering-up-with-node-js-c7531fa33e3e)
- [ ] [Understanding Redis High-Availability Architectures](https://semaphoreci.medium.com/understanding-redis-high-availability-architectures-2b544fe3ec1e)
- [ ] [A Crash Course in Redis](https://blog.bytebytego.com/p/a-crash-course-in-redis?utm_source=profile&utm_medium=reader2)
- [ ] [Top 5 Redis Use Cases in Distributed Systems](https://medium.com/@maheshsaini.sec/top-5-redis-use-cases-in-distributed-systems-6aadc73121c6)
- [ ] [The 6 Most Impactful Ways Redis is Used in Production Systems](https://blog.bytebytego.com/p/the-6-most-impactful-ways-redis-is?utm_source=profile&utm_medium=reader2)
- [ ] [Redis Can Do More Than Caching](https://blog.bytebytego.com/p/redis-can-do-more-than-caching?utm_source=profile&utm_medium=reader2)
# 什麼是 Redis？
> - **Redis** 代表 **Remote Dictionary Server** ，是快速的開放原始碼記憶體內資料存放區，可做為資料庫、快取、訊息代理程式和佇列使用。
> - **Redis** 的回應時間低於一毫秒，可讓遊戲、廣告科技、金融服務、醫療保健和 IoT 等產業的即時應用程式每秒處理數百萬個請求。如今，Redis 是當今最熱門的開源引擎之一，連續五年被 Stack Overflow 評為「最愛」資料庫。由於其快速的效能，Redis 是用於快取、工作階段管理、遊戲、排行榜、即時分析、地理空間、叫車、聊天/簡訊、媒體串流和發佈/訂閱應用程式的熱門選項。
> - **Redis** 是一個 分散式、支援持久化、使用 **key-value** 做資料處理的快取伺服器，或者應該用最熱門的詞彙來說明更貼切 – **NoSQL**

![redis slogan.png](http://192.168.25.60:8000/files/file_storage/f316060d.png)

Redis 是一種**內存資料庫** ( **in-memory** )的架構，可以被當作一個簡易的資料庫( **key-value database** )，這功能就讓我想起 Android 的 Shared Preferences 一樣是可以將資料存在用戶自己的裝置中，也等同於網頁中的 cookie、localStorage ，Redis 使用 RAM 來存取資料所以相對來得快速，缺點是重開機後記憶體就會被釋放掉，但 Redis 有個功能 persistence 可以解決這個問題，此外 Redis 也能設定資料的生命週期( Expire )當你在儲存資料的時候，你可以新增一個 Expire time 的參數，時間一到之後，這個 key 就會自動被清除。

# 為什麼 Redis 這麼快？

![why-is-redis-so-fast.png](http://192.168.25.60:8000/files/file_storage/947e3773.png)

![Why is Redis so fast colorful version.png](http://192.168.25.60:8000/files/file_storage/cb61aa0e.png)


1. Redis是一個基於RAM的數據庫。 RAM 訪問至少比隨機磁盤訪問快 1000 倍。
Redis is a RAM-based database. RAM access is at least 1000 times faster than random disk access.

2. Redis利用 IO 多路復用和單線程執行循環來提高執行效率。
Redis leverages IO multiplexing and single-threaded execution loop for execution efficiency.

3. Redis 利用了幾個高效的底層數據結構。
Redis leverages several efficient lower-level data structures.

# Redis 與 Memcached

![redis-vs-memcached.png](http://192.168.25.60:8000/files/file_storage/62045645.png)

# 安裝 Redis for Windows
## 下載 Redis-x64-3.0.504.msi 並安裝
Redis 主要是運行在 Linux 環境，因此在官網下載區是找不到 Windows 版安裝程式，需要到 [MicrosoftArchive/redis](https://github.com/MicrosoftArchive/redis) 提供的 github 頁面中點擊 [release page](https://github.com/microsoftarchive/redis/releases) 連結才有 Windows for Redis 安裝檔

# 安裝 圖形化使用者管理介面 GUI 
## 下載 Redis Desktop Manager：redis-desktop-manager-0.8.8.384.exe 並安裝
官網安裝最新版目前是要收費的，可以先使用它之前的版本(免費)，到官方 [github](https://github.com/uglide/RedisDesktopManager/releases/tag/0.8.8) 有提供舊的版本檔案下載

## 測試本機連線
點選 **Connect to Redis Server**，並輸入以下資訊，再按下 **Test Connection** ，連線成功後可以在左邊看到預設建立的 16 個 DB
- **Name**：本機 localhost
- **Host**：127.0.0.1
- **Port**：6349 (default)

![redis connect to redis server test connection.png](http://192.168.25.60:8000/files/file_storage/2fb7eb04.png)

# 安裝 Redis Client for Node.js
## 安裝套件 [@fastify/redis](https://github.com/redis/@fastify/redis)
> Fastify Redis connection plugin

```
npm install @fastify/redis --save
```

```
feat：add dependency @fastify/redis

原因：新增套件 @fastify/redis

調整項目：
1. package.json：
    - 新增套件 @fastify/redis ：Fastify Redis connection plugin
```

## 新增一個 redis plugin 
```js
/**
 * @description redis plugin
 */

const fastifyPlugin = require('fastify-plugin');
const fastifyRedis = require('@fastify/redis');
const errorLogger = require('./logger');

async function redisConnector(fastify) {
  fastify.register(fastifyRedis, {
    host: '127.0.0.1',
    // 設為 true 表示在 app 關閉時中斷連線
    closeClient: true,
  });
  /**
   * @description 取得單筆 redis 資料路由( GET /redis )
   */
  fastify.get('/redis', async (request, reply) => {
    try {
      const { redis } = fastify;
      const { key } = request.query;
      // redis 取得資料
      await redis.get(key, (err, val) => {
        reply.send(err || JSON.parse(val));
      });
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  });

  /**
   * @description 新增單筆 redis 資料路由( POST /redis )
   */
  fastify.post('/redis', async (request, reply) => {
    try {
      const { redis } = fastify;
      const { key, value } = request.body;
      // redis 新增資料
      await redis.set(key, JSON.stringify(value), (err) => {
        if (err) {
          errorLogger.error(err);
        }
      });
      // 回傳資料
      await redis.get(key, (err, val) => {
        reply.send(err || JSON.parse(val));
      });
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  });

  /**
   * @description 刪除單筆 redis 資料路由( DELETE /redis )
   */
  fastify.delete('/redis', async (request, reply) => {
    try {
      const { redis } = fastify;
      const { key } = request.query;
      // redis 刪除資料
      await redis.del(key, (err) => {
        reply.send(err || { status: 'ok' });
      });
    } catch (error) {
      errorLogger.error(error);
      console.log(error);
    }
  });
}

module.exports = fastifyPlugin(redisConnector);

```
## 在 app.js 中 register redis plugin
```js
// import plugin
const fastifyRedisPlugin = require('./src/plugin/redis');
// init app
const app = fastify({
  // init logger
  logger: true,
});
// redis
app.register(fastifyRedisPlugin);
```
## Postman 測試
- 新增資料

```json
{
    "key":"Test",
    "value":"hello world"
}
```

```json
{
    "key":"Test Json",
    "value":
    {
        "Test":"hello world"
    }
}
```

![redis postman request post example.png](http://192.168.25.60:8000/files/file_storage/7902c73c.png)

- 取得資料

![redis postman request get example.png](http://192.168.25.60:8000/files/file_storage/b943dd06.png)

- 刪除資料

![redis postman request delete example.png](http://192.168.25.60:8000/files/file_storage/636910b7.png)

## GUI 查看 data
![redis gui key value example.png](http://192.168.25.60:8000/files/file_storage/e88e799f.png)

# Docker 部屬 Redis
## 在 Docker 起一個 container
-  在 docker-compose.yml 中新增 redis

```yaml
  redis:
    restart: always
    container_name: redis
    image: redis:alpine
    ports:
      - "6379:6379"
    volumes:
      - "/data/redis_data:/data"
```

- 將 redis_data 資料夾讀寫權限設定 777，原本會是 755 ( owner 可讀/寫/執行, 群組用戶及其他用戶可讀/執行)
```
cd /data/redis_data
```
```
sudo chmod 777 redis_data
```
## 操作 Redis
- 進入到 container 裡面
```
docker exec -it redis sh
```

- 使用 redis-cli
```
redis-cli
```

- 測試是否連線成功
```
ping
```

- 列出所有 key 
```
keys *
```

- 新增一個 key
```
setex mykey 60 "hello world"
```

- 取得一個 key
```
mget mykey
```

- 離開 container 
```
exit
```

# Redis 的優勢
## 效能
所有 Redis 資料都存放在記憶體中，從而實現低延遲和高輸送量的資料存取。與傳統資料庫不同，記憶體內資料儲存不需要存取磁碟，從而將引擎延遲縮減到微秒。因此，記憶體內資料儲存能夠支援更大規模的操作，而且回應時間更快。這項優勢提供超快速的效能，平均讀取和寫入操作時間低於一毫秒，並支援每秒百萬個操作。
## 彈性的資料結構
其他鍵值資料存放區提供的資料結構有限，而 Redis 則提供多樣化的資料結構以滿足應用程式的需要。Redis 資料類型包括：
- **字串 String**：文字或二進位資料，最大 512 MB
- **清單 List**：按新增順序排列的字串集合
- **資料集 Set**：未排序的字串集合，可與其他資料集類型交叉、合併和區分
- **排序資料集 sorted set**：依數值排列的資料集
- **雜湊 Hash**：存放欄位和值清單的資料結構
- **點陣圖**：提供位元層級操作的資料類型
- **HyperLogLogs**：用來評估資料集獨特項目的機率資料結構
- **串流**：日誌資料結構訊息佇列
- **地理空間**：以經度/緯度為基礎的項目地圖，「附近」
- **JSON**：巢狀式、半結構化命名值物件，支援數字、字串、布林值、陣列和其他物件

## 簡單易用
Redis 可讓您用更少、更簡單的行編寫傳統上複雜的程式碼。藉由 Redis，您只要撰寫幾行程式碼即可存放、存取和使用應用程式的資料。差異在於使用 Redis 的開發人員使用簡單的命令結構，而不是傳統資料庫的查詢語言。例如，您使用 Redis 雜湊資料結構，僅用一行程式碼即可將資料移至資料存放區如果在沒有雜湊資料結構的資料存放區進行類似的任務，則需要撰寫許多行的程式碼才能將格式轉換為另一種格式。Redis 隨附可操控和與資料互動的原生資料結構和許多選項。Redis 開發人員可使用超過一百種開放原始碼用戶端。支援的語言包含 Java、Python、PHP、C、C++、C#、JavaScript、Node.js、Ruby、R、Go 等等。

## 複寫和持續性
Redis 採用主要-複本架構，並支援非同步複寫，可將資料複寫到多部複本伺服器。這不但可提升讀取效能 (因為請求可分割到多部伺服器)，還能在主伺服器發生故障時快速恢復。至於持久性，Redis 支援時間點備份 (將 Redis 資料集複製到磁碟)。

Redis 並非建置為持久且一致的資料庫。如果您需要持久且與 Redis 相容的資料庫，請考慮 Amazon MemoryDB for Redis。 由於 MemoryDB 使用持久的交易日誌，該日誌跨多個可用區域 (AZ) 存放資料，您可以將其用作主要資料庫。MemoryDB 專為使開發人員能夠使用 Redis API 而無需擔心管理單獨的快取、資料庫或底層基礎設施而打造。

## 高可用性和可擴展性
Redis 在單一節點的主要或叢集拓撲提供主要-複本架構。這可讓您建立高度可用解決方案，以提供一致的效能和可靠性。需要調整叢集大小時，還可利用擴展和縮減或擴增等各種選項。這可讓您的叢集隨著需要擴展。

## 開源
Redis 是開源專案，由充滿活力的社群所支援，包括 AWS。由於 Redis 使用開放式標準、支援開放資料格式並含有豐富的用戶端，因此沒有廠商或技術鎖定的問題。

# Redis 熱門使用案例

![redis use cases.png](http://192.168.25.60:8000/files/file_storage/eb32047e.png)

## 快取
Redis 是實作高可用性記憶體內快取的絕佳選項，可減少資料存取延遲、增加輸送量，以及減輕關聯式或 NoSQL 資料庫和應用程式的負載。Redis 可提供低於一毫秒的回應時間來服務頻繁要求的項目，且讓您能夠輕鬆地針對較高的負載進行擴展，無須擴充較為昂貴的後端。資料庫查詢結果快取、持久性工作階段快取、網頁快取，以及影像、檔案和中繼資料等快取常用物件都是 Redis 快取的常見範例。

## 聊天、簡訊和佇列
Redis 支援含模式比對功能的發佈/訂閱，以及清單、排序資料集和雜湊等各種資料結構。這讓 Redis 能夠支援高效能聊天室、即時註解串流、社交媒體饋送及伺服器互相通訊。Redis 清單資料結構可輕鬆實作輕量型的佇列。清單提供原子操作以及封鎖功能，因此適合用於需要可靠的訊息代理程式或循環清單的各種應用程式。

## 遊戲排行榜
對於想要建立即時排行榜的遊戲開發人員而言，Redis 是絕佳的選擇。只要使用 Redis 排序集資料結構，即可提供元素的唯一性，同時維護依使用者分數排序的清單。建立即時排名清單就和每次使用者分數變更時進行更新一樣簡單。您也可以透過利用時間戳記做為分數，使用排序集處理時間序列資料。

## 工作階段存放區
應用程式開發人員廣泛使用 Redis 做為高度可用且持久的記憶體內資料存放區，用以存放和管理網際網路規模應用程式的工作階段資料。Redis 提供管理使用者描述檔、登入資料、工作階段狀態和使用者專屬個人化等工作階段資料所需的低於一毫秒延遲、擴展和彈性。

## 豐富的媒體串流
Redis 提供快速的記憶體內資料存放區，以支援即時串流使用案例。Redis 可用於存放使用者描述檔的相關中繼資料，以及檢視數百萬個使用者的歷史記錄、身份驗證資訊/字元以及資訊清單檔案，以啟用 CDN 將影片一次串流到數百萬行動和桌面使用者。

## 地理空間
Redis 提供專門打造的記憶體內資料結構和運算子，可大規模快速管理即時地理空間資料。利用 GEOADD、GEODIST、GEORADIUS 和 GEORADIUSBYMEMBER 等命令即時存放、處理和分析地理空間資料，讓 Redis 可輕鬆快速利用地理空間。您可以使用 Redis 新增以位置為基礎的功能，例如行車時間、行車距離，以及應用程式的興趣點。

## Machine Learning
現代的資料驅動應用程式需要機器學習迅速處理大量、多樣化和快速的資料，並自動做出決策。像遊戲和金融服務的詐騙偵測、廣告技術的即時競標以及約會配對和共乘等使用案例，在數十毫秒內處理即時資料和做出決策的能力最為重要。Redis 提供快速的記憶體內資料存放區讓您迅速建立、訓練和部署機器學習模型。

## 即時分析
Redis 可與 Apache Kafka 和 Amazon Kinesis 等串流解決方案搭配使用做為記憶體內資料存放區，以低於一毫秒的延遲導入、處理和分析即時資料。Redis 非常適合用於即時分析使用案例，例如社交媒體分析、廣告目標、個人化和 IoT。

# Redis 語言支援
最一開始的介紹就有提到了，Redis 發展至今目前支援共 51 種語言，幾乎可以說是所有軟韌體工程師都可以使用的一個軟體。

# Redis 功能
## Pub/Sub (Publish/Subscribe)
- Redis 提供針對一個 Key 進行 Pub/Sub 設定，意即當該 Key 有新訊息進行 Pub 時，所有 Sub 的客戶端都會收到新的訊息。
- 常見於即時聊天、社群平台等應用。
## Transactions
- Redis 是少數能夠提供類似於 SQL ACID 性質 Transactions 的軟體，並有提供 WATCH 命令得以監控執行期間資料是否被異動，若被異動，則整個 Transaction 會取消。
- 此外 Redis 並不支援 Roll Back 功能，這是因其面向產品而生的軟體，因此極其單純的僅是處理送入的指令，而這種錯誤通常在開發環節就 “應該” 要被修正，因此 Redis 並不提供任何過於複雜的機制，此包含了 Roll Back 功能。

# 聊天室實作

![How do we build a simple chat application using Redis.png](http://192.168.25.60:8000/files/file_storage/2966971c.png)