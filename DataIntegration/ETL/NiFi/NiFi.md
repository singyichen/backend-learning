---
title: NiFi
description: NiFi 介紹
published: true
date: 2023-07-13T08:23:08.643Z
tags: nifi
editor: markdown
dateCreated: 2023-07-06T00:36:02.732Z
---

# NiFi
- [ ] [Apache NiFi - 讓你輕鬆設計 Data Pipeline 系列](https://ithelp.ithome.com.tw/users/20140257/ironman/4025)
- [ ] [Apache NiFi系列文章](https://www.cnblogs.com/alanchan2win/p/17403621.html)
- [ ] [Apache NIFI中文文件](https://nifichina.github.io/)
- [ ] [Nifi：nifi的基本使用](https://codingnote.cc/zh-tw/p/388377/)
- [ ] [Apache NIFI 講解(讀完立即入門)](https://cloud.tencent.com/developer/article/1690392)

# 關於 NiFi

![nifi logo.png](http://192.168.25.60:8000/files/file_storage/94ea469a.png)

Apache NiFi 是一個易於使用，功能強大且可靠的系統，用於處理和分發資料。可以自動化管理系統間的資料流。它使用高度可組態的指示圖來管理資料路由、轉換和系統中介邏輯，支援從多種資料來源動態拉取資料。NiFi 原來是 NSA 的一個項目，目前已經程式碼開源，是 Apache 基金會的頂級項目之一。

NiFi 是基於 Java 的，使用 Maven 支援包的建構管理。 NiFi 基於 Web 方式工作，後台在伺服器上進行調度。使用者可以將資料處理定義為一個流程，然後進行處理，NiFi 後台具有資料處理引擎、任務調度等元件。

簡單的說，NiFi 就是為瞭解決不同系統間資料自動流通問題而建立的。

雖然 dataflow 這個術語在各種場景都有被使用，但我們在這裡使用它來表示不同系統間的自動化的可管理的資訊流。自企業擁有多個系統開始，一些系統會有資料生成，一些系統要消費資料，而不同系統之間資料的流通問題就出現了。這些問題出現的相應的解決方案已經被廣泛的研究和討論，其中企業整合 eip 就是一個全面且易於使用的方案。

dataflow 要面臨的一些挑戰包括：

- 網路故障，磁碟故障，軟體崩潰，人為事故。( Systems fail )

- 有時，給定的資料來源可能會超過處理鏈或交付鏈的某些部分的處理能力，而只需要一個環節出現問題，整個流程都會受到影響。( Data access exceeds capacity to consume )

- 總是會得到太大、太小、太快、太慢、損壞、錯誤或格式錯誤的資料。( Boundary conditions are mere suggestions )

- 現實業務或需求變更快，設計新的資料處理流程或者修改已有的流程必必須要迅速。( What is noise one day becomes signal the next )

- 給定的系統所使用的協議或資料格式可能隨時改變，而且常常跟周圍其他系統無關。dataflow 的存在就是為了連接這種大規模分佈的，鬆散的，甚至根本不是設計用來一起工作的元件系統。( Systems evolve at different rates )

- 法律，法規和政策發生變化。企業對企業協議的變化。系統到系統和系統到使用者的互動必須是安全的，可信的，負責任的。( Compliance and security )

- 通常不可能在測試環境中完全模擬生產環境。( Continuous improvement occurs in production )

多年來，資料流一直是架構中不可避免的問題之一。現在有許多活躍的、快速發展的技術，使得 dataflow 對想要成功的特定企業更加重要，比如soa，API，iot，bigData。此外，合規性，隱私性和安全性所需的嚴格程度也在不斷提高。儘管不停的出現這些新概念新技術，但 dataflow 面臨的困難和挑戰依舊，其中主要的區別還是複雜的範圍，需要適應的需求變化的速度以及大規模邊緣情況的普遍化。NiFi 旨在幫助解決這些現代資料流挑戰。

# 核心概念
NiFi 的基本設計概念與基於流程的程式設計 FBP 的主要思想密切相關。以下是一些主要的 NiFi 概念以及它們如何對應到 FBP：

|          NiFi 術語        |          FBP Term        |  描述 |
|:-----------------------:|:----------------------------------------:|:-----------:|
|         FlowFile        |    Information Packet    | FlowFile 示在系統中移動的每個對象，對於每個 FlowFile，NiFi 都會記錄它一個屬性鍵值對和 0 個或多個位元組內容( FlowFile 有 `attribute` 和 `content` )。  | 
|    FlowFile Processor   |        Black Box         |    實際上是處理器起主要作用。在 eip 術語中，處理器就是不同系統間的資料路由，資料轉換或者資料中介的組合。處理器可以訪問給定 FlowFile 的屬性及其內容。處理器可以對給定工作單元中的零或多個流檔案進行操作，並提交該工作或回滾該工作。     | 
|        Connection       |     Bounded Buffer       |    Connections 用來連接處理器。它們充當佇列並允許各種處理程序以不同的速率進行互動。這些佇列可以動態地對進行優先順序排序，並且可以在負載上設定上限，從而啟用背壓。     |  
|     Flow Controller     |        Scheduler         |    流控製器維護流程如何連接，並管理和分配所有流程使用的執行緒。流控製器充當代理，促進處理器之間流檔案的交換。     |
|      Process Group      |          subnet          |    處理程序組裡是一組特定的流程和連接，可以通過輸入連接埠接收資料並通過輸出連接埠傳送資料，這樣我們在處理程序組裡簡單地組合元件，就可以得到一個全新功能的元件( Process Group )。    | 


此設計模型也類似於 seda ，帶來了很多好處，有助於 NiFi 成為非常有效的、建構功能強大且可擴展的資料流的平台。其中一些好處包括：

- 有助於處理器有向圖的可視化建立和管理
- 本質上是非同步的，允許非常高的吞吐量和足夠的自然緩衝
- 提供高並行的模型，開發人員不必擔心並行的複雜性
- 促進內聚和鬆散耦合元件的開發，然後可以在其他環境中重複使用並方便單元測試
- 資源受限的連接(流程中可組態 connections )使得背壓和壓力釋放等關鍵功能非常自然和直觀
- 錯誤處理變得像基本邏輯一樣自然，而不是粗粒度的全部捕獲( catch-all )
- 資料進入和退出系統的點，以及它是如何流動的，都是容易理解和跟蹤的。

# 架構 Architecture
## 基本架構 Basic
Apache NiFi 是透過 Java 來做開發，所以根據官方檔案所提供的架構圖，我們可以看見 NiFi 的 Core Component 都位於 JVM 上 :

![nifi architecture.png](http://192.168.25.60:8000/files/file_storage/91999f16.png)

從下往上一一介紹

### FlowFile Repository
Repository 就是一個存放的地方，在 NiFi 的運作原理會經過壓縮與 WAL 方式寫入在所屬的 instance 上。

因此 FlowFile Repository 就是將 FlowFile 經過我們於 NiFi 所設計的 Data Pipeline 過程中，將其狀態做一個保存。舉例來說，Pipeline 有 3個 Processor，FlowFile 在每經過一個 Processor 都會將他的狀態和 metadata 儲存於 FlowFile Repository。

### Content Repository
Content 就是指的是 FlowFile 真實的資料內容，Nifi 會將這樣的內容透過壓縮與加密，再接著存放到自身的 FileSystem，也就是Content Repository。

### Provenance Repository
Provenance Repository 是用來存放所有 Flowfile 的追蹤事件，也就是記錄著 FlowFile 從哪裡留到目前的 Processor，以及後續以哪一個 Processor 作為下一步流向的標的。主要就是用來追蹤 FlowFiles 在每個 Processor 的狀態，包含時間、資料變化等

### Flow Controller
Flow Controller 你可以想像著他整個 NiFi 的操作核心，所有 Pipeline 的觸發與排程，以及對於給 Processor 的相關資源，都是由它來做一個分派與調度。

### Web Server
Apache NiFi 有自己的 API 可做使用，所以除了可以在 Web UI 操作之外，我們也可透過呼叫 API 的方式來做設定與執行，但其實 Web UI 也就是基於 Web Server 來根據使用者在 UI 的操作來執行對應的 API。所以這就是一個控制 NiFi API 的地方。


## 叢集架構 Clustering
Apache NiFi 除了可以建立成 Single Node 之外，也可以建立成 clustering 的架構，根據官方提供的圖參考如下：

![nifi cluster architecture.png](http://192.168.25.60:8000/files/file_storage/c82814cd.png)

但是要注意一點的地方是，NiFi 的 Clustering 與一般我們所想到的像是 EMR、hadoop 的那種 Clustering 不太一樣。通常最常見的 Clustering，會有一個 Master (或稱 Leader)，去搭配其他的 Worker (或稱 Follower )。但在 NiFi 的設計中，它是採用『Zero-Master』的 Clustering，也就是每一個在 Clustering 的 Node 都會負責到 Data 的處理，差別在於 Data 會被切分到各個 Node 中。

正因為沒有一個主要的 Master Node，所以使用者可以從各個 Node 去操作 UI，而 NiFi 就會來同步到其他 Node 中。舉例來說，使用者在 A Node 加入一個 Processor，而使用者也可以在同一個 Clustering 中的 B Node 看到剛剛所加入的 Processor。

除了 Nifi 本身之外，還會一個 Apache Zookeeper 的服務，主要是用來執行故障的處理，而它會選擇一個 Node 作為 Coordinator，其他 Node 要向 Coordinator 發送 Heartbeats 和回報狀態來確保彼此之間的連線(預設為 5 秒發送一次)。所以當有一個 Node 沒有在時間內回報任何資訊時，Coordinator 就會斷開該 Node，直到恢復連線為止。

# 元件 Component
一開始，會先大概講一下在 Apache NiFi 中最重要的幾個 component，會先有簡單的描述來讓大家可以容易理解，而往後也會一ㄧ套用到實際的操作來做一個呼應，這裡先帶出一張自己設計的圖，接著搭配下面的 component 來介紹，圖如下：

![nifi component.png](http://192.168.25.60:8000/files/file_storage/7852559c.png)

## FlowFile
何謂 FlowFile？我們可以想像是資料中或是 File 中的一筆 Record，甚至是一包資料同時含有很多筆 record，今天假設有一張 Table 且其中有 100 筆資料時，當 NiFi 從中讀取時，這 100 筆 Record 就會在 NiFi 產生 100 筆 FlowFile，而 FlowFile 會帶有自己的 `attribute` 和 `content`，這兩個有什麼差異呢？

- `attribute`：可以想像成是 metadata，以 `Key-Value` 的方式來對 FlowFile 的描述，包含 size, path, permission 等
- `content`：真實 data 的內容，可能是 csv，json 等格式。

## Processor
可以想像成是一個邏輯處理的 Unit，在 NiFi 中它提供了許多內建的 Processor，可參考官方連結，這可使我們透過設定的方式來產生、處理、轉換、輸出 FlowFile 等相關操作。

因此，我們可以從上圖的橘線，可看到在一個 Pipeline 當中，FlowFiles 會經過中間多個 Processor 的處理，而第一個 Processor 會被用來產生 FlowFiles (ex. 讀取 DB 或 files ); 而最後一個通常會是一個落地輸出的 Processor (ex. 寫入 DB 或 files )

> 如果有熟悉 Apache Airflow 的話，Processor 的概念就是類似於 Airflow 的 `operator`。
{.is-info}

## Connection
在 NiFi 建立 Data Pipeline 的時候，其實就是透過一連串 Processor 來建置，而Processor 彼此之間會建立一個關係，就稱作為 Connection，可從圖中看到 Processor 之間一定會有個 Connection 的存在。

在 Connection 當中，我們可以透過設定來描述該 Connection 的定義，以上圖中的綠框與紅框分別代表著最常見的 Success 和 Failed，若今天有 FlowFiles 是走 Success 的 Connection (上圖中的橘線)，也就代表上一個 Processor 是處理成功的。因此，我們可自身建立多種 Connection 來決定每一條下游路徑的狀態與意義為何。

此外，在 Connection中我們還可以設定 Queue 的狀態，像是 FIFO 或是 New First 等，來緩解當 Connection 中因有太多 Flowfiles 時所導致效能的問題。

## Processor Group
Processor Group 通常被用來作為 Processor 的 Module ，為 Processors 的集合。最常發生在什麼樣的情境呢？會有 3 種情境需要 Processor Group:

1. 今天假如有兩個 pipeline，其中有一段的流程是一模一樣的，那這時候我們就可以把那一段 Processors 獨立做成 Processor Group ，而後續若遇到一樣的需求時，我就只要拉這個 Processor Group 做串接即可，使用這就不需要再一一建立流程。簡單來說，就是視為『Module』的意思。

2. 第二個會用到的情境就是『分專案或部門』為使用，若今天有一個 Team，同時有10個專案需要建立 Data Pipeline，理所當然每一個專案的流程都會不一樣，這時候就可以透過 Processor Group 來做專案的劃分；或是有不同 Team 要採用時，也可以利用這個方式來劃分不同 Team。

3. 第3種是第2種的延伸，Processor Group 通常也會是一個 User 權限的最小單位，我們可以針對特定 Processor Group 來決定哪些 User 擁有 Write 或 read 的權限

其中情境1，我們可以看到上圖的橘框，它被整合在 Pipeline 的其中一環，但其實我們將其放大來看，他就是由一連串的 Processor 組合而成，所以未來若有其他 Pipeline 需要類似的情境時，他僅可拉這個 Processor Group，就不需要再重頭拉一次原先 Processor Group 內部的流程。

> 若對應到 Airflow，其實就是類似於 Subdag 或是 Airflow2.0 的 `TaskGroup`
{.is-info}

## Funnel
用來將多個 source connection 的組合成單一個 connection，這對於可讀性可以提供相當大的幫助，如同上圖的紫框，我們可以想像假設有 n 個 Processor 同時要連到同一個 Processor，如果不透過 Funnel 的話，在下游的那一個 Processor 身上會有很多條線，這對於使用者在檢視或是 Debug 是不理想的。

接下來的 Component 介紹，就沒有呈現在上圖，但也是有一定的重要性。

## Controller Service
Controller Service 可以想像成他是一個與第三方對接的 connection ，注意不是前面所提到的 connection ，而是真的透過網路服務建立的 connection。

當我們選擇 NiFi 作為我們 Data Pipeline 的工具時，照理來說就會在服務上建立許多的 Pipeline，甚至有些當中的 Processor 會共同存取同一個 DB 或是 cloud 的服務，如果每一個 Processor 都對其建立太多 connection 的話可能會造成 DB 或 cloud 的問題。

所以此時就可以統一透過 Controller Service 來做一個管理，他可以事先建立好一個與第三方服務的 connection ，而當有 Processor 需要做使用的就可以直接套用對應的 Controller Service ，一方面不用再重新設定，另一方面可以對目標存取控制好連線數以節省開銷。

## Reporting Task
Reporting Task 是 NiFi 在做 Monitoring 很重要的角色，他可以將一些 Metrics、Memory & Disk Utilization、以及一些 Monitoring 的資訊發送到出來，常見應用是發送到 Prometheus、DataDog、Cloudwatch 等第三方服務來做視覺化呈現。

## Templates
Templates 通常用於轉換環境做使用，假設我有一個既有的 NiFi 在 A 機器上，但這時候我需要轉換環境到 B 機器上，此時使用者可以把在 A 機器上所有的 Pipeline 輸出成 Templates（為 XML 檔），接著再匯入到 B 機器上的 NiFi，就可以在新的環境中繼續使用相同的 Pipeline了

這當中的轉換是以 Processor Group 作為單位，其中也會把這個底下的所需要用到的參數和設定一起匯出成 templates，所以在環境轉換時就會是無痛轉移。簡單的呈現如下圖：

![nifi templates.png](http://192.168.25.60:8000/files/file_storage/4b477c35.png)

# 主要功能
## 資料流管理 Flow Management 
### 保證交付 Guaranteed Delivery 

NiFi 的核心理念是，即使在非常高的規模下，也必須保證交付。這是通過有效地使用專門建構的 Write-Ahead Log 和 content repository 來實現的。它們一起被設計成具備允許非常高的事務速率、有效的負載分佈、寫時複製和能發揮傳統磁碟讀/寫的優勢。

### 數據緩衝與背壓和壓力釋放 Data Buffering w/ Back Pressure and Pressure Release

NiFi 支援緩衝所有排隊的資料，以及在這些佇列達到指定限制時提供背壓的能力，或在資料達到指定期限（其值已失效）時老化資料的能力。

### 優先級排序 Prioritized Queuing

NiFi 允許設定一個或多個優先順序方案，用於如何從佇列中檢索資料。默認情況是先進先出，但有時應該首先提取最新的資料(後進先出)、最大的資料先出或其他定製方案。

### 特定於流程的服務品質（延遲 v 吞吐量、容錯率等） Flow Specific QoS (latency v throughput, loss tolerance, etc.)

可能在資料流的某些節點上資料至關重要，不容丟失，並且在 某些時刻這些資料需要在幾秒鐘就處理完畢傳向下一節點才會有意義。對於這些方面 NiFi 也可以做細粒度的組態。

## 使用便捷性 Ease of Use

### 視覺化指令和控制 Visual Command and Control

資料流的處理邏輯和過程可能會非常複雜。能夠可視化這些流程並以可視的方式來表達它們可以極大地幫助使用者降低資料流的複雜度，並確定哪些地方需要簡化。NiFi 可以實現資料流的可視化建立，而且是即時的。並不是“設計、部署”，它更像泥塑。如果對資料流進行了更改，更改就會立即生效，並且這些更改是細粒度的和元件隔離的。使用者不需要為了進行某些特定修改而停止整個流程或流程組。

### 流程模板 Flow Templates

FlowFIle 往往是高度模式化的，雖然通常有許多不同的方法來解決問題，但能夠共享這些最佳實踐卻大有幫助。流程範本允許設計人員建構和發佈他們的流程設計，並讓其他人從中受益和復用。

### 資料來源 Data Provenance

在對象流經系統時，甚至在扇入、扇出、轉換等過程，NiFi 會自動記錄、索引並提供可用的源資料。這些資訊在支援法規遵從性、故障排除、最佳化以及其他方案中變得極其關鍵。

### 恢復 / 記錄一個滾動緩衝區的細節歷史記錄 Recovery / Recording a rolling buffer of fine-grained history

NiFi 的content repository 旨在充當歷史資料的滾動緩衝區。資料僅在 content repository 老化或需要空間時才會被刪除。 content repository 與 data provenance 能力相結合，為在對象的生命週期中的特定點（甚至可以跨越幾代）實現可以查看內容，內容下載和重放等功能提供了非常有用的基礎。

## 安全性 Security

### 系統對系統 System to System

資料流越安全越好。對於資料流中每個節點 NiFi 都是通過使用加密協議（如雙向 SSL ）來安全地交換資料。此外，NiFi 的流程能夠加密和解密內容，並在傳送方/接收方任何一側使用共享金鑰或其他機制來保證資料的安全。

### 使用者對系統 User to System

NiFi 支援雙向 SSL 身份驗證，並提供可插拔授權方式，以便能夠正確控制使用者的存取權和特定等級（唯讀，資料流管理， admin ）。如果使用者在流程中輸入敏感屬性（如密碼），則會立即在伺服器端加密，保證敏感資訊不會再次暴露在客戶端(前端 UI )中(比如使用者 A 在流程中輸入了 MySQL 的使用者密碼，填寫完畢後任何人即使是使用者 A 也看不到明文密碼)。

### 多租戶授權 Multi-tenant Authorization

NiFi 資料流的權限等級適用於每個元件，並且允許管理員使用者擁有細粒度的控制訪問等級。這意味著每個 NiFi 叢集都能夠處理一個或多個組織的需求。與隔離拓撲相比，多租戶授權支援資料流管理的自助服務，允許每個團隊或組織在完全瞭解流的其餘部分的情況下管理流，而無法訪問流。

## 可擴展架構 Extensible Architecture

### 擴充功能 Extension

NiFi 的核心是可擴展，因此它是一個可以以可預測和可重複的方式去執行和互動的資料流流程平台。可擴展的包括：processors, Controller Services, Reporting Tasks, Prioritizers, 和 Customer User Interfaces。

### 類加載器隔離 Classloader Isolation

對於任何基於元件的系統，涉及依賴的問題時常發生。NiFi 通過提供自訂類載入器(在下面的 NiFi 原始碼會有進一步講解)來解決這個問題，確保每個擴展包都暴露在一組非常有限的依賴中。因此，建構擴展包的時候不必擔心它們是否可能與另一個擴展包衝突。這些擴展包的概念稱為“ NiFi Archives ”，在 Developer’s Guide 中有更詳細的討論。

### 站點對站點通信協議 Site-to-Site Communication Protocol

NiFi 實例之間的首選通訊協議是 NiFi 站點到站點（S2S）協議。 S2S 輕鬆，高效，安全地將資料從一個 NiFi 實例傳輸到另一個實例。 NiFi 客戶端庫可以輕鬆建構並捆綁到其他應用程式或裝置中，通過 S2S 協議與 NiFi 進行通訊。 S2S 中支援以 socket 的協議和 HTTP（S） 協議作為底層傳輸協議，使得可以將代理伺服器嵌入到 S2S 協議的通訊中。

## 靈活的擴展模型 Flexible Scaling Model

### 擴展（集群） Scale-out (Clustering)

NiFi 的設計是可叢集，可橫向擴展的。如果組態單個節點並將其組態為每秒處理數百 MB 資料，那麼可以相應的將叢集組態為每秒處理GB級資料。但這也帶來了 NiFi 與其獲取資料的系統之間的負載平衡和故障轉移的挑戰。採用基於非同步排隊的協議（如消息服務，Kafka 等）可以提供幫助解決這些問題。使用 NiFi 的 s2s 功能也非常有效，因為它是一種協議，允許 NiFi 和客戶端（包括另一個 NiFi 群集）相互通訊，共享有關載入的資訊，以及交換特定授權的資料連接埠。

### 擴展與縮減 Scale-up & down

NiFi 還可以非常靈活地擴展和縮小。從 NiFi 框架的角度來看，在增加吞吐量方面，可以在組態時增加“調度”選項卡下處理器上的並行任務數。這允許更多執行緒同時執行，從而提供更高的吞吐量。另一方面，您可以完美地將NiFi縮小到適合在邊緣裝置上運行，因為硬體資源有限，所需的佔用空間很小，這種情況可以使用 MINIFI ，我們可以在此處找到更多詳細資訊：[MiNiFi](https://cwiki.apache.org/confluence/display/MINIFI)

# 優缺點
## 優點
- 支援 AWS, GCP, Azure 之 cloud db 和 storage 相關服務，ex. S3, GCS, PubSub, Knesis 等。
- 支援 opensource DB，ex. Cassandra、MySQL、MongoDB 等。
- 主要透過 Web UI 視覺化之設定與拖拉的方式即可建立 Data pipeline。
- 可以 Batch 和 Streaming 做切換。
- 具備 Monitoring 機制來監控 data 的處理狀況。
- 具備 Module Group 的機制，讓 Pipeline 變成模組來重複使用。
- 可建置成 clustering(叢集)，來使大量資料做 load balance。
- 可針對 Data pipeline 去達到像 Git 一樣的版本控制，讓使用者可以指定版本去使用對應的 pipeline。
- 可搭配自己撰寫的 Script (ex. python, golang, etc.)去做資料處理與控制。
- 容易追蹤每筆資料的流向與轉換狀況。
- 對於錯誤處理有提供完善的措施。
- 如果 Pipeline 有問題時，不需要停止 Pipeline的運作，只要針對特定的 Processor 作處理即可。

## 缺點
- 介面設定的參數較多，涵蓋的 Component 也較多，所以時常需要搭配 document 去做實作與理解。
- 需要 Self-hosted 和 Self-maintain。
- 假如是Clustering(叢集)的狀況下，若需要再額外加入一個 node，需要將服務 downtime，接著更改設定再重啟。
- 台灣本身的 Community 和 UseCase 相對較少，但在國外有許多 Community 和相關的資源與 Case 可以去做參考跟尋找。


