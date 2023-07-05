---
title: Unit Test CH5
description: The Art of Unit Testing Third Edition v7
published: true
date: 2022-07-13T02:05:22.008Z
tags: unit test
editor: markdown
dateCreated: 2022-07-11T02:27:00.970Z
---

# CH5 Isolation (mocking) frameworks
> - 定義隔離框架 `Isolation Frameworks` 並且他們可以提供什麼幫助
> - 兩種框架的主要味道
> - 使用 Jest 做假模組
> - 使用 Jest 做假函式
> - 物件導向假物件當作替身

## 5.1 定義隔離框架
> Defining Isolation Frameworks

- 一個隔離框架是一套可用來幫助寫程式的 API，使用這套 API 來建立假物件比手刻假物件要容易得多、快得多、簡潔許多。它是一個可以在 runtime 建立和設定假物件的 library，這些假物件稱為 dynamic stub 或 dynamic mock
- 之所以稱為隔離框架，是因為這些框架幫你隔離工作 unit 跟它的 dependency object
- 使用隔離框架的好處是讓隔離框架幫我們生 stub 跟 mock object，省去手刻的麻煩

## 5.2 快速的看一下隔離框架的樣子
> A quick look at the Isolation frameworks landscape

### 不同的程式語言有不同的隔離框架
- C++：mockpp
- Java：Mockito，PowerMock，JMock
- C#：Moq，FakeItEasy，NSubstitute
- Kotlin：MockK，Mockito-kotlin
### JS 有的隔離框架
#### 鬆散的 JS 隔離框架
- Vanilla JS
#### 有型別的 JS 隔離框架
##### 函式型
- Jest
- Sinon
- Testdouble
- Jasmine (有隔離 APIs)
##### 物件導向
- ts-mockito
- typemoq
- substitute.js
- ts-mockery
- OmniMock

## 5.3 選擇鬆散的或有型別的
> Choosing a loose vs typed "flavor"

### 什麼樣子的依賴是最常需要去做假物的呢?
#### 模組依賴 `Module dependencies` ：引入，需要的
- Jest，其他鬆散的 JS 隔離框架
#### 函式 `Functional` ：單一/高階函式，簡單參數與值
- Jest，其他有型別且函式的 JS 隔離框架
#### 全物件 `Full object` ：物件繼承與介面
- substitute.js，其他有型別且物件導向的 JS 隔離框架

## 5.4 動態產生模組的假物件
> Faking Modules Dynamically

- 一個 password verifier 需要兩個依賴
- 一個 configuration service 可以協助決定 logging 層級 (INFO or ERROR)

![Unit Test presents an a password verifier with two dependencies.png](http://192.168.25.60:8000/files/file_storage/96609bd9.png)

```javascript
// app-config.json
{
  "logLevel": "info"
}
```
```javascript
// configuration-service.js
const configs = require('./app-config.json');

const getLogLevel = () =>{
    return configs.logLevel
}

module.exports ={
    getLogLevel
}
```
```javascript
// complicated-logger.js 
const info = (text) => {
  console.log(`INFO: ${text}`);
};
const debug = (text) => {
  console.log(`DEBUG: ${text}`);
};

module.exports = {
  info,
  debug
};
```

![Unit Test test a password verifier with two dependencies.png](http://192.168.25.60:8000/files/file_storage/758aed61.png)

- 虛設常式 `Stub`：從 configuration-service 中的 getLogLevel( ) 得到得回傳值
- 模擬物件 `Mock`：確認 logger module 中的 info( ) 是否有被呼叫

```javascript
// password-verifier.js
const { info, debug } = require("./complicated-logger");
const { getLogLevel } = require("./configuration-service");

const log = (text) => {
  if (getLogLevel() === "info") {
    info(text);
  }
  if (getLogLevel() === "debug") {
    debug(text);
  }
};

const verifyPassword = (input, rules) => {
  const failed = rules
    .map((rule) => rule(input))
    .filter((result) => result === false);

  if (failed.length === 0) {
    log("PASSED");
    return true;
  }
  log("FAIL");
  return false;
};

module.exports = {
  verifyPassword,
};
```

- 使用 jest.mock( )

```javascript
// password-verifier.jestmocks.spec.js 
// The first two lines in the file are required to contain the module faking API calls.
jest.mock("./complicated-logger");
jest.mock("./configuration-service");

const { stringMatching } = expect;
const { verifyPassword } = require("./password-verifier");

// We're getting the fake instances of the moudles by requiring them in our test, so that we can configure them later on.
const mockLoggerModule = require("./complicated-logger");
const stubConfigModule = require("./configuration-service");

describe("password verifier", () => {
  // We're telling jest to reset any fake module behavior between tests.
  afterEach(jest.resetAllMocks);

  test(`with info log level and no rules, 
          it calls the logger with PASSED`, () => {
    // We're configuring the stub logger module's getLogLevel( ) function to return a fake "info" value.
    stubConfigModule.getLogLevel.mockReturnValue("info");
    verifyPassword("anything", []);
		// We're asserting the mock logger's info( ) function was called correctly using the stringMatching helper function that exits on the expect object. 
    expect(mockLoggerModule.info).toHaveBeenCalledWith(stringMatching(/PASS/));
  });

  test(`with debug log level and no rules, 
          it calls the logger with PASSED`, () => {
    // Reconfiguring the stub config and asserting on the mock logger as done previously.
    stubConfigModule.getLogLevel.mockReturnValue("debug");
    verifyPassword("anything", []);
    expect(mockLoggerModule.debug).toHaveBeenCalledWith(stringMatching(/PASS/));
  });
});
```

### 5.4.1 可以注意到 Jest 的 APIs

- `hoisting`：https://betterprogramming.pub/hoisting-in-javascript-6af97650dbb2

### 5.4.2 考慮將依賴做直接的抽象化

- `Hexagonal architecture`：https://alistair.cockburn.us/hexagonal-architecture/
- 使用 adapter

## 5.5 動態產生函式型的模擬物件與虛設常式
> Functional Mocks and Stubs - Dynamically

```javascript
// 00-password-verifier00.js
const makeVerifier = (rules, logger) => {
  return (input) => {
    const failed = rules
      .map(rule => rule(input))
      .filter(result => result === false);

    console.log(failed);
    if (failed.length === 0) {
      logger.info('PASSED');
      return true;
    }
    logger.info('FAIL');
    return false;
  };
};

module.exports = {
  makeVerifier
};
```

- 手動做一個函式的模擬物件

```javascript
test("given logger and passing scenario", () => {
  // we have to declare a custom variable to hold the value passed in.
  let logged = "";
  // we have to generically save the passed in value to that variable
  const mockLog = { info: (text) => (logged = text) };
  const passVerify = makeVerifier([], mockLog);
  passVerify("any input");
  // we assert on the value of that variable
  expect(logged).toMatch(/PASSED/);
});
```

- 使用 jest.fn( )

```javascript
// 01-password-verifier00.spec.js
const { makeVerifier } = require('./00-password-verifier00');
const { stringMatching } = expect;

describe('higher order factory functions', () => {
  describe('password verifier', () => {
    test('given logger and passing scenario', () => {
      const mockLog = { info: jest.fn() };
      const verify = makeVerifier([], mockLog);
      verify('any input');

      expect(mockLog.info)
        .toHaveBeenCalledWith(stringMatching(/PASS/));
    });
  });
});
```

## 5.6 動態產生物件導向的模擬物件與虛設常式
> Object Oriented Dynamic Mocks and Stubs

```javascript
// complicated-logger.ts
export interface IComplicatedLogger {
    info(text: string, method: string)
    debug(text: string, method: string)
    warn(text: string, method: string)
    error(text: string, method: string)
}
```

```javascript
// 02-password-verifier.manual.spec.ts
import { PasswordVerifier2 } from "./00-password-verifier2";
import { IComplicatedLogger } from "./interfaces/complicated-logger";

describe("working with long interfaces", () => {
  describe("password verifier", () => {
    class FakeLogger implements IComplicatedLogger {
      debugText = "";
      debugMethod = "";
      errorText = "";
      errorMethod = "";
      infoText = "";
      infoMethod = "";
      warnText = "";
      warnMethod = "";

      debug(text: string, method: string) {
        this.debugText = text;
        this.debugMethod = method;
      }

      error(text: string, method: string) {
        this.errorText = text;
        this.errorMethod = method;
      }

      info(text: string, method: string) {
        this.infoText = text;
        this.infoMethod = method;
      }

      warn(text: string, method: string) {
        this.warnText = text;
        this.warnMethod = method;
      }
    }

    test("verify, with logger and passing, calls logger with PASS", () => {
      const mockLog = new FakeLogger();
      const verifier = new PasswordVerifier2([], mockLog);

      verifier.verify("anything");

      expect(mockLog.infoText).toMatch(/PASSED/);
    });
  });
});
```

```javascript
// 03-password-verifier.jestFn.longinterfaces.spec.ts
import { IComplicatedLogger } from "./interfaces/complicated-logger";
import { PasswordVerifier2 } from "./00-password-verifier2";
import stringMatching = jasmine.stringMatching;

describe("working with long interfaces", () => {
  describe("password verifier", () => {
    test("verify, with logger and passing, calls logger with PASS", () => {
      const mockLog: IComplicatedLogger = {
        info: jest.fn(),
        warn: jest.fn(),
        debug: jest.fn(),
        error: jest.fn(),
      };

      const verifier = new PasswordVerifier2([], mockLog);
      verifier.verify("anything");

      expect(mockLog.info).toHaveBeenCalledWith(
        stringMatching(/PASS/),
        "verify"
      );
    });
  });
});
```

### 5.6.1 轉換成有型別的隔離框架
- 使用 substitute.js 
```javascript
// 03-password-verifier.substitute.longinterfaces.spec.ts 
import { IComplicatedLogger } from "./interfaces/complicated-logger";
import { PasswordVerifier2 } from "./00-password-verifier2";
import { Substitute, Arg } from "@fluffy-spoon/substitute";

describe("working with long interfaces", () => {
  describe("password verifier", () => {
    test("verify, with logger and passing, calls logger with PASS", () => {
      // we're using a Substitute.for(T) to generate the fake object, which absolves us of caring about any other functions that then one we're testing againt, even if the object signature changes in the feature.
      const mockLog = Substitute.for<IComplicatedLogger>();

      const verifier = new PasswordVerifier2([], mockLog);
      verifier.verify("anything");

      mockLog.received().info(
        // we'rs using .received() as our verification mechanism, as well as another argument matcher(Arg.is), this time from Substitute's API, that works just like stringMatches from jasmine, but is only the tip of the iceberg
        Arg.is((x) => x.includes("PASSED")),
        "verify"
      );
    });
  });
});
```

## 5.7 動態生成虛設常式行為
> Stubbing Behavior Dynamically

- 從一個假函式使用 jest.fn( ) 生成虛設常式

```javascript
// 04-jestFn-mockReturnValue.spec.ts
test("fake same return values", () => {
  const stubFunc = jest.fn()
      .mockReturnValue("abc");

  //value remains the same
  expect(stubFunc()).toBe("abc");
  expect(stubFunc()).toBe("abc");
  expect(stubFunc()).toBe("abc");
});

test("fake multiple return values", () => {
  const stubFunc = jest.fn()
    .mockReturnValueOnce("a")
    .mockReturnValueOnce("b")
    .mockReturnValueOnce("c");

  //value remains the same
  expect(stubFunc()).toBe("a");
  expect(stubFunc()).toBe("b");
  expect(stubFunc()).toBe("c");
  expect(stubFunc()).toBe(undefined);
});
```

### 5.7.1 一個物件導向的模擬物件與虛設常式案例

![Unit Test using MaintenanceWindow interface.png](http://192.168.25.60:8000/files/file_storage/0ecd1c12.png)

- MaintenanceWindow 介面為一個依賴注入，他被用來決定哪裡可以使用或不可以使用 password verification，並傳送適合的訊息給 logger

```javascript
// maintenance-window.ts
export interface MaintenanceWindow {
    isUnderMaintenance(): boolean
}
```

```javascript
// 00-password-verifier3.ts 
import { IComplicatedLogger } from "../02-longinterfaces-faking/interfaces/complicated-logger";
import { MaintenanceWindow } from "./maintenance-window";

export class PasswordVerifier3 {
  private _rules: any[];
  private _logger: IComplicatedLogger;
  private _maintenanceWindow: MaintenanceWindow;

  constructor(
    rules: any[],
    logger: IComplicatedLogger,
    maintenanceWindow: MaintenanceWindow
  ) {
    this._rules = rules;
    this._logger = logger;
    this._maintenanceWindow = maintenanceWindow;
  }

  verify(input: string): boolean {
    if (this._maintenanceWindow.isUnderMaintenance()) {
      this._logger.info("Under Maintenance", "verify");
      return false;
    }
    const failed = this._rules
      .map((rule) => rule(input))
      .filter((result) => result === false);

    if (failed.length === 0) {
      this._logger.info("PASSED", "verify");
      return true;
    }
    this._logger.info("FAIL", "verify");
    return false;
  }
}
```

### 5.7.2 使用 Substitute.js 產生模擬物件與虛設常式

![Unit Test A MaintenanceWindow dependency.png](http://192.168.25.60:8000/files/file_storage/15e233ca.png)

```javascript
// 05-password-verifier3.substitute.spec.ts
import { Substitute } from "@fluffy-spoon/substitute";
import { PasswordVerifier3 } from "./00-password-verifier3";
import { IComplicatedLogger } from "../02-longinterfaces-faking/interfaces/complicated-logger";
import { MaintenanceWindow } from "./maintenance-window";

const makeVerifierWithNoRules = (log, maint) =>
  new PasswordVerifier3([], log, maint);

describe("working with substitute part 2", () => {
  test("verify, with logger, calls logger", () => {
    const stubMaintWindow = Substitute.for<MaintenanceWindow>();
    stubMaintWindow.isUnderMaintenance().returns(true);
    const mockLog = Substitute.for<IComplicatedLogger>();
    const verifier = makeVerifierWithNoRules(mockLog, stubMaintWindow);

    verifier.verify("anything");

    mockLog.received().info("Under Maintenance", "verify");
  });

  test("verify, with logger, calls logger", () => {
    const stubMaintWindow = Substitute.for<MaintenanceWindow>();
    stubMaintWindow.isUnderMaintenance().returns(false);
    const mockLog = Substitute.for<IComplicatedLogger>();
    const verifier = makeVerifierWithNoRules(mockLog, stubMaintWindow);

    verifier.verify("anything");

    mockLog.received().info("PASSED", "verify");
  });
});
```

## 5.8 隔離框架的好處與陷阱
> Advantages and traps of isolation frameworks

- 更容易的做模組假物件
- 更容易模擬值或錯誤
- 更容易做假物件

### 5.8.1 在很多時候不用做模擬物件
- 最大的陷阱是隔離框架讓你很容易的做假物件，但這導致你以為一開始都需要做假物件
- 不是說不需要模擬物件，而是對於大部分的單元測試來說，做模擬物件不是標準程序
- 工作單元有三種出口點：回傳值、改變狀態、呼叫第三方。只有其中一個使用模擬物件會為單元測試帶來效益，其他則不會

### 5.8.2 沒有可讀性的測試程式碼
- 在測試中使用模擬物件已經會造成不太好閱讀
- 擁有過多的模擬物件或預期，在單一測試裡面會造成難以維護或難以得知什麼事要被測試
- 若發現測試變得難以閱讀時，要適時的移除模擬物件或將測試再切割成更小的測試

### 5.8.3 確認到不對的東西
- 模擬物件讓你去確認介面被呼叫的方法或函式，但不是代表你測試到對的東西
- 很多新手確認東西只是因為可以確認，而不是因為他們是合理的
#### ex：
- 確認一個內部函式呼叫另外一個內部函式(非出口點)
- 確認一個被呼叫的模擬物件(對於即將要進來的依賴不應該被確認)
- 確認一個被呼叫的很簡單的東西，因為有人叫你寫測試，但你卻不確定是否真的應該被測試

### 5.8.4 在每個測試前有多於一個模擬物件
- 測試多於一個重點會使測試遺失焦點並產生維護的問題
- 有兩個模擬物件跟在同一個工作單元測試很多結果是一樣的

### 5.8.5 過度使用的測試
- 當測試有過多的預期時，測試會變得脆弱
- 盡量使用虛設常式而非模擬物件：虛設常式可以到處都使用，但是模擬物件無法，模擬物件一次只能使用一個，過多的模擬物件會干擾到正在測試的模擬物件
- 避免把虛設常式當作模擬物件在使用：使用虛設常式只用於模擬值到工作單元或拋出預期，不要用虛設常式來確認被呼叫的方法






