---
title: Docker
description: Docker學習筆記
published: true
date: 2023-11-12T23:35:26.530Z
tags: docker, devops
editor: markdown
dateCreated: 2022-07-19T06:36:20.283Z
---

# Docker
- [ ] [Docker Roadmap](https://roadmap.sh/docker)
- [ ] [Docker簡介](https://cutejaneii.gitbook.io/docker/docker/untitled)
- [ ] [Docker的故事及原理](https://joshhu.gitbooks.io/docker_theory_install/content/index.html)
- [ ] [Docker 初探：基本指令與簡單介紹 Dockerfile 和 docker-compose](https://askie.today/docker-dockerfile-dockercompose-intro/)
- [ ] [Docker 筆記](https://blog.poychang.net/note-docker/)
- [ ] [Docker 實戰系列（一）：一步一步帶你 dockerize 你的應用](https://larrylu.blog/step-by-step-dockerize-your-app-ecd8940696f4)
- [ ] [Docker 基本教學](https://ithelp.ithome.com.tw/articles/10199339)
- [ ] [docker-tutorial](https://github.com/twtrubiks/docker-tutorial)
- [ ] [《Docker —— 從入門到實踐》正體中文版](https://philipzheng.gitbook.io/docker_practice/)
- [ ] [Docker學習筆記](https://peihsinsu.gitbooks.io/docker-note-book/content/)
- [ ] [Docker筆記 - Docker基礎教學](https://medium.com/alberthg-docker-notes/docker%E7%AD%86%E8%A8%98-docker%E5%9F%BA%E7%A4%8E%E6%95%99%E5%AD%B8-7bbe3a351caf)
- [ ] [六角鼠年鐵人賽 Week 1 - 介紹 Docker](https://hackmd.io/3dxF5Nb1RUyvjneYP11kRA)
- [ ] [What are the differences between Virtualization (VMware) and Containerization (Docker)?](https://blog.bytebytego.com/p/what-are-the-differences-between?utm_source=profile&utm_medium=reader2)
- [ ] [How does Docker work](https://blog.bytebytego.com/p/ep57-how-chatgpt-works-technically?utm_source=profile&utm_medium=reader2)
- [ ] [Docker Network in Depth](https://towardsdev.com/docker-network-in-depth-2387a3508926)
- [ ] [Docker 是什麼？Docker 基本觀念介紹與容器和虛擬機的比較](https://www.omniwaresoft.com.tw/product-news/docker-news/docker-introduction/)
- [ ] [Docker — What it is, How Images are structured, Docker vs. VM and some tips (part 1)](https://ragin.medium.com/docker-what-it-is-how-images-are-structured-docker-vs-vm-and-some-tips-part-1-d9686303590f)
- [ ] [Streamline Application Shipping with Docker: Linux and Windows Made Easy!](https://medium.com/@adarsh_d/streamline-application-shipping-with-docker-linux-and-windows-made-easy-78195361887)
- [ ] [【Docker 容器化】初探微服務時代的虛擬化技術](https://vocus.cc/article/64dab182fd89780001252c70)
- [ ] [Day11 認識Docker](https://ithelp.ithome.com.tw/articles/10237261)
- [ ] [A Crash Course in Docker](https://blog.bytebytego.com/p/a-crash-course-in-docker?utm_source=profile&utm_medium=reader2)

# Docker 簡介
> 2013 年，dotCloud 公司將內部開發的 Docker 開源，基於上面提到的 LXC 技術基礎，將系統設計容器轉為微服務（Micro-service）的容器！因為很火熱，dotCloud 後來索性改名叫做 Docker Inc 現在成熟的 Docker 已經不是以 LXC 為架構，而是自己開發的 Libcontainer 作為底層的技術。

## 什麼是虛擬化?
我寫好了一支程式，在我的電腦上可以正常運作，但搬到你的電腦上可能就爆掉!

會有這樣的問題是因為：每台電腦的作業系統與硬體配置不盡相同，我的程式可能剛好只跟我電腦上的環境相容。而虛擬化要做的就是模擬出一個環境，讓程式可以在不同硬體上執行時，都以為自己在同一個環境中執行。

目前常見用來比較的虛擬化技術有兩種，一種是在系統層級的虛擬化技術，在這我們叫它稱虛擬機器（Virtual machine）。另外一種則是在作業系統層級，此稱容器（Container）。前者的代表如 Virtual Box，而後者如 Docker。

## Docker Slogan
![docker slogan.png](http://192.168.25.60:8000/files/file_storage/1f77dc13.png)

## Docker 解決的問題
 
以往軟體工程師總認為，自己開發的程式應該要可以在各種作業環境上順利運行，但往往因為作業系統、環境變數等等的不同，導致許多未知的問題。Docker的出現，讓軟體工程師只要專注於自身產品的開發，而不需費神於處理作業環境上的種種問題。

Docker 是個輕量級的虛擬化技術，可以把你的應用程式連同環境一起打包，部屬的時候就不用再擔心環境的問題。

以下例子就是使用 Docker 將三個已經打包起來的程式跑在不同的 container（容器）中，每個 container 都是一個獨立的環境，可以跑不同的系統跟安裝不同的資料庫、編譯器等等，意思就是說你可以 A 專案用 php5.3，另外一個用 php7，完全不會衝突。

![docker 將三個不同打包起來的程式跑在不同的容器中.png](http://192.168.25.60:8000/files/file_storage/5201e793.png)

> 只要該作業系統能夠安裝 Docker Engine，就可以保證開發的軟體可順利在該 OS 上執行。
{.is-info}
  
![Dockerizing a production ready MERN (MongoDB Express React Nodejs) application.png](http://192.168.25.60:8000/files/file_storage/8e57f856.png)  
  
## Docker 怎麼做到的
1. 在作業系統上架設 Docker Engine
2. 將應用程式打包成「標準化單元」，在 Docker Engine 上運行
 
![docker 怎麼做到的.png](http://192.168.25.60:8000/files/file_storage/59b94750.png)
 
![docker 怎麼做到的.png](http://192.168.25.60:8000/files/file_storage/59b94750.png) 

## Docker 跟傳統的虛擬化方式相比具有眾多的優勢
- Docker container 的啟動可以在秒級實作，這相比傳統的虛擬機方式要快得多。
- Docker 對系統資源的使用率很高，一台主機上可以同時執行數千個 Docker container。

## Docker 在以下幾個方面具有較大的優勢
- 更快速的交付和部署
- 更有效率的虛擬化
- 更輕鬆的遷移和擴展
- 更簡單的管理

## Docker 可以做什麼？
- 一致性的發佈環境：
 讓開發和發佈有統一的標準環境，這有利於持續性整合與發佈continuous integration and continuous delivery (CI/CD) 工作流程。開發者撰寫程式碼，並運行在開發者的電腦的容器中，而其它部門也使用容器運行他們的應用程式。當所有的應用程式協同運作、測試都沒問題後，就可以發佈在同樣讓容器運行的正式環境中。若在測試中有任何容器內的程式有錯誤，只要修正部分程式碼，又可以快速的重新整合後發佈。
 
- 可攜性發佈和按需式縮放( scaling )：
容器可以容易地運行在不同的環境，像是開發者電腦、虛擬機器、雲端環境或是雲端和本地的混合環境。因為容器的可攜性和輕量級地運行，我們可以輕易的按照商業需求擴張和縮小應用程式的實體數量。

- 在同一台機器運行更多的工作：
容器提供輕量級的沙盒環境，在同一台機器可以運行多個容器共享硬體資源，不像虛擬機器需要模擬整個主機，它所需要的資源更多。

- 基礎設施即代碼( Infrastructure as Code )及軟體定義網路：
Docker 的架構中提供 REST API 和客戶端 Docker CLI 來控制 Docker 物件，像是：images、containers、networks和 volumes。因此，各個容器的協同運作、網路、檔案系統都是可以被軟體定義、控制，像是用 Docker Compose File 和 Swarm。舉個實務上常常看到的例子：

- 代碼修改網頁伺服器應用程式所監聽的 port 和掛載它所需的資料。
- 代碼控制應用程式副本的縮放。
- 因為代碼控制基礎設施，所以代碼可以保存基礎設施的設定也可以進行版本控制。

# Docker 最重要的觀念
用一個簡單的比喻來解釋，如果映像檔是一個做蛋糕的模具，容器則是用該模具烤出來的蛋糕，而倉庫是用來集中放置模具們的收納櫃。接下來讓我們搭配這個比喻深入一點解釋這三個概念

![docker Docker Life Circle.png](http://192.168.25.60:8000/files/file_storage/c646d4c8.png)

![docker example.png](http://192.168.25.60:8000/files/file_storage/ed4a9423.png)

![組成 Docker 的重要組件.png](http://192.168.25.60:8000/files/file_storage/b876eafd.png)

![docker 的架構.png](http://192.168.25.60:8000/files/file_storage/99230c15.png)

![Docker Architecture.gif](http://192.168.25.60:8000/files/file_storage/f108687a.gif)

執行 Docker 的主機，稱為 Host。

在 Host 執行 docker run 指令時，Docker 會操作這三個元件來完成任務。首先來了解這三個元件

## 映像檔 (Image)

![docker image.png](http://192.168.25.60:8000/files/file_storage/189ca195.png)

- 利用 Docker Engine 將「應用系統」打包的一個唯讀單元
- Docker 映像檔是一個模板，用來重複產生容器實體。例如：一個映像檔裡可以包含一個完整的 MySQL 服務、一個 Golang 的編譯環境、或是一個 Ubuntu 作業系統。
- 透過 Docker 映像檔，我們可以快速的產生可以執行應用程式的容器。而 Docker 映像檔可以透過撰寫由命令行構成的 Dockerfile 輕鬆建立，或甚至可以從公開的地方下載已經做好的映像檔來使用。
- 舉例來說，如果我今天想要一個 node.js 的執行環境跑我寫好的程式，我可以直接到上 DockerHub 找到相對應的 node.js 映像檔 ，而不需要自己想辦法打包一個執行環境。

## 容器 (Container)

![docker container.png](http://192.168.25.60:8000/files/file_storage/dcf24b1c.png)

- 利用映像檔建立的一個執行實例 (runtime instance)，一個映像檔可建立多個容器
- 就像是用蛋糕模具烤出來的蛋糕本體，容器是用映像檔建立出來的執行實例。它可以被啟動、開始、停止、刪除。每個容器都是相互隔離、保證安全的平台。
- 可以把容器看做是一個執行的應用程式加上執行它的簡易版 Linux 環境（包括 root 使用者權限、程式空間、使用者空間和網路空間等）。
- 另外要注意的是，Docker 映像檔是唯讀（read-only）的，而容器在啟動的時候會建立一層可以被修改的可寫層作為最上層，讓容器的功能可以再擴充。這點在下面的實例會有更多補充。

## 倉庫 (Registry)

![docker repository.png](http://192.168.25.60:8000/files/file_storage/32f21560.png)

- 存放映像檔的地方，分為公用倉庫及私有倉庫
- 倉庫（Repository）是集中存放映像檔檔案的場所，也可以想像成存放蛋糕模具的大本營。倉庫註冊伺服器（Registry）上則存放著多個倉庫。
- 最大的公開倉庫註冊伺服器是上面提到過的 Docker Hub，存放了數量龐大的映像檔供使用者下載，我們可以輕鬆在上面找到各式各樣現成實用的映像檔。
- 而 Docker 倉庫註冊伺服器的概念就跟 Github 類似，你可以在上面建立多個倉庫，然後透過 push、pull 的方式上傳、存取。 

![docker 最重要的觀念.png](http://192.168.25.60:8000/files/file_storage/fe845149.png)

Image、container、repository 之間的關係就像光碟一樣：早期世紀帝國等光碟遊戲，會需要搭配其他可讀寫空間（如硬碟），才有辦法執行。

Image 像光碟片一樣，唯讀且不能獨立執行
Container 像硬碟一樣，可讀可寫可執行
Repository 則是光碟盒，而 Registry 則是像光碟零售商

## Dockerfile

![docker dockerfile.png](http://192.168.25.60:8000/files/file_storage/ddd0f5b6.png)

這個部份主要在描述映像檔( Image )的組成方式， Docker 就會依照我們的描述去產生映像檔( Image )，那開發者可以根據不同的應用程式製作不一樣的映像檔。

我們只要記住 Dockerfile 等於是一份汽車製造設計圖，而車廠根據設計圖進行工程施作，最終打造出一台汽車也就是映像檔，待駕駛上坐之後開始運行，因此汽車相當於是載具，擔任乘客的容器。

但 Dockerfile 其實並沒有這麼簡單，我們後續也會根據 Dockerfile 撰寫專門的一篇來進行介紹。

# Docker 原理
## Docker 與 Virtual Machine 的差異

![docker Docker 與 Virtual Machine 的差異.png](http://192.168.25.60:8000/files/file_storage/08275957.png)

### 虛擬機器 Virtual Machine 
- 以作業系統為中心，虛擬「作業系統及應用程式」，耗費較多資源
- 虛擬化的目標：將一個應用程式所需的執行環境打包起來，建立一個獨立環境，方便在不同的硬體中移動
- 虛擬機器是在系統層上虛擬化，透過 Hypervisor 在目標的機器上提供可以執行一個或多個虛擬機器的平台。而這些虛擬機器可以執行完整的作業系統。簡單來說，Hypervisor 就是一個可以讓你在作業系統（Host OS）上面再裝一個作業系統（Guest OS），然後讓兩個作業系統彼此不會打架的平台。
- 透過選擇不同的 Guest OS，虛擬機器的技術就可以確保只要我的程式在該 Guest OS 上可以正常運作，那放到你的電腦上跑時，可以不管你的 Host OS 是什麼，只要在你的 Host OS 上先裝上我的 Guest OS，我的程式就可以正常在你的電腦上運作。
### 容器 Docker Container 
- 以應用程式為中心，虛擬「應用程式」，相對節省資源。在建立及重啟時，所需的反應時間也較少
- 容器化的目標：改善虛擬機器因為需要裝 Guest OS 導致啟動慢、佔較大記憶體的問題。
- 容器是在作業系統層上虛擬化，透過 Container Manager 直接將一個應用程式所需的程式碼、函式庫打包，建立資源控管機制隔離各個容器，並分配 Host OS 上的系統資源。透過容器，應用程式不需要再另外安裝作業系統（Guest OS）也可以執行。
- 因為不需要另外安裝作業系統，建立容器所需要的硬碟容量可以大幅降低，且啟動速度可以更快，不需要等待 Guest OS 的開機時間。

> 虛擬機器擁有獨立 OS ，但 Docker 容器卻是共用 Linux 核心模組。
{.is-info}


 
## Docker 共用 Linux 核心模組是什麼意思
Linux 的 2 個核心模組，namespace 及 cgroup 使得本機上的 process 得以擁有隔離性，也就是所謂的容器。
Namespace 隔離各項資源（PID、網路等等），使得容器內的 process 得以被隔離在自己的空間內。cgroup 則用來分配硬體資源。
既然 Docker 共用本機上的 Linux kernel ，那也就代表 Docker 提供的 Linux 映像檔不需要包含 Linux kernel ，事實上各種 Linux 版本的基底映像檔（例如：Ubuntu, CentOS, Fedora, Debian..）都只有包含目錄結構、操作系統庫（例如：/bin, /sbin底下的shell命令）、/lib 底下的依賴庫、套件管理系統（例如：apt, yum....）。

![docker Linux Containers.png](http://192.168.25.60:8000/files/file_storage/5bcde4d6.png)

## Docker Daemon 與 Docker Client
- Docker client 對 Docker 下命令的使用者
- Docker daemon 是 Docker 常駐程式，負責執行指令，視為 Docker server 端
- Libcontainer 用來與 Linux kernel 溝通

![docker Docker Daemon 與 Docker Client.png](http://192.168.25.60:8000/files/file_storage/f034032f.png)

## Daemon/Client/Linux kernel 溝通過程

Client 下令建立容器 ➔ Docker Daemon 接收到指令 ➔ Libcontainer 通知 Linux kernel 建立容器 ➔ Linux kernel 建立獨立空間，擁有獨立的 namespace 及 cgroup ➔ 把映像檔的內容放進容器 ➔ 啟動容器


## Docker 如何運作的

![How does Docker work.png](http://192.168.25.60:8000/files/file_storage/f9fdb01e.png)



