---
title: Casbin
description: Casbin 簡介
published: true
date: 2023-09-14T05:41:28.317Z
tags: authorization, casbin
editor: markdown
dateCreated: 2023-09-05T01:16:42.899Z
---

# Casbin
- [ ] [[NestJS 帶你飛！] DAY25 - Authorization & RBAC](https://ithelp.ithome.com.tw/articles/10279982)
- [ ] [Casbin is All You Need —— 存取控制框架 Casbin Ramp Up](https://zhuanlan.zhihu.com/p/533586189)

![casbin logo.png](http://192.168.25.60:8000/files/file_storage/c32e5ba5.png)

# Casbin 心智圖

![casbin.png](https://edrawcloudpublicus.s3.amazonaws.com/viewer/self/2742053/share/2023-9-5/1693876952/main.svg)

# Casbin 源起
## 存取控制
> 存取控制，顧名思義，是指判斷一條請求是否可以訪問受保護的資源的技術。

![casbin 存取控制.png](http://192.168.25.60:8000/files/file_storage/7808b4bc.png)

在上圖的例子中，我們的後台中有兩個資源， Resource1 和 Resource2 。它們可以是伺服器、帳號、圖片、視訊等等等等。但是，它們的相同特性是不能被所有使用者都訪問。

比如 Resource1 屬於使用者 Alice，那麼只有 Alice 能夠訪問它，Bob 則不能。因此，我們就需要對訪問請求進行過濾，判斷其是否被允許到達目標資源。在上面的例子中，Alice 發起了兩個訪問請求，分別想要訪問 Resource1 和 Resource2 。存取控制層需要做的工作就是允許訪問 Resource1 的請求通過，而阻攔想要訪問 Resource2 的請求，因為 Resource2 屬於 Bob，Alice 是無法訪問的。

## 原理
> Casbin 的底層原理基於其建立者羅楊博士所發表的一篇論文 《PML: An Interpreter-Based Access Control Policy Language for Web Services》

### 設計目標
這篇論文主要關注於如何解決現實中雲服務廠商有關權限校驗所遇到的兩個問題：

每個雲服務廠商都有著自己的一套權限檢驗規則。這對於在多個雲環境都進行部署的使用者來說，造成了很大的遷移和維護成本。
同樣的，維護自己的一套權限校驗規則對於雲服務廠商來說，也是一個挑戰。如果雲服務廠商缺乏在這方面的相關經驗，就很有可能造成安全漏洞。
既然文章的目標本質上是通過通用性來解決問題，作者也考慮了如何實現這個目標，提出了兩個 independent 的設計要求：

- Access Control Model Independent：PML 既需要支援使用者可以在多個雲服務廠商中使用同一個模型，也需要支援使用者在不改變校驗程式碼的同時，可以切換不同的模型。
- Implementation Language Independent: PML 的設計不應該依賴於某種程式語言的特性。 因此，文中提出了一種新的權限校驗語言——PML (PERM Modeling Language)，希望通過一種支援多種權限校驗模型的組態語言來彌補這個 gap。

### 設計實現
在介紹 PML 的設計之前，我們可以先大致瞭解一下存取控制問題中，所涉及到的一些概念。

![casbin policy and model.png](http://192.168.25.60:8000/files/file_storage/4ba1becf.png)


一般來說，存取控制會涉及到兩個部分：

1. Model 存取控制模型。常見的模型有 ACL（Access Control List 存取控制列表），RBAC（Role-Based Access Control 基於角色的存取控制），ABAC（Attribute-Based Access Control 基於屬性的存取控制）。對於一個應用來說，Model 的選擇是與應用的業務邏輯是密切相關的，因此也是相對靜態的。一旦程式碼編譯完成，這部分是不會隨著應用的運行而產生變化的。
2. Policy 存取控制規則。Policy 是和 Model 相對應的，每種不同的 Model，都會有不同格式的 Policy。而與 Model 完全相反的是，Policy 是相對動態的。在編寫程式碼的過程中，我們只能去定義 Policy 的格式，而 Policy 的具體內容都是應用運行過程中新增或修改的。例如，有一個新使用者註冊了我們的應用。那麼，我們就需要動態的為其新增一條 Policy。

我們可以將這兩部分理解為傳統應用中的程式碼和資料。有了這兩部分後，再加上使用者特定的校驗邏輯，那麼就可以完成存取控制任務。

### PERM 模型
當然，對於現實環境中複雜的情況，簡單地將問題建模為這兩部分肯定是不夠的，因此，論文提出了一個新的元模型 PERM（Policy-Effect-Request-Matcher）。

![casbin PERM 模型.png](http://192.168.25.60:8000/files/file_storage/db27079d.png)

PERM 模型主要包含了 6 個主要的概念：

- Request：訪問請求定義。使用者真實的訪問請求，通常會包含 sub（訪問者），obj（被訪問的資源）， act（訪問時所進行的操作）或其他使用者自訂的屬性。
- Policy：存取控制規則定義。定義了需要對訪問請求的哪些屬性進行校驗。
- Policy Rule：存取控制規則實例。
- Matcher：如何為一條 Request 匹配到其對應的 Policy Rule。
- Effect：當一條 Request 匹配到了一條或多條 Policy Rule，如何判斷其是否應該被允許。
- Stub Function：在實際應用中，Request 實例 和 Policy Rule 的匹配往往無法通過簡單的 == 等於來解決，例如萬用字元等等。所以 Stub Function 允許使用者自訂一些複雜的匹配方法。

這六個更加詳細的建模了存取控制的問題。我們也可以對其簡單的分一下類，Request，Policy，Matcher，Effect 和 Stub Function 都是靜態的，屬於 Model 的一部分。通過這個五項的組合，ACL 等常見的模型以及一些使用者自訂的規則，都可以很輕易的表示出來。在最後一部分中，會以 RBAC 為例，介紹如何通過 PML 實現這樣一個模型。

而 Policy Rule 就屬於動態變化的內容。在實際實現中，往往也是像資料一樣，儲存在資料庫當中的。

## 結構
與論文中的實現相比，目前 Casbin 的實現更加強大，支援了更多功能。所以，這裡以 Casbin 主庫（Go 版本），介紹 Casbin 是如何進行工作的。

![casbin 結構.png](http://192.168.25.60:8000/files/file_storage/2647b9ca.png)

1. 在應用啟動時，Casbin 會讀取使用者已經定義好的 Model，其中會包含 Request, Policy, Matcher 和 Effector 四個部分的定義。同時，Casbin 會利用 Adapter，從資料來源處讀取 Policy 實例（也就是上文提到的 Policy Rule）。後文就將 Policy 實例簡稱為 Policy。
2. 對於 Policy 的儲存和讀取，Casbin 將其解耦到了獨立的 Adapter 模組。通過使用不同的 Adapter（File, MySQL等等），可以從不同的資料來源中讀取 Policy。對於 Policy 比較多的場景，將所有的 Policy 同時載入進記憶體，確實會導致一定的性能損失。所以，在載入時，部分 Adapter 也提供 LoadFilteredPolicy 的介面，通過只載入 Policy 的一部分子集，減少這部分帶來的性能瓶頸。
3. 在一條請求到來時，該請求首先會按照 Model 中的定義進行拆分。接下來，Matcher 會根據 Model 中定義的規則，與 Policy 進行匹配。除了支援 == 強匹配外，Matcher 還支援通過 Function 和 Role Manager 進行模糊匹配。Function 像使用者提供了自訂匹配規則的介面。通過向 Matcher 傳入自訂函數，Matcher 可以對 Request 與 Policy 之間進行一些複雜的匹配。
4. 對於 RBAC 等存取控制模型，除了單純的使用者與權限之間存在關係之外，使用者與角色（Role）之間還存在著繼承關係。Casbin 中採用了 Role Manager 來為一條 Request 的使用者以及其對應角色（包含繼承角色）尋找與其相關的 Policy。同時，Role Manager 也支援新增自訂的 Function，來對使用者與角色之間進行複雜的匹配。
5. 在實際應用中，一條 Request 可能會匹配到多條 Policy。得到所有的 Policy 後，需要進一步將多條 Policy 的結果進行聚合，得到最終是否允許 Request。Effector 根據 Model 中組態的規則，對所有 Matched Policy 的 effect 項進行進一步的 eval。
6. 在很多場景下，存取控制服務會有多個實例。Casbin 支援對 Policy 進行增量更新，那麼，就需要 Dispatcher 維護多個 Casbin 實例的 Policy 之間的一致性。Dispatcher 主要提供兩部分的功能，一部分是 Casbin 的 API，另一部分是 Dispatcher 自身的 API，用來實現成員管理等一致性問題，可以通過 Raft 等共識演算法實現。

## 工作流

![Workflow of Casbin.png](http://192.168.25.60:8000/files/file_storage/ce53db80.png)

# Casbin 概念
> Casbin 由兩部分所組成：**存取控制模型**、**政策模型**

## 存取控制模型 (Access Control Model)
存取控制模型簡單來說就是用來定義怎麼做驗證的地方，也就是驗證規則的制定。在 Casbin 我們會製作一個 `model.conf` 的設定檔，它是基於 PERM 模型 來進行組態，讓驗證規則只需要用一個設定檔就可以解決，那什麼是 PERM 模型呢？他們分別是這四個元素：請求 (Request)、政策 (Policy)、驗證器 (Matcher)、效果 (Effect)，不過，RBAC 還會多一種叫 角色定義 (Role Definition) 的元素。

### 請求 (Request)
> 定義驗證時所需使用的參數與順序，必須包含：主題/實體 (Subject)、對象/資源 (Object) 以及 操作/方式 (Action)。

請求的範例格式如下：

```
[request_definition]
r = sub, obj, act
```

上述範例的各參數意義如下：

- `[request_definition]`：定義請求時需要以此作為開頭。
- `r`：變數名稱，因為定義了 `[request_definition]`，該變數就代表了請求。
- `sub`：代表主題，通常主題可以是使用者、角色等。
- `obj`：代表對象，通常對象可以是資源等。
- `act`：代表操作，通常操作會是針對資源所執行的動作名稱。

用比較白話文的方式來解釋的話，可以說成：

- `請求(r)` 提供了「 `誰(sub)` 想要對 `什麼東西(obj)` 做 `什麼動作(act)`」的資訊。

我們將這段話帶入 RBAC 的概念來重新解釋：

- `請求(r)` 提供了「 `角色(sub)` 想要對 `某個資源(obj)` 做 `特定操作(act)`」的資訊。


### 政策 (Policy)
> 定義政策模型的骨架，使未來可以依照該骨架來制定政策模型。

政策的範例格式如下：

```
[policy_definition]
p = sub, obj, act, eft
```

上述範例的各參數意義如下：

- `[policy_definition]`：定義政策時需要以此作為開頭。
- `p`：變數名稱，因為定義了 `[policy_definition]`，該變數就代表了政策。
- `sub`：代表主題。
- `obj`：代表對象。
- `act`：代表操作。
- `eft`：代表 允許(`allow`) 或 拒絕(`deny`)，非必要項目，預設值為 `allow`。

用比較白話文的方式來解釋的話，可以說成：

- `政策(p)` 制定了「`誰(sub)`  `可不可以(eft)` 對 `什麼東西(obj)` 做 `什麼動作(act)`」的規則描述。

我們將這段話帶入 RBAC 的概念來重新解釋：

- `政策(p)` 制定了「 `某個角色(sub)` `可不可以(eft)` 對 `某個資源(obj)` 做 `特定操作(act)`」的規則描述。

### 驗證器 (Matcher)
> 驗證請求帶來的資訊是否與政策模型制定的規則吻合，是一個條件敘述式，在執行驗證流程時，會將請求與政策模型的值帶入進行驗證。

驗證器的範例如下：

```
[matchers]
m = r.sub == p.sub && r.act == p.act && r.obj == p.obj
```

上述範例的各參數意義如下：

- `[matchers]`：定義驗證器時需要以此作為開頭。
- `m`：變數名稱，因為定義了 `[matchers]`，該變數就代表了驗證器。
- `r`：變數名稱，前面已經透過 `[request_definition]` 將它宣告成請求。
- `p`：變數名稱，前面已經透過 `[policy_definition]` 將它宣告成政策。
- `r.sub`：代表請求的主題。
- `p.sub`：代表政策的主題。
- `r.obj`：代表請求的對象。
- `p.obj`：代表政策的對象。
- `r.act`：代表請求的操作。
- `p.act`：代表政策的操作。

以上方範例來說，用比較白話文的方式來解釋可以說成：

- `請求主題(r.sub)` 必須與 `政策主題(p.sub)` 相同、`請求操作(r.act)` 必須與 `政策操作(p.act)` 相同以及 `請求對象(r.obj)` 必須與 `政策對象(p.obj)` 相同。

### 效果 (Effect)
> 針對驗證結果再進行一個額外的驗證。

效果的範例如下：

```
[policy_effect]
e = some(where (p.eft == allow))
```

上述範例的各參數意義如下：

- `[policy_effect]`：定義效果時需要以此作為開頭。
- `e`：變數名稱，因為定義了 `[policy_effect]`，該變數就代表了效果。
- `p.eft`：政策的許可值。
- `allow`：`eft` 的結果之一。

以上方範例來說，用比較白話文的方式來解釋可以說成：

- 在驗證結果中，只要有一個政策許可值為 `allow` 就表示通過。

### 角色定義 (Role Definition)
> 用來實現角色繼承的定義，不是必要的組態項目。

下方為角色定義的範例：

```
[role_definition]
g = _, _
```

上述範例的各參數意義如下：

- `[role_definition]`：定義角色定義時需要以此作為開頭。
- `g`：變數名稱，因為定義了 `[role_definition]`，該變數就代表了角色定義。

在範例中可以看到 `_, _` 這樣的組態，這個意思是前項的角色將會繼承後項角色的權限，可以運用這個方式來綁定角色和資源的關係。後面會針對這塊做更完整的實作範例與解說。

## 政策模型 (Policy Model)
> 政策模型是制定角色與資源存取關係的地方，也就是哪些角色可以對哪些資源做哪些操作的明確定義。在 Casbin 中最簡單的實作方法就是制定 `policy.csv` 檔，當然，也可以透過資料庫來維護這些定義，本篇將會以 csv 檔的方式進行介紹與呈現。

### 定義模型
> 定義模型的方法很簡單，還記得前面我們定義政策為 `p` 並且 `p = sub, obj, act` 嗎？我們只要根據這個骨架進行組態即可，需特別注意的是開頭必須是指定的政策變數。

下方為一個簡單的模型定義：

```
p, role:staff /todos read
```

可以看到我們使用政策 `p` 來定義模型，該模型的 `sub` 為 `role:staff`、`obj` 為 `/todos`、`act` 為 `read`，完全呼應了 `sub` 為角色、`obj` 為資源、`act` 為操作資源的動作。

### 角色繼承
如果我們有一個新的角色叫 `role:manager`，他同時也是 `role:staff` 的一份子，這要如何實現角色繼承呢？前面有提到角色定義可以辦到，透過使用 `g` 作為開頭並且將繼承的角色放在前項、被繼承的角色放在後項：
```
p, role:staff /todos read
p, role:manager /todos write
g, role:manager role:staff
```

這樣在政策模型上他們就是繼承關係了，但還有一個地方需要去調整，就是我們前面提到的驗證器，它也必須去調用 `g` 來為 `sub` 做匹配：

```
m = g(r.sub, p.sub) && r.act == p.act && r.obj == p.obj
```

# Casbin 使用
在 Casbin 中，model、adapter 和 enforcer 是三個不同的概念，分別代表以下含義：

## Model
Model（模型）定義了 Casbin 的策略結構。模型是 Casbin 中的核心組件之一，它定義了策略（Policy）規則和角色的基本結構。模型描述了策略如何組織，包括規則類型、角色、資源和操作等。它通過定義策略規則的格式和內容，提供了一種統一的方式來表示和管理訪問控制邏輯。

## Adapter
Adapter（適配器）負責從外部資源（如數據庫、文件或API）中加載策略數據。適配器是 Casbin 中用於將策略數據存儲到持久化存儲介質（例如數據庫）或從持久化存儲介質中讀取策略數據的介面。適配器負責將策略數據轉換為 Casbin 可理解的格式，並提供對策略的增刪改查操作。Casbin 提供了多種適配器實現，可以與不同的存儲介質（如 MySQL、PostgreSQL、Redis 等）進行交互。

## Enforcer
Effector（執行器）負責根據策略結果決定是否允許訪問。執行器是 Casbin 的核心組件之一，它負責實際執行訪問控制邏輯。執行器接收輸入的請求，並根據事先定義的策略規則進行評估，來決定是否允許該請求的執行。執行器檢查請求中的角色、資源和操作，並使用模型和適配器進行策略驗證。如果符合策略規則，執行器則允許該請求的執行，否則拒絕該請求。

## Watcher
Watcher（監視器）負責監視策略的更改。它可以用於在策略發生更改時自動重新加載策略數據，以確保 Casbin 始終使用最新的策略。

## Role Manager
Role Manager（角色管理器）負責對角色進行管理和操作。它提供了添加、刪除和查詢角色的方法，並為 Casbin 提供了角色相關的功能。

## Dispatcher
Dispatcher（調度器）負責將請求分發給 Casbin 的其他組件。它協調不同組件之間的通信和操作。

總結來說，Model 定義了策略的結構，Adapter 用於存儲和讀取策略數據，Enforcer 負責執行訪問控制邏輯。這三個組件共同協作，實現了 Casbin 的訪問控制功能。

# Casbin 功能
## Casbin 有的功能
- 遵循經典的 {主體，對象，操作} 形式或您定義的自定義形式來執行策略，支持允許和拒絕授權。
- 處理訪問控制模型及其策略的存儲。
- 管理角色-用戶映射和角色-角色映射（也稱為 RBAC 中的角色層次結構）。
- 支持內置的超級用戶，如 `root` 或 `administrator`。超級用戶可以在沒有明確權限的情況下執行任何操作。
- 提供多個內置操作符來支持規則匹配。例如，`keyMatch` 可以將資源鍵 `/foo/bar` 映射到模式 `/foo*` 。

## Casbin 不具備以下功能
- 認證（即驗證用戶名和密碼當用戶登錄時）。
- 管理用戶或角色列表。我認為由項目本身來管理這些實體更方便。通常，用戶擁有自己的密碼，而 Casbin 並非設計為密碼容器。然而，Casbin 會在 RBAC 情境中存儲用戶-角色映射。
