---
title: Unit Test CH8
description: The Art of Unit Testing Third Edition v7
published: true
date: 2022-07-27T08:56:10.225Z
tags: unit test
editor: markdown
dateCreated: 2022-07-21T02:19:05.229Z
---

# CH8 Maintainability
> - 測試失敗的根本原因
> - 測試程式碼常見應避免的變動
> - 當現行測試不會失敗時如何增進維護性

## 8.1 測試變動根本原因
> Root causes for test changes
- 什麼時候會注意到測試可能會需要一個異動?(例如當測試失敗時)
- 為什麼測試失敗?
- 怎樣的測試失敗導致我們需要去異動測試?
- 什麼時候我們選擇去異動一個測試，即使我們不是被強迫去做的

## 8.2 失敗的測試被迫要去異動
- 測試可能會因為好多原因突然失敗

### 8.2.1 當有產品程式碼的 bug 時測試被破壞(好事!)
- 一個產品的 bug 會產生，是當你改變產品程式碼的時候，且現存的測試被破壞了
- 要是在測試之後真的有 bug，這樣測試是沒有問題的，不需要異動測試，這是有測試最好的成果

### 8.2.2 當一個測試有 bug 時測試被破壞(增加信心)
- 要盡量去避免讓測試有 bug

### 8.2.3 當一個測試不重要或與其他測試有衝突時測試被破壞(移除)
- 一個產品程式碼有一個新功能時，但卻跟一個或多個現存的測試有衝突。這表示不是測試發現一個 bug，而是發現衝突或新的需求
- 在測試驅動模式，一個新的測試被寫出來是為了反映一個新需求，並且我們藉由新增新需求讓他通過，但一個舊的測試開始失敗，這代表在產品程式碼中一個舊的預期應該如何運作
- 這代表是現有失敗的測試不再失敗或新的需求是錯誤的，假設需求是正確的，我們可能要刪除不再重要的測試

### 8.2.4 當產品程式碼中的語意或 API 有異動時測試被破壞
- 當產品程式碼異動時測試可能會失敗，一個函式或物件在測試時有不同的使用，即使他可能有相同的功能
- 有一個 PasswordVerifier 類別，需要兩個建構子參數
 	- rules 的陣列
  - ILogger 的介面

```javascript
// logger.ts
export interface ILogger {
    info(text: string);
}
```

```javascript
// 00-password-verifier.ts
import { ILogger } from "./interfaces/logger";

export class PasswordVerifier {
  private _rules: ((input: string) => boolean)[];
  private _logger: ILogger;

  constructor(rules: ((input) => boolean)[], logger: ILogger) {
    this._rules = rules;
    this._logger = logger;
  }

  verify(input: string): boolean {
    const failed = this._rules
      .map((rule) => rule(input))
      .filter((result) => result === false);

    if (failed.length === 0) {
      this._logger.info("PASSED");
      return true;
    }
    this._logger.info("FAIL");
    return false;
  }
}
```

```javascript
// 00-password-verifier.v1.spec.ts
import { PasswordVerifier } from "./00-password-verifier";

describe("password verifier 1", () => {
  it("passes with zero rules", () => {
    const verifier = new PasswordVerifier([], { info: jest.fn() });
    const result = verifier.verify("any input");
    expect(result).toBe(true);
  });

  it("fails with single failing rule", () => {
    const failingRule = (input) => false;
    const verifier = new PasswordVerifier([failingRule], { info: jest.fn() });
    const result = verifier.verify("any input");
    expect(result).toBe(false);
  });
});
```
- 程式碼必須能長久使用
- 當我們使用 IComplicatedLogger，而不是 ILogger 的時候

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
import { IComplicatedLogger } from "./interfaces/complicated-logger";

export class PasswordVerifier2 {
  private _rules: ((input: string) => boolean)[];
  private _logger: IComplicatedLogger;

  constructor(rules: ((input) => boolean)[], logger: IComplicatedLogger) {
    this._rules = rules;
    this._logger = logger;
  }

  verify(input: string): boolean {
    const failed = this._rules
      .map((rule) => rule(input))
      .filter((result) => result === false);

    if (failed.length === 0) {
      this._logger.info("PASSED");
      return true;
    }
    this._logger.info("FAIL");
    return false;
  }
}
```
- 工廠函式會在測試下解耦物件的建立 ( factory functions decouple creation of object under test )

```javascript
// 00-password-verifier.v2.spec.ts
import { PasswordVerifier } from "./00-password-verifier";
import { ILogger } from "./interfaces/logger";

describe("password verifier 1", () => {
  const makeFakeLogger = () => {
    // A centralized point for creating a fake logger
    return { info: jest.fn() };
  };

  const makePasswordVerifier = (
    rules: ((input) => boolean)[],
    fakeLogger: ILogger = makeFakeLogger()
  ) => {
    // A centralized point for creating a PasswordVerifier
    return new PasswordVerifier(rules, fakeLogger);
  };

  it("passes with zero rules", () => {
    // Using factory functions to create PasswordVerifier
    const verifier = makePasswordVerifier([]);

    const result = verifier.verify("any input");

    expect(result).toBe(true);
  });

  it("fails with single failing rule", () => {
    const failingRule = (input) => false;
    // Using factory functions to create PasswordVerifier
    const verifier = makePasswordVerifier([failingRule]);

    const result = verifier.verify("any input");

    expect(result).toBe(false);
  });
});
```

- 重構工廠方法去適應新的簽章

```javascript
// 00-password-verifier.v3.spec.ts
import { PasswordVerifier } from "./00-password-verifier";
import { IComplicatedLogger } from "./interfaces/complicated-logger";
import { Substitute } from "@fluffy-spoon/substitute";
import { PasswordVerifier2 } from "./00-password-verifier2";

describe("password verifier (ctor change)", () => {
  const makeFakeLogger = () => {
    return Substitute.for<IComplicatedLogger>();
  };

  const makePasswordVerifier = (
    rules: ((input) => boolean)[],
    fakeLogger: IComplicatedLogger = makeFakeLogger()
  ) => {
    return new PasswordVerifier2(rules, fakeLogger);
  };

  it("passes with zero rules", () => {
    const verifier = makePasswordVerifier([]);

    const result = verifier.verify("any input");

    expect(result).toBe(true);
  });

  it("fails with single failing rule", () => {
    const failingRule = (input) => false;
    const verifier = makePasswordVerifier([failingRule]);

    const result = verifier.verify("any input");

    expect(result).toBe(false);
  });
});
```

### 8.2.5 當其他測試有異動時測試被破壞(降低測試的耦合性)
- 受時間約束的測試順序：當一個測試假設一個預先的測試要被執行，而沒有被先執行，因為他依賴其他測試設定的共享狀態
- 若是一個測試改變了記憶體中或外部資源(如資料庫)中的共享狀態，且其他測試依賴此變數的值當第一個測試執行之後，基於順序性測試之間我們會有一個依賴性
- 一個 SpecialApp 物件使用一個 UserCache 物件，User Cache 掌握一個單獨被一個緩存機制分享的實例

![Unit Test A shared user cache instance.png](http://192.168.25.60:8000/files/file_storage/3ef10c44.png)

```javascript
// shared user cache and associated interfaces
// sharedUserCache.ts
export interface IUserDetails {
  key: string;
  password: string;
}

export interface IUserCache {
  addUser(user: IUserDetails): void;
  getUser(key: string);
  reset(): void;
}

export class UserCache implements IUserCache {
  users: object = {};
  addUser(user: IUserDetails): void {
    if (this.users[user.key] !== undefined) {
      throw new Error("user already exists");
    }
    this.users[user.key] = user;
  }

  getUser(key: string) {
    return this.users[key];
  }

  reset(): void {
    this.users = {};
  }
}

let _cache: IUserCache;
export function getUserCache() {
  if (_cache === undefined) {
    _cache = new UserCache();
  }
  return _cache;
}
```

```javascript
// SpecialApp implementation
// specialApp.ts
import { getUserCache, IUserCache, IUserDetails } from "./sharedUserCache";

export class SpecialApp {
  loginUser(key: string, pass: string): boolean {
    const cache: IUserCache = getUserCache();
    const foundUser: IUserDetails = cache.getUser(key);
    if (foundUser?.password === pass) {
      return true;
    }
    return false;
  }
}
```

```javascript
// tests that need to run in a specific order
// specialApp.spec.ts
import { getUserCache } from "./sharedUserCache";
import { SpecialApp } from "./specialApp";

describe("Test Dependence", () => {
  describe("loginUser with loggedInUser", () => {
    test("A: no user, login fails", () => {
      const app = new SpecialApp();
      const result = app.loginUser("a", "abc");
      expect(result).toBe(false);
    });

    test("B: can only cache each user once", () => {
      getUserCache().addUser({
        key: "a",
        password: "abc",
      });

      expect(() =>
        getUserCache().addUser({
          key: "a",
          password: "abc",
        })
      ).toThrowError("already exists");
    });

    test("C: user exists, login succeeds", () => {
      const app = new SpecialApp();
      const result = app.loginUser("a", "abc");
      expect(result).toBe(true);
    });
  });
});
```

- 測試 A 需要測試 B 還沒執行，因為只有在測試 B 中一個使用者會被加到緩存中
- 測試 C 需要測試 B 已經執行，因為他需要一個使用者已經存在在緩存中
- 測試 C 若是只單純使用 it.only 會失敗
```javascript
test.only("C: user exists, login succeeeds () => {
   const app = new SpecialApp();
   const result = app.loginUser("a", "abc");
   expect(result).toBe(true);  /// ß wOULD FAIL
 });
```
- 這種反模式 `anti-pattern` 通常會發生在我們嘗試去重複使用測試中的一部分，而沒有抽離出輔助函式

```javascript
// Refactored tests to remove order dependence
// specialApp.v2.spec.ts
import { getUserCache } from "./sharedUserCache";
import { SpecialApp } from "./specialApp";
// Extracted user creation helper function
const addDefaultUser = () =>
  getUserCache().addUser({
    key: "a",
    password: "abc",
  });
// Extracted factory function
const makeSpecialApp = () => new SpecialApp();

describe("Test Dependence v2", () => {
  // Reset user cache between tests
  beforeEach(() => getUserCache().reset());
  describe("user cache", () => {
    test("B: can only cache each user once", () => {
      // call reusable helper functions
      addDefaultUser();

      expect(() => addDefaultUser()).toThrowError("already exists");
    });
  });
  describe("loginUser with loggedInUser", () => {
    test("A: user exists, login succeeds", () => {
      addDefaultUser();
      const app = makeSpecialApp();

      const result = app.loginUser("a", "abc");
      expect(result).toBe(true);
    });

    test("C: no user , login fails", () => {
      const app = makeSpecialApp();

      const result = app.loginUser("a", "abc");
      expect(result).toBe(false);
    });
  });
});
```
- 抽離出兩個輔助函式：一個工廠函式 makeSpecialApp，一個輔助函式 addDefaultUser，可以重複使用
- 在每個測試前使用 beforeEach 重置使用者緩存。不管我們有沒有共用的資源，我們可以在測試執行前或後使用 beforeEach 或 afterEach 重置
- 測試 A 與 C 在各自的巢狀 describe 結構中執行，他們皆使用工廠函式 makeSpecialApp，且其中一個是使用 addDefaultUser 去確保他不需要任何其他測試先執行
- 測試 B 在自己的巢狀 describe 中執行，且重複使用 addDefaultUser 函式

## 8.3 進行重構來增加維護性
> Refactoring to increase maintainability

### 8.3.1 避免測試私有或保護的方法
- 若你看到一個私有的方法，且發現系統中公開的地方會使用到，若只單純測試私有的方法且可行，這也不表示系統剩下的部分也是正確使用這個私有的方法，或他提供的結果是正確的。
- 通常一個私有的方法若是值得測試的，他應該是值得被公開的或靜態的。
- 使方法公開化：這可能會牴觸物件導向的原則，藉由公開化，會使得他變成官方的，藉由私有化，會使得後面的開發人員可以改變他的實作而不用擔心有哪些程式碼會使用到他。
- 抽取出方法到新的類別或模組：若一個方法有很多屬於自己的邏輯，或他使用類別裡特殊的狀態變數，或模組跟方法是沒有關係的，這樣可以抽取出方法到一個新的類別或他自己的模組，在系統中扮演一個特別的角色。
- 讓無狀態的私有方法變成公開靜態的：若一個方法是完全無狀態的，有些人會選擇去用靜態的方式重構方法，這樣會更加好測試。

### 8.3.2 移除重複的部分
- 若是一個重複的部分需要修改，這樣會強迫也需要修改所有重複的部分
- 使用輔助函數會有效的在測試中減少重複的部分

### 8.3.3 避免設置方法 `setup methods` (站在維護的角度)
- 設置函式 `setup function`太容易被濫用，所以開發人員傾向於去使用他卻並不代表著些什麼，這樣測試會變的更難閱讀更難維護
- 幾個濫用設置函式的方式：
	- 初始化物件在設置方法中，只有在一些測試中會使用到
	- 有長一大段的設置方法程式碼，變得難以理解
  - 在設置方法中使用模擬物件與假物件
- 設置方法會有一些限制，而可以使用更簡單的輔助方法：
	- 設置方法只有在初始化東西的時候有幫助
  - 設置方法不是移除重複部分最好的選擇，移除重複性不是一直是關於建立或初始化物件的事情，通常是移除重複的斷言邏輯
  - 設置方法不能有參數或回傳值
  - 設置方法不能像工廠方法一樣回傳值，他們通常在測試之前執行，所以他們會用一個通用的方法執行。測試有時候需要去請求一些特別的東西或使用參數呼叫一個共用的程式碼(例如取回一個物件並設置他的屬性為一個特別的值)
	- 設置方法在現有的測試類別中應該只包含了應用在所有測試的程式碼，或方法會變得難以閱讀與理解
  
### 8.3.4 使用參數化的測試去移除重複性
- 另外一個取代設置方法讓測試看起來一致的方法是使用參數化的測試 `parameterized test`
- 不同測試框架會有不同的參數化測試支援，若使用 jest，可以使用 test.each 或 it.each

```javascript
// parametrized tests with jest
// parametrized.spec.ts
const sum = (numbers) => {
  if (numbers.length > 0) {
    return parseInt(numbers);
  }
  return 0;
};

describe("sum with regular tests", () => {
  test("sum number 1", () => {
    const result = sum("1");
    expect(result).toBe(1);
  });
  test("sum number 2", () => {
    const result = sum("2");
    expect(result).toBe(2);
  });
});

describe("sum with parametrized tests", () => {
  // providing two parameters to the test function that will be mapped to the arrays.
  // invoke a new function with the braces opening.
  test.each([
    ["1", 1],
    ["2", 2],
  ])("add, for each number %s it returns that number", (input, expected) => {
    const result = sum(input);
    expect(result).toBe(expected);
  });
});
```

## 8.4 避免超規格
> Avoid overspecification

- 一個測試斷言一個物件中純粹的內部狀態
- 一個測試使用多個模擬物件
- 一個測試同時使用模擬物件與虛設常式
- 一個測試假設一個特別的順序或抽取出字串匹配當他其實是不需要的

### 8.4.1 模擬物件中內部超規格的行為
- 呼叫一個受保護的函式 findFailedRules()，去得到一個結果並做計算

```javascript
// Production code that calls a protected function
// 00-password-verifier4.ts
import { IComplicatedLogger } from "./interfaces/complicated-logger";

export class PasswordVerifier4 {
  private _rules: ((input: string) => boolean)[];
  private _logger: IComplicatedLogger;

  constructor(rules: ((input) => boolean)[], logger: IComplicatedLogger) {
    this._rules = rules;
    this._logger = logger;
  }

  verify(input: string): boolean {
    const failed = this.findFailedRules(input);

    if (failed.length === 0) {
      this._logger.info("PASSED");
      return true;
    }
    this._logger.info("FAIL");
    return false;
  }

  protected findFailedRules(input: string) {
    const failed = this._rules
      .map((rule) => rule(input))
      .filter((result) => result === false);
    return failed;
  }
}
```

```javascript
// over specified test verifying call to protected function
// overspec.1.spec.ts
import { PasswordVerifier4 } from "./00-password-verifier4";
import { Substitute } from "@fluffy-spoon/substitute";
import { IComplicatedLogger } from "./interfaces/complicated-logger";

describe("verifier 4", () => {
  describe("overcpecify protected function call", () => {
    test("checkfailedFules is called", () => {
      const pv4 = new PasswordVerifier4(
        [],
        Substitute.for<IComplicatedLogger>()
      );
      // the fake function returns an empty array
      const failedMock = jest.fn(() => []);
      pv4["findFailedRules"] = failedMock;

      pv4.verify("abc");

      //Don't do this
      expect(failedMock).toHaveBeenCalled();
    });
  });
});
```

### 8.4.2 抽取出輸出值與超規格的順序
```javascript
// a verifier that returns a list of outputs
// 00-password-verifier5.ts
interface IResult {
  result: boolean;
  input: string;
}

export class PasswordVerifier5 {
  private _rules: ((input: string) => boolean)[];

  constructor(rules: ((input) => boolean)[]) {
    this._rules = rules;
  }

  verify(inputs: string[]): IResult[] {
    const failedResults = inputs.map((input) => this.checkSingleInput(input));
    return failedResults;
  }

  private checkSingleInput(input: string) {
    const failed = this.findFailedRules(input);
    return {
      input,
      result: failed.length === 0,
    };
  }

  protected findFailedRules(input: string) {
    const failed = this._rules
      .map((rule) => rule(input))
      .filter((result) => result === false);
    return failed;
  }
}
```

- 我們函式回傳一個 IResult 的陣列，包含了 input 與 result，以下為測試

```javascript

// overspec.2.spec.ts
import { PasswordVerifier5 } from "./00-password-verifier5";
describe("verifier 5", () => {
  describe("overcpecify outputs", () => {
    // overcpecify order and structure (schema)
    test("overspecify order and schema", () => {
      const pv5 = new PasswordVerifier5([(input) => input.includes("abc")]);
      const results = pv5.verify(["a", "ab", "abc", "abcd"]);
      expect(results).toEqual([
        { input: "a", result: false },
        { input: "ab", result: false },
        { input: "abc", result: true },
        { input: "abcd", result: true },
      ]);
    });
		// ignore schema of results
    test("overspecify order but ignore schema", () => {
      const pv5 = new PasswordVerifier5([(input) => input.includes("abc")]);
      const results = pv5.verify(["a", "ab", "abc", "abcd"]);
      expect(results.length).toBe(4);
      expect(results[0].result).toBe(false);
      expect(results[1].result).toBe(false);
      expect(results[2].result).toBe(true);
      expect(results[3].result).toBe(true);
    });
		// ignoring order and schema
    test("ignore order and schema", () => {
      const pv5 = new PasswordVerifier5([(input) => input.includes("abc")]);

      const results = pv5.verify(["a", "ab", "abc", "abcd"]);

      const falseResults = results.filter((res) => !res.result);
      const trueResults = results.filter((res) => res.result);
      expect(results.length).toBe(4);
      expect(falseResults.length).toBe(2);
      expect(trueResults.length).toBe(2);
    });
  });
});
```

- 超規格的字串
- 有一個驗證密碼，當驗證時很多規則被破壞時會給我們訊息

```javascript
// a verifier that returns a string message
// 00-password-verifier6.ts
interface IResult {
  result: boolean;
  input: string;
}

export class PasswordVerifier6 {
  private _rules: ((input: string) => boolean)[];
  private _msg: string = "";

  constructor(rules: ((input) => boolean)[]) {
    this._rules = rules;
  }

  getMsg(): string {
    return this._msg;
  }

  verify(inputs: string[]): IResult[] {
    const allResults = inputs.map((input) => this.checkSingleInput(input));
    this.setDescription(allResults);
    return allResults;
  }

  private setDescription(results: IResult[]) {
    const failed = results.filter((res) => !res.result);
    this._msg = `you have ${failed.length} failed rules.`;
  }

  private checkSingleInput(input: string) {
    const failed = this.findFailedRules(input);
    return {
      input,
      result: failed.length === 0,
    };
  }

  protected findFailedRules(input: string) {
    const failed = this._rules
      .map((rule) => rule(input))
      .filter((result) => result === false);
    return failed;
  }
}
```

```javascript
// over specifying a string using equality
// overspec.3.spec.ts
import { PasswordVerifier6 } from "./00-password-verifier6";

describe("verifier 6", () => {
  test("over specify string", () => {
    const pv5 = new PasswordVerifier6([(input) => input.includes("abc")]);

    pv5.verify(["a", "ab", "abc", "abcd"]);
    const msg = pv5.getMsg();
    expect(msg).toBe("you have 2 failed rules.");
  });
  test("more future proof string checking", () => {
    const pv5 = new PasswordVerifier6([(input) => input.includes("abc")]);

    pv5.verify(["a", "ab", "abc", "abcd"]);
    const msg = pv5.getMsg();
    expect(msg).toMatch(/2 failed/);
  });
});
```










