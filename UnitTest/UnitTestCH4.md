---
title: Unit Test CH4
description: The Art of Unit Testing Third Edition v7
published: true
date: 2022-07-11T01:47:18.790Z
tags: unit test
editor: markdown
dateCreated: 2022-07-06T01:13:49.408Z
---

# CH4 Interaction testing using mock objects
> - 定義交互測試 `interaction testing`
> - 使用 mock 的理由
> - 注入並使用 mock
> - 函式例子
> - 模組例子
> - 物件導向例子
> - 處理複雜的介面
> - 部分模擬

## 4.1 交互測試、模擬物件、虛設常式
> Interaction testing, Mocks and Stubs

![Unit Test mock and stub.png](http://192.168.25.60:8000/files/file_storage/9300f5d3.png)

### 交互測試 ( Interaction testing )
- 交互測試是在確認工作單元如何相互作用，並呼叫函式發送訊息給一個依賴

### 模擬物件 ( Mocks )
- 用來打破要出去的依賴
- 為假的模組、物件、函式提供假個行為或資料給測試
- 要驗證

### 虛設常式 ( Stubs )
- 用來打破要進來的依賴
- 為假的模組、物件、函式提供假個行為或資料給測試
- 沒有要驗證

## 4.2 依賴一個日誌
> Depending on a logger

```javascript
// 00-password-verifier00.js
// this dependency is impossible to fake with traditional injection techniques
const _ = require("lodash");
const log = require("./complicated-logger");

const verifyPassword = (input, rules) => {
  const failed = rules
    .map((rule) => rule(input))
    .filter((result) => result === false);

  console.log(failed);
  if (failed.length === 0) {
    // this line is impossible to test with traditional injection techniques
    log.info("PASSED");
    return true;
  }
  // this line is impossible to test with traditional injection techniques
  log.info("FAIL");
  return false;
};
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
- verifyPassword 函式是一個工作單元的進入口
- 有兩個出口點：一個回傳一個值，一個呼叫 logger.info()

![Unit Test Password verifier.png](http://192.168.25.60:8000/files/file_storage/d05e998a.png)

- 抽象化依賴：創建一個接縫 `seam`

![Unit Test way to create a seam.png](http://192.168.25.60:8000/files/file_storage/585ec270.png)


## 4.3 標準格式：介紹參數重構
> Standard Style: Introduce Parameter Refactoring

- 模擬物件 Logger 參數注入
```javascript
const verifyPassword2 = (input, rules, logger) => {
  const failed = rules
    .map((rule) => rule(input))
    .filter((result) => result === false);

  if (failed.length === 0) {
    logger.info("PASSED");
    return true;
  }
  logger.info("FAIL");
  return false;
};
```

```javascript
const { verifyPassword2 } = require("./00-password-verifier00");

describe("password verifier", () => {
  describe("given logger, and passing scenario", () => {
    it("calls the logger with PASSED", () => {
      let written = "";
      const mockLog = { info: (text) => (written = text) };

      verifyPassword2("anything", [], mockLog);

      expect(written).toMatch(/PASSED/);
    });
  });
});
```

## 4.4 模擬物件 Mock 與虛設常式 Stub 差異的重要性
> The Importance of Mock and Stub Differentiation

- 模擬物件：表示一個工作單元需要的東西
	- ex：一個 logger 、發送一個電郵
- 虛設常式：表示要進來的資訊或行為
	- ex：資料庫查詢回應的 false 、配置丟出一個錯誤
- 可以有多個虛設常式，但通常只有一個模擬物件
- 若無法區別出差異會有以下缺點：可讀性、可維護性、可信賴性

## 4.5 模組化的模擬物件
> Modular Style Mocks

### 4.5.1 案例產品的程式碼

```javascript
// password-verifier-before.js
const { info, debug } = require('./complicated-logger');
const { getLogLevel } = require('./configuration-service');

const log = (text) => {
  if (getLogLevel() === 'info') {
    info(text);
  }
  if (getLogLevel() === 'debug') {
    debug(text);
  }
};
const verifyPassword = (input, rules) => {
  const failed = rules
    .map((rule) => rule(input))
    .filter((result) => result === false);
  if (failed.length === 0) {
    log('PASSED');
    return true;
  }
  log('FAIL');
  return false;
};
module.exports = {
  verifyPassword,
};
```

### 4.5.2 用模組化注入重構程式碼

```javascript
// password-verifier-after.js
const originalDependencies = {
  log: require('./complicated-logger'),
};

let dependencies = { ...originalDependencies };

const resetDependencies = () => {
  dependencies = { ...originalDependencies };
};

const injectDependencies = (fakes) => {
  Object.assign(dependencies, fakes);
};

const verifyPassword = (input, rules) => {
  const failed = rules
    .map((rule) => rule(input))
    .filter((result) => result === false);

  if (failed.length === 0) {
    dependencies.log.info('PASSED');
    return true;
  }
  dependencies.log.info('FAIL');
  return false;
};

module.exports = {
  verifyPassword,
  injectDependencies,
  resetDependencies,
};
```

### 4.5.3 一個用模組化注入的測試案例

```javascript
// password-verifier-injectable.spec.js
const {
  verifyPassword,
  injectDependencies,
  resetDependencies,
} = require("./password-verifier-injectable");

describe("password verifier", () => {
  afterEach(resetDependencies);

  describe("given logger and passing scenario", () => {
    it("calls the logger with PASS", () => {
      let logged = "";
      const mockLog = { info: (text) => (logged = text) };
      injectDependencies({ log: mockLog });

      verifyPassword("anything", []);

      expect(logged).toMatch(/PASSED/);
    });
  });
});
```

## 4.6 函式化格式的模擬物件
> Mocks in a Functional Style

### 4.6.1 使用柯里化 `curry` 方式

```javascript
// 00-password-verifier00.js
const verifyPassword3 = _.curry((rules, logger, input) => {
  const failed = rules
    .map((rule) => rule(input))
    .filter((result) => result === false);

  if (failed.length === 0) {
    logger.info("PASSED");
    return true;
  }
  logger.info("FAIL");
  return false;
});
```

```javascript
// 02-password-verifier00.currying.spec.js
const { verifyPassword3 } = require("./00-password-verifier00");
const { stringMatching } = expect;

describe("password verifier", () => {
  describe("given logger and passing scnario", () => {
    it("calls the logger with PASS", () => {
      let logged = "";
      const mockLog = { info: (text) => (logged = text) };
      const injectedVerify = verifyPassword3([], mockLog);

      // this partially applied function can be passed arround
      // to other places in the code
      // without needing to inject the logger
      injectedVerify("anything");

      expect(logged).toMatch(/PASSED/);
    });
  });
});
```

### 4.6.2 使用高階函式並不做柯里化
- 注入一個高階函式
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

```javascript
// 01-password-verifier00.spec.js
const { makeVerifier } = require("./00-password-verifier00");

describe("higher order factory functions", () => {
  describe("password verifier", () => {
    test("given logger and passing scenario", () => {
      let logged = "";
      const mockLog = { info: (text) => (logged = text) };
      const passVerify = makeVerifier([], mockLog);

      passVerify("any input");

      expect(logged).toMatch(/PASSED/);
    });
  });
});
```

## 4.7 物件導向形式的模擬物件
> Mocks in an Object-Oriented Style

### 4.7.1 為了注入而重構程式碼
- 類別基底建構函式的注入
```javascript
// 00-password-verifier00.js
class PasswordVerifier {
  _rules;
  _logger;

  constructor(rules, logger) {
    this._rules = rules;
    this._logger = logger;
  }

  verify(input) {
    const failed = this._rules
        .map(rule => rule(input))
        .filter(result => result === false);

    console.log(failed);
    if (failed.length === 0) {
      this._logger.info('PASSED');
      return true;
    }
    this._logger.info('FAIL');
    return false;
  }
}

module.exports = {
  PasswordVerifier
};
```
- 注入一個模擬物件 logger 當作建構函式的參數
```javascript
// 01-password-verifier00.spec.js 
const { PasswordVerifier } = require("./00-password-verifier00");

describe("duck typing with function constructor injection", () => {
  describe("password verifier", () => {
    test("given logger and passing scenario, calls logger with PASSED", () => {
      let logged = "";
      const mockLog = { info: (text) => (logged = text) };
      const verifier = new PasswordVerifier([], mockLog);
      verifier.verify("any input");

      expect(logged).toMatch(/PASSED/);
    });
  });
});
```

### 4.7.2 用介面注入重構程式碼
- 寫一個介面 ILogger
```javascript
// logger.ts
export interface ILogger {
    info(text: string);
}
```
```javascript
// simple-logger.ts
import {ILogger} from "./interfaces/logger";

//this class might have dependencies on files or network
class SimpleLogger implements ILogger{
    info(text: string) {
    }
}
```
```javascript
// 00-password-verifier.ts 
import {ILogger} from "./interfaces/logger";

export class PasswordVerifier {
    private _rules: any[];
    private _logger: ILogger;

    constructor(rules: any[], logger:ILogger) {
        this._rules = rules;
        this._logger = logger;
    }

    verify(input: string):boolean{
        const failed = this._rules
            .map(rule => rule(input))
            .filter(result => result === false);

        if (failed.length === 0) {
            this._logger.info('PASSED');
            return true;
        }
        this._logger.info('FAIL');
        return false;
    }
}
```
- 注入一個自己寫的模擬物件 ILogger
```javascript
// 01-password-verifier.handwritten.spec.ts
import { PasswordVerifier } from "./00-password-verifier";
import { ILogger } from "./interfaces/logger";

class FakeLogger implements ILogger {
  written: string;
  info(text: string) {
    this.written = text;
  }
}
describe("faking strongly typed interfaces", () => {
  describe("password verifier", () => {
    test("verify, with logger, calls logger", () => {
      const mockLog = new FakeLogger();
      const verifier = new PasswordVerifier([], mockLog);

      verifier.verify("anything");

      expect(mockLog.written).toMatch(/PASS/);
    });
  });
  describe("Explicit Function Mocks", () => {
    test("verify, with logger, calls logger", () => {
      const mockLog = new FakeLogger();
      let logged = "";
      mockLog.info = (text) => (logged = text);
      const verifier = new PasswordVerifier([], mockLog);

      verifier.verify("anything");

      expect(logged).toMatch(/PASS/);
    });
  });
});
```
## 4.8 處理複雜的介面
> Dealing with complicated interfaces

### 4.8.1 複雜的介面案例

```javascript
// complicated-logger.ts 
export interface IComplicatedLogger {
    info(text: string)
    debug(text: string, obj: any)
    warn(text: string)
    error(text: string, location: string, stacktrace: string)
}
```
```javascript
// 00-password-verifier2.ts
import {IComplicatedLogger} from "./interfaces/complicated-logger";

export class PasswordVerifier2 {
    private _rules: any[];
    private _logger: IComplicatedLogger;

    constructor(rules: any[], logger:IComplicatedLogger) {
        this._rules = rules;
        this._logger = logger;
    }

    verify(input: string):boolean{
        const failed = this._rules
            .map(rule => rule(input))
            .filter(result => result === false);

        if (failed.length === 0) {
            this._logger.info('PASSED');
            return true;
        }
        this._logger.info('FAIL');
        return false;
    }
}
```

### 4.8.2 寫處理複雜介面的測試
```javascript
// 03-password-verifier.longinterfaces.spec.ts
import { IComplicatedLogger } from "./interfaces/complicated-logger";
import { PasswordVerifier2 } from "./00-password-verifier2";

// describe("working with long interfaces", () => {
describe("password verifier", () => {
  class FakeComplicatedLogger implements IComplicatedLogger {
    infoWritten = "";
    debugWritten = "";
    errorWritten = "";
    warnWritten = "";

    debug(text: string, obj: any) {
      this.debugWritten = text;
    }

    error(text: string, location: string, stacktrace: string) {
      this.errorWritten = text;
    }

    info(text: string) {
      this.infoWritten = text;
    }

    warn(text: string) {
      this.warnWritten = text;
    }
  }

  test("verify, with logger and passing, calls logger with PASS", () => {
    const mockLog = new FakeComplicatedLogger();

    const verifier = new PasswordVerifier2([], mockLog);
    verifier.verify("anything");

    expect(mockLog.infoWritten).toMatch(/PASSED/);
  });

  test("verify, with duck typing", () => {
    const mockLog = {} as IComplicatedLogger;
    let logged = "";
    mockLog.info = (text) => (logged = text);

    const verifier = new PasswordVerifier2([], mockLog);
    verifier.verify("anything");

    expect(logged).toMatch(/PASSED/);
  });
});
```

### 4.8.3 直接使用複雜的介面缺點
- 對於確認多個參數使用在多個方法與呼叫會是低效的
- 看起來我們會是依賴第三方介面而不是內部的，這樣會讓測試變得更脆弱
- 即使我們依賴內部的介面，大型的介面會有更多的理由去做異動，這樣測試同樣也是

建議使用假的介面

- 可以控制介面，因為他們不是第三方套件
- 他們會對工作單元或元件做適時的調整

### 4.8.4 介面隔離原則 `Interface Segregation Principle`
- 模組與模組之間的依賴，不應有用不到的功能可以被對方呼叫
- 因為模組之間的依賴不應有用不到的功能，所以我們可以透過介面來進行分割，把模組分得更合符本身的角色，也讓使用介面的角色只能分別接觸到應有的功能

## 4.9 部分模擬物件
> Partial Mocks

### 4.9.1 一個函式部分模擬物件案例
- 我們建立一個真實的 logger，並簡單的常規的真實函式覆寫
```javascript
// 00-password-verifier.ts
import {RealLogger} from "./real-logger";

export class PasswordVerifier {
    private _rules: any[];
    private _logger: RealLogger;

    constructor(rules: any[], logger:RealLogger) {
        this._rules = rules;
        this._logger = logger;
    }

    verify(input: string):boolean{
        const failed = this._rules
            .map(rule => rule(input))
            .filter(result => result === false);

        if (failed.length === 0) {
            this._logger.info('PASSED');
            return true;
        }
        this._logger.info('FAIL');
        return false;
    }
}
```

```javascript
// 02-password-verifier.partial.spec.ts
import { PasswordVerifier } from "./00-password-verifier";
import { RealLogger } from "./real-logger";
import mock = jest.mock;

describe("password verifier with interfaces", () => {
  test("verify, with logger, calls logger", () => {
    const testableLog: RealLogger = new RealLogger();
    let logged = "";
    testableLog.info = (text) => (logged = text);

    const verifier = new PasswordVerifier([], testableLog);
    verifier.verify("any input");

    expect(logged).toMatch(/PASSED/);
  });
});
```

### 4.9.2 物件導向部分模擬物件案例

```javascript
// 02-password-verifier.partial.spec.ts
class TestableLogger extends RealLogger {
  logged = "";
  info(text) {
    this.logged = text;
  }
  //the error() and debug() functions
  // are still "real"
}
describe("partial mock with inhertiance", () => {
  test("verify with logger, calls logger", () => {
    const mockLog: TestableLogger = new TestableLogger();

    const verifier = new PasswordVerifier([], mockLog);
    verifier.verify("any input");

    expect(mockLog.logged).toMatch(/PASSED/);
  });
});
```

## 4.10 關於 Spies 呢?
> What about Spies?

- Spy 就很像是部分 模擬物件/虛設常式










