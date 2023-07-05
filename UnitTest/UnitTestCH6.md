---
title: Unit Test CH6
description: The Art of Unit Testing Third Edition v7
published: true
date: 2022-07-18T06:15:55.571Z
tags: unit test
editor: markdown
dateCreated: 2022-07-13T02:13:22.296Z
---

# CH6 Unit Testing Async Code
> - 非同步 `Async` done() 與 等待 `awaits`
> - 非同步 `Async` 的整合與單元測試
> - 抽取出進入點模式 `Entry Point Pattern`
> - 抽取出轉換器模式 `Adapter Pattern`
> - 模擬物件、進階的與重置的時間
> - 可觀察的、堅硬的的單元測試

## 6.1 處理非同步方式取得資料
> Dealing with Async Data Fetching

> [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise)
{.is-info}

![Unit Test IsWebsiteAlive callback vs async await version.png](http://192.168.25.60:8000/files/file_storage/daeaba94.png)

```javascript
// fetching-samples-before.js 
const fetch = require("node-fetch");

const isWebsiteAliveWithCallback = (callback) => {
  const website = "http://example.com";
  fetch(website)
    .then((response) => {
      if (!response.ok) {
        // how can we simulate this network issue?
        // we throw custom errors to have a single palce where we handle problems in our code
        throw Error(response.statusText);
      }
      return response;
    })
    .then((response) => response.text())
    .then((text) => {
      if (text.includes("illustrative")) {
        callback(null, { success: true, status: "ok" });
      } else {
        //how can we test this path?
        throw new Error("text missing");
      }
    })
    .catch((err) => {
      //how can we test this exit point?
      callback(err);
    });
};
const isWebsiteAliveWithPromises = async () => {
  try {
    const resp = await fetch("http://example.com");
    if (!resp.ok) {
      // we throw custom errors to have a single palce where we handle problems in our code
      throw new Error(resp.statusText);
    }
    const text = await resp.text();
    const included = text.includes("illustrative");
    if (included) {
      return { success: true, status: "ok" };
    }
    // we throw an error to denote a failure to the user of our async/await function.This way we enable the user of the async/await version to use try/catch or thn/catch syntax.
    throw new Error("text missing");
  } catch (err) {
    throw err;
  }
};

module.exports = {
  isWebsiteAliveWithCallback,
  isWebsiteAliveWithPromises,
};
```

### 6.1.1 開始嘗試一個整合測試

- 測試一個出口點為一個 callback 函式的函式
- 透過確認傳遞值的正確性
- 利用測試框架告知測試什麼時候停止等待，此處使用的是 done( )

```javascript
// fetching-samples-before.integration.spec.js
// An initial integration test
const samples = require("./fetching-samples-before");

test("NETWORK REQUIRED (callback): website with correct content, returns true", (done) => {
  samples.isWebsiteAliveWithCallback((err, result) => {
    expect(err).toBeNull();
    expect(result.status).toBe("ok");
    expect(result.success).toBe(true);
    done();
  });
});
```

### 6.1.2 等待一個動作

- 在 AAA 模式， act 是我們必須要等待的一個環節
- 使用 done( )

### 6.1.3 非同步 `Async` 與 等待 `awaits` 的整合測試
- callbacks 與 .then() 的整合測試
```javascript
// fetching-samples-before.integration.spec.js
// Integration test with callbacks and .then()
const samples = require("./fetching-samples-before");

test("NETWORK REQUIRED (await): website with correct content, returns true", (done) => {
  samples.isWebsiteAliveWithPromises().then((result) => {
    expect(result.success).toBe(true);
    expect(result.status).toBe("ok");
    done();
  });
});
```

- async/await 的整合測試

```javascript
// fetching-samples-before.integration.spec.js
// Integration test with async/await
const samples = require("./fetching-samples-before");

test("NETWORK REQUIRED2 (await): website with correct content, returns true", async () => {
  const result = await samples.isWebsiteAliveWithPromises();
  expect(result.success).toBe(true);
  expect(result.status).toBe("ok");
});
```

### 6.1.4 整合測試的困難
- 執行時間：相比單元測試，整合測試可能需要耗時幾秒或幾分鐘
- 片段式的 `Flaky`：測試呈顯出不一致的結果(不同的東西會基於什麼地方跑，會有不一致的成功或失敗結果)
- 測試到不重要的程式碼或環境(例如：網路、 node-fetch library、防火牆、外部網站等等)
- 需較長的調查時間：當測試失敗時，需要較多時間去調查及除錯，基於很多可能失敗的理由
- 難以模擬：例如模擬錯誤的網站內容、網路斷線、網站斷線
- 難以信任結果：我們有可能基於外部問題而相信錯誤，但實際上可能是我們程式碼有錯誤

## 6.2 使單元測試更友善
> Making our code unit test friendly

- 抽取出進入點：抽取出進入到函式為純邏輯的部分，將這些視為進入到測試的進入點
- 抽取出轉換器：抽取出繼承異步 `asynchronous`的部分且抽象化他，如此當有東西是異步的時候我們可以取代他 

### 6.2.1 抽取出工作單元中的邏輯

![Unit Test Extracting Entry Point Pattern.png](http://192.168.25.60:8000/files/file_storage/dbc1acd3.png)

- Before：工作單元裡面包含了異步程式碼還有執行非同步的內部結果，並回傳一個 callback 或 promise
- Step1：抽取出包含只有非同步的結果當作進入點的邏輯
- Step2：將函式提到外部，在單元測試中可以使用它們當作進入點

### 6.2.2 使用 callbacks 應用到抽取出進入點

![Unit Test Extracting Entry Points from isWebsiteAlive().png](http://192.168.25.60:8000/files/file_storage/e7d16440.png)

- 抽取出抓取結果的邏輯，並分配到不同的函式，一個處理成功結果 processFetchSuccess( )，一個處理失敗結果 processFetchError( )
- 將兩個函式可以提供給外部使用，這樣在測試的時候就可以調用他們

```javascript
// fetching-samples-callback.js
const fetch = require("node-fetch");

const isWebsiteAlive = (callback) => {
  fetch("http://example.com")
    .then(throwOnInvalidResponse)
    .then((resp) => resp.text())
    .then((text) => {
      processFetchSuccess(text, callback);
    })
    .catch((err) => {
      processFetchError(err, callback);
    });
};

const throwOnInvalidResponse = (resp) => {
  if (!resp.ok) {
    throw Error(resp.statusText);
  }
  return resp;
};

//Entry Point
const processFetchSuccess = (text, callback) => {
  if (text.includes("illustrative")) {
    callback(null, { success: true, status: "ok" });
  } else {
    callback(new Error("missing text"));
  }
};

//Entry Point
const processFetchError = (err, callback) => {
  callback(err);
};

module.exports = {
  isWebsiteAlive,
  processFetchSuccess,
  processFetchError,
};
```

![Unit Test new entry points introduced.png](http://192.168.25.60:8000/files/file_storage/6a2765af.png)

- 可以直接調用新的進入點
- 可以更容易的模擬不同的情況
- 在這個測試當中沒有非同步的情況，但我們還是得需使用 done( )，因為 callbacks 可能不會被調用，但我們會希望去抓取到

```javascript
// fetching-samples-callback.unit.spec.js
const samples = require("./fetching-samples-callback");

describe("Website alive checking", () => {
  test("When website fetch content matches, returns true", (done) => {
    samples.processFetchSuccess("illustrative", (err, result) => {
      expect(err).toBeNull();
      expect(result.success).toBe(true);
      expect(result.status).toBe("ok");
      done();
    });
  });

  test("When website fetch content does not match, returns false", (done) => {
    samples.processFetchSuccess("text not on the website", (err, result) => {
      expect(err.message).toBe("missing text");
      done();
    });
  });
  test("When fetch fails, returns false", (done) => {
    samples.processFetchError(new Error("error text"), (err, result) => {
      expect(err.message).toBe("error text");
      done();
    });
  });
});
```

```javascript
// fetching-samples-callback.integration.spec.js
const samples = require("./fetching-samples-callback");

test("isWebsiteAlive with real website returns true", (done) => {
  samples.isWebsiteAlive((err, result) => {
    expect(err).toBeNull();
    expect(result.success).toBe(true);
    done();
  });
});
```

### 6.2.3 使用等待抽取出進入點

![Unit Test extract entry points with async await.png](http://192.168.25.60:8000/files/file_storage/542722d0.png)

- 使用 async/await，可以寫成一個線形的模式，一行代表一個區塊，當需要的時候只回傳值或拋出錯誤

```javascript
// fetching-samples-await.unit.spec.js
const samples = require("./fetching-samples-await");
describe("website up check", () => {
  test("on fetch success with good content, returns true", () => {
    const result = samples.processFetchContent("illustrative");
    expect(result.success).toBe(true);
    expect(result.status).toBe("ok");
  });
  test("on fetch success with bad content, returns false", () => {
    const result = samples.processFetchContent("text not on site");
    expect(result.success).toBe(false);
    expect(result.status).toBe("missing text");
  });
  test("on fetch fail, throws ", () => {
    expect(() => samples.processFetchError("error text"))
     .toThrowError("error text");
  });
});
```
- 不同於 callback 的案例，我們使用回傳或拋出來區分成功與失敗
- 這是一個基於 async/await 很基本的模式寫法
- 此處我們不需要直接寫出 async/await 等關鍵字，就能達到等待的效果，因為我們已經將非同步的邏輯從工作單元抽取出來

```javascript
// fetching-samples-await.unit.spec.js
const samples = require("./fetching-samples-await");   
describe("website up check", () => {
  test("on fetch success with good content, returns true", () => {
    const result = samples.processFetchContent("illustrative");
    expect(result.success).toBe(true);
    expect(result.status).toBe("ok");
  });
  test("on fetch success with bad content, returns false", () => {
    const result = samples.processFetchContent("text not on site");
    expect(result.success).toBe(false);
    expect(result.status).toBe("missing text");
  });
  test("on fetch fail, returns error text and false", () => {
    const result = samples.processFetchError("error text");
    expect(result.success).toBe(false);
    expect(result.status).toBe("error text");
  });
});
```

## 6.3 抽取出轉換器模式
> Extract Adapter Pattern

- 抽取出轉換器模式跟之前的模式是相反看法
- 反而不是將邏輯抽取出變成一組進入點，我們抽取出非同步的程式碼，並使用轉換器進行抽象化，之後便可像其他依賴一樣進行注入

![Unit Test Extract Adapter Pattern.png](http://192.168.25.60:8000/files/file_storage/1b629069.png)

- 建立一個特別的介面，為了轉換器，以為了依賴的需要
- 這種使用方式稱為介面隔離原則 `Interface Segregation Principle` (`ISP`)
- 在此我們建立一個 network-adapter，隱藏真實抓取資料的功能，並且有自定義函式

![Unit Test Extract network-adapter.png](http://192.168.25.60:8000/files/file_storage/7eae6cbb.png)

```javascript
// network-adapter.js
const fetch = require("node-fetch");

const fetchUrlText = async (url) => {
  const resp = await fetch(url);
  if (resp.ok) {
    const text = await resp.text();
    return { ok: true, text: text };
  }
  return { ok: false, text: resp.statusText };
};

module.exports = {
  fetchUrlText,
};
```

- network-adapter 在專案裡面只是一個檔案/模組，擁有 import/require 給 node-fetch
- 當依賴在未來某個時間有異動的話，這樣會增加只需要修改這個檔案的機會
- 我們使用名稱或功能來簡化函式，我們去隱藏從 url 得到狀態或文字的需求，並使用一個單一函式去抽象化他們
- 這是一個基於 SOLID 原則的介面隔離原則

### 6.3.1 模組化的轉換器

- 用初始化 isWebsiteALive 來實作出 network-adapter 的模組化

```javascript
// website-verifier.js
const network = require("./network-adapter");

const isWebsiteAlive = async () => {
  try {
    const result = await network.fetchUrlText("http://example.com");
    if (!result.ok) {
      throw result.text;
    }
    const text = result.text;
    return processFetchSuccess(text);
  } catch (err) {
    throw processFetchFail(err);
  }
};

const processFetchSuccess = (text) => {
  const included = text.includes("illustrative");
  if (included) {
    return { success: true, status: "ok" };
  }
  return { success: false, status: "missing text" };
};

const processFetchFail = (err) => {
  return { success: false, status: err };
};

module.exports = {
  isWebsiteAlive,
};
```
- 我們直接在 website-verifier 模組中 import network-adapter
- 我們只有 export 函式 isWebsiteAlive， 因為稍後會在測試中模擬 network-adapter 模組
- 我們使用 jest.mock( ) 去模擬模組

```javascript
// website-verifier.unit.spec.js
// fake the network-adapter module
jest.mock("./network-adapter"); // must be first line
// require/import it so that I can simulate its behavior
// the name make the synchronous nature of the test clearer
const stubSyncNetwork = require("./network-adapter");
const webverifier = require("./website-verifier");

describe("unit test website verifier", () => {
  // use jest.resetAllMocks( ) to reset all the stubs before each test
  beforeEach(jest.resetAllMocks);

  test("with good content, returns true", async () => {
    // use mockReturnValue( ) to simulate a return value from the stub module
    stubSyncNetwork.fetchUrlText.mockReturnValue({
      ok: true,
      text: "illustrative",
    });
    // using the original entry point
    const result = await webverifier.isWebsiteAlive();
    expect(result.success).toBe(true);
    expect(result.status).toBe("ok");
  });

  test("with bad content, returns false", async () => {
    stubSyncNetwork.fetchUrlText.mockReturnValue({
      ok: true,
      text: "<span>hello world</span>",
    });
    // using the original entry point
    const result = await webverifier.isWebsiteAlive();
    expect(result.success).toBe(false);
    expect(result.status).toBe("missing text");
  });

  test("with bad url or network, throws", async () => {
    stubSyncNetwork.fetchUrlText.mockReturnValue({
      ok: false,
      text: "some network error",
    });
    try {
      await webverifier.isWebsiteAlive(stubSyncNetwork);
      fail("promise.rejext expected");
    } catch (err) {
      expect(err.success).toBe(false);
      expect(err.status).toBe("some network error");
    }
  });
});
```

### 6.3.2 函式型的轉換器

- 增加一個參數到進入點

```javascript
// website-verifier.js
const isWebsiteAlive = async (network) => {
  const result = await network.fetchUrlText("http://example.com");
  if (result.ok) {
    const text = result.text;
    return onFetchSuccess(text);
  }
  return Promise.reject(onFetchError(result.text));
};

const onFetchSuccess = (text) => {
  const included = text.includes("illustrative");
  if (included) {
    return { success: true, status: "ok" };
  }
  return { success: false, status: "missing text" };
};

const onFetchError = (err) => {
  return { success: false, status: err };
};

module.exports = {
  isWebsiteAlive,
};
```

- 我們預期 network-adapter 模組可以透過一個參數被注入到函式中
- 在函式設計中，我們可以使用高階函式與柯理化，並使用我們自己的網路依賴去配置一個預注入的函式
- 我們不再 import network-adapter 模組，import/requires 的數量減少會有效幫助長遠的維護

```javascript
// website-verifier.unit.spec.js 
const webverifier = require("./website-verifier");
// returns a custom object that matches the important parts of interface of the network-adapter, which always returns our fake result.We call this fuction from each test to generate a new fake network adapter.
const makeStubNetworkWithResult = (fakeResult) => {
  return {
    fetchUrlText: () => {
      return fakeResult;
    },
  };
};
describe("unit test website verifier", () => {
  test("with good content, returns true", async () => {
    const stubSyncNetwork = makeStubNetworkWithResult({
      ok: true,
      text: "illustrative",
    });
    // inject the fake network by sending it as a parameter to our entry point
    const result = await webverifier.isWebsiteAlive(stubSyncNetwork);
    expect(result.success).toBe(true);
    expect(result.status).toBe("ok");
  });

  test("with bad content, returns false", async () => {
    const stubSyncNetwork = makeStubNetworkWithResult({
      ok: true,
      text: "unexpected content",
    });
    // inject the fake network by sending it as a parameter to our entry point
    const result = await webverifier.isWebsiteAlive(stubSyncNetwork);
    expect(result.success).toBe(false);
    expect(result.status).toBe("missing text");
  });

  test("with bad url or network, throws", async () => {
    const stubSyncNetwork = makeStubNetworkWithResult({
      ok: false,
      text: "some error",
    });
    try {
      await webverifier.isWebsiteAlive(stubSyncNetwork);
      fail("promise.rejext expected");
    } catch (err) {
      expect(err.success).toBe(false);
      expect(err.status).toBe("some error");
    }
  });
});
```

- 我們不再需要做很多在模組化轉換器時做的事情，不需要直接模擬模組 jest.mock( )，預先 import 模組 require network-adapter，使用 jest.resetAllMocks( ) 重置 jest 的狀態

### 6.3.3 物件導向介面基底的轉換器

- 透過參數注入並將他使用到依賴注入設計當中

```javascript
// INetworkAdapter.ts
export interface INetworkAdapter {
  fetchUrlText(url: string): Promise<NetworkAdapterFetchResults>;
}
export interface NetworkAdapterFetchResults {
  ok: boolean;
  text: string;
}
```

```javascript
// network-adapter.ts
import fetch from "node-fetch";
import { INetworkAdapter, NetworkAdapterFetchResults } from "./INetworkAdapter";

export class NetworkAdapter implements INetworkAdapter {
  async fetchUrlText(url: string): Promise<NetworkAdapterFetchResults> {
    const resp = await fetch(url);
    if (resp.ok) {
      const text = await resp.text();
      return Promise.resolve({ ok: true, text: text });
    }
    return Promise.reject({ ok: false, text: resp.statusText });
  }
}
```

- 我們建立一個類別 WebsiteVerifier ，他有一個建構函式可以接收一個 INetworkAdapter 參數

```javascript
// website-verifier.ts
import { INetworkAdapter, NetworkAdapterFetchResults } from "./INetworkAdapter";
export interface WebsiteAliveResult {
  success: boolean;
  status: string;
}

export const isWebsiteAlive = async (
  network: INetworkAdapter
): Promise<WebsiteAliveResult> => {
  let netResult: NetworkAdapterFetchResults;
  try {
    netResult = await network.fetchUrlText("http://example.com");
    if (!netResult.ok) {
      throw netResult.text;
    }
    const text = netResult.text;
    return processNetSuccess(text);
  } catch (err) {
    throw processNetFail(err);
  }
};

const processNetSuccess = (text): WebsiteAliveResult => {
  const included = text.includes("illustrative");
  if (included) {
    return { success: true, status: "ok" };
  }
  return { success: false, status: "missing text" };
};

const processNetFail = (err): WebsiteAliveResult => {
  return { success: false, status: err };
};
```

- 這個類別的測試可以實例化一個假的 network adapter，並透過一個建構函式注入
- 在這個案例我們使用 Substitute.js 去建立一個假物件可以契合一個新的介面

```javascript
// website-verifier.unit.spec.ts 
import { WebsiteVerifier } from "./website-verifier";
import { Arg, Substitute } from "@fluffy-spoon/substitute";
import { INetworkAdapter, NetworkAdapterFetchResults } from "./INetworkAdapter";
// Helper fuction our tests will use to simulate the network adapter
const makeStubNetworkWithResult = (
  fakeResult: NetworkAdapterFetchResults
): INetworkAdapter => {
  // use substitute.js to generate the fake object
  const stubNetwork = Substitute.for<INetworkAdapter>();
  // make the fake adapter return what the test requires for a specific scenario
  stubNetwork.fetchUrlText(Arg.any()).returns(Promise.resolve(fakeResult));
  return stubNetwork;
};

describe("unit test website verifier", () => {
  test("with good content, returns true", async () => {
    const stubSyncNetwork = makeStubNetworkWithResult({
      ok: true,
      text: "illustrative",
    });
    const webVerifier = new WebsiteVerifier(stubSyncNetwork);

    const result = await webVerifier.isWebsiteAlive();
    expect(result.success).toBe(true);
    expect(result.status).toBe("ok");
  });

  test("with bad content, returns false", async () => {
    const stubSyncNetwork = makeStubNetworkWithResult({
      ok: true,
      text: "unexpected content",
    });
    const webVerifier = new WebsiteVerifier(stubSyncNetwork);

    const result = await webVerifier.isWebsiteAlive();
    expect(result.success).toBe(false);
    expect(result.status).toBe("missing text");
  });

  test("with bad url or network, throws", async () => {
    const stubSyncNetwork = makeStubNetworkWithResult({
      ok: false,
      text: "fake network error",
    });
    try {
      const webVerifier = new WebsiteVerifier(stubSyncNetwork);

      await webVerifier.isWebsiteAlive();
      fail("expected promise.reject via simulated network error");
    } catch (err) {
      expect(err.success).toBe(false);
      expect(err.status).toBe("fake network error");
    }
  });
});
```

- 這種控制反轉與依賴注入可以一起運作的很好，在物件導向的世界，使用介面做建構式的注入很常見，並提供一個很有效維護的方式去將依賴從邏輯分離出來

## 6.4 處理時間
> Dealing with Timers

- 時間處理器像是 setTimeout 代表著 JS 一個特別的問題
- 他們在很多程式碼中一部分會常使用到的
- 在一開始的時候抽取出轉換器還有進入點，這樣可以有效的不直接使用或運作他們
- 直接猴子補丁 `Monkey patching` 函式
- 使用 Jest 或其他測試框架去使他們不被使用或控制他們

### 6.4.1 使用猴子補丁去模擬時間
- 猴子補丁 `Monkey patching`：可以在程式運行時動態修改或擴充程式碼
- 程式語言像是 JS、Ruby、Python 都能很簡單的使用猴子補丁，但像是強型別並且編譯型語言 C#、Java 就很難使用

```javascript
// timing-samples.js 
const { Observable, interval } = require('rxjs');
const { map } = require('rxjs/operators');
const util = require('util');

const calculate1 = (x, y, resultCallback) => {
  setTimeout(() => { resultCallback(x + y); },
    5000);
};
```

```javascript
// timing-samples.spec.js
const { Observable, Subject, from, interval } = require("rxjs");
const Samples = require("./timing-samples");
const util = require("util");

describe("monkey patching ", () => {
  let originalTimeOut;
  // save and reset setTimout before and after each test otherwise we could impact future tests implicitly
  beforeEach(() => (originalTimeOut = setTimeout));
  afterEach(() => (setTimeout = originalTimeOut));

  test("calculate1", (done) => {
    // replace setTimout with a purely synchronous implementation that invokes the received callback immediately
    setTimeout = (callback, ms) => callback();
    Samples.calculate1(1, 2, (result) => {
      expect(result).toBe(3);
      done();
    });
  });
});
```

- 所有東西都是同步的，所以不需要使用 done( ) 去等待一個 callback 的調用

### 6.4.2 使用 Jest 做 setTimout 假物件
- Jest 提供三個主要函式可以處理 JS 大部分時間
- jest.useFakeTimers：可以模擬多種時間函式，像是 setTimout
- jest.ResetAllTimers：重置所有假時間物件變成真的
- jest.advanceTimerstoNextTimer：觸發任何假時間物件，使得任何 callbacks 可以被觸發

```javascript
// timing-samples.spec.js
describe("calculate1 - with jest", () => {
  beforeEach(jest.clearAllTimers);
  beforeEach(jest.useFakeTimers);

  test("fake timeout with callback", (done) => {
    Samples.calculate1(1, 2, (result) => {
      expect(result).toBe(3);
      done();
    });
    jest.advanceTimersToNextTimer();
  });
});
```
- 不需要使用 done( )，因為所有事情都是同步的
- 如果沒有 advanceTimersToNextTimer，我們的時間假物件 setTimout 會被永遠卡住
- advanceTimersToNextTimer 在處理遞迴性時間情況很有幫助
- 同樣的設計使用 setInterval 也可以有很好的運作

```javascript
// timing-samples.js
const calculate4 = (getInputsFn, resultFn) => {
  setInterval(() => {
    const { x, y } = getInputsFn();
    resultFn(x + y);
  }, 1000);
};
```

```javascript
// timing-samples.spec.js
describe("calculate with intervals", () => {
  beforeEach(jest.clearAllTimers);
  beforeEach(jest.useFakeTimers);

  test("calculate 4 with input and output functions for intervals", () => {
    const inputFn = () => ({ x: 1, y: 2 });
    const results = [];
    // add to variable so that we can verify the number of callbacks.By doing that we've sacrificed some readablility and complexity to make the test a bit more robust.
    Samples.calculate4(inputFn, (result) => results.push(result));

    jest.advanceTimersToNextTimer();
    jest.advanceTimersToNextTimer();

    expect(results[0]).toBe(3);
    expect(results[1]).toBe(3);
  });
});
```
- 呼叫 advanceTimersToNextTimer 兩次去觸發 setInterval 兩次
- 這樣做是為了確認我們計算新的值有被正確的傳遞

## 6.5 處理普通事件
> Dealing with Common Events

### 6.5.1 自訂事件觸發 `event emitters`

https://www.digitalocean.com/community/tutorials/using-event-emitters-in-node-js

- 在 Node.js，自訂事件觸發為藉由發送一個訊息表示一個動作已經完成去觸發一個事件
- JS 開發者可以從自訂事件觸發監聽一個事件，允許他們在每次事件被觸發時執行函式
- 這些事件由需要被傳給監聽者的獨立的字串與資料所組成

```javascript
// adder.js
const EventEmitter = require("events");

class Adder extends EventEmitter {
  constructor() {
    super();
  }

  add(x, y) {
    const result = x + y;
    this.emit("added", result);
    return result;
  }
}

module.exports = {
  Adder,
};
```

```javascript
// 
const { Adder } = require("./adder");
describe("events based module", () => {
  describe("add", () => {
    it("generates addition event when called", (done) => {
      const adder = new Adder();
      adder.on("added", (result) => {
        expect(result).toBe(3);
        done();
      });
      adder.add(1, 2);
    });
  });
});
```
- 使用 done( ) 確認事件實際上有發出
- 使用 expect(x).toBe(y) 可以確認事件中發出的值，是否跟測試中事件被觸發時的一樣

### 6.5.2 處理點擊事件

```html
// index.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>File to Be Tested</title>
    <script src="index-helper.js"></script>
</head>
<body>
    <div>
        <div>A simple button</div>
        <Button data-testid="myButton" id="myButton">Click Me</Button>
        <div data-testid="myResult" id="myResult">Waiting...</div>
    </div>
</body>
</html>
```

```javascript
// index-helper.js
window.addEventListener("load", () => {
  document
    .getElementById("myButton")
    .addEventListener("click", onMyButtonClick);

  const resultDiv = document.getElementById("myResult");
  resultDiv.innerText = "Document Loaded";
});

function onMyButtonClick() {
  const resultDiv = document.getElementById("myResult");
  resultDiv.innerText = "Clicked!";
}
```

- 觸發一個點擊事件並確認在頁面中有正確的改變值

![Unit Test Click as an entry point, element as an exit point.png](http://192.168.25.60:8000/files/file_storage/7094aae3.png)

```javascript
// index-helper.spec.js 
// apply the browser-simulating jsdom environment just for this file
/**
 * @jest-environment jsdom
 */
//(the above is required for window events)
const fs = require("fs");
const path = require("path");
require("./index-helper.js");
// extracted utility methof
const loadHtml = (fileRelativePath) => {
  const filePath = path.join(__dirname, "index.html");
  const innerHTML = fs.readFileSync(filePath);
  document.documentElement.innerHTML = innerHTML;
};
// extracted utility methof
const loadHtmlAndGetUIElements = () => {
  loadHtml("index.html");
  const button = document.getElementById("myButton");
  const resultDiv = document.getElementById("myResult");
  return { window, button, resultDiv };
};

describe("index helper", () => {
  test("vanilla button click triggers changing the result on the page", () => {
    const { window, button, resultDiv } = loadHtmlAndGetUIElements();
    // simulate the document.load event so that our custom script under test can start running
    window.dispatchEvent(new Event("load"));
		// trigger the click,as if the used had clicked the button
    button.click();
		// verify that an element in our document had actually changed, which means our code successful subscribed to the event and had done its work
    expect(resultDiv.innerText).toBe("Clicked!");
  });
});
```

## 6.6 dom-testing-library
> Bringing in dom-testing-library

```javascript
// index-helper-testlib.spec.js
/**
 * @jest-environment jest-environment-jsdom-sixteen
 */
//(the above is required for window events)
const fs = require("fs");
const path = require("path");
// import some of the library APIs to be used
const { fireEvent, findByText, getByText } = require("@testing-library/dom");
require("./index-helper.js");

const loadHtml = (fileRelativePath) => {
  const filePath = path.join(__dirname, "index.html");
  const innerHTML = fs.readFileSync(filePath);
  document.documentElement.innerHTML = innerHTML;
  // return the document element, since the libaray APIs require using that as the basis for most of the work
  return document.documentElement;
};

const loadHtmlAndGetUIElements = () => {
  const docElem = loadHtml("index.html");
  // use regular text of the page items to get the items, instead of their IDs or test ids.This is part of way the library pushed us to work so things feel more natuaral and from the user's point of view. 
  const button = getByText(docElem, "click me", { exact: false });
  return { window, docElem, button };
};

describe("index helper", () => {
  test("dom test lib button click triggers change in page", () => {
    const { window, docElem, button } = loadHtmlAndGetUIElements();
    // use the library's fireEvent api to simplify event dispatching
    fireEvent.load(window);
		// use the library's fireEvent api to simplify event dispatching
    fireEvent.click(button);

    //wait until true ot timeout in 1 sec
    // use the findByText query that will wait until item is found or time out within 1 second
    expect(findByText(docElem, "clicked", { exact: false })).toBeTruthy();
  });
});
```













