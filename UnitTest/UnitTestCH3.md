---
title: Unit Test CH3
description: The Art of Unit Testing Third Edition v7
published: true
date: 2022-07-06T01:04:08.703Z
tags: unit test
editor: markdown
dateCreated: 2022-07-05T00:33:00.236Z
---

# CH3 Breaking dependencies with stubs
> - 套件的種類：Mocks、Stubs、其他
> - 使用 Stubs 的理由
> - 注入函數技巧
> - 物件導向注入函數技巧

## 3.1 套件的種類
> Types of Dependencies

![Unit Test two types of dependencies.png](http://192.168.25.60:8000/files/file_storage/2c7b8e41.png)

### Outgoing 套件
- 呈顯工作單元的一個出口點
#### ex：
- 呼叫一個 logger
- 儲存資料到資料庫
- 發送一個電郵
- 偵測到一個 API 的 webhook

### Incoming 套件
- 提供可以測試的資料或行為給工作單元
#### ex：
- 資料庫查詢結果
- 檔案系統的檔案內容
- 網路的回應

### 測試替身 `Test Double`
> Test Double 可以解決兩個常見的測試問題
> - 如何單獨驗證 SUT 的邏輯而不用真的使用到 DOC ？
> - 如何避免測試執行太慢？

- SUT：System Under Test 或 Software Under Test 的簡寫，代表待測程式。如果是單元測試，SUT 就是一個 function 或 method
- DOC：Depended-on Component（相依元件），又稱為 Collaborator（合作者）。DOC 是 SUT 執行的時候會使用到的元件。例如，有一個函數 X 如果執行失敗會寄送 email ，則 email 元件就是函數 X 的 DOC
- Test Double：是一種讓 SUT 可以不依靠 DOC 而單獨被測試的作法，在實作上有五種 Test Double，分別是 Dummy Object、Test Stub、Test Spy、Fake Object、Mock Object

![Unit Test stub and mock.png](http://192.168.25.60:8000/files/file_storage/e485e7d8.png)


## 3.2 使用虛設常式的理由
> Reasons to use stubs

```javascript
// password-verifier-time00.js
const moment = require('moment');
const SUNDAY = 0; 
const SATURDAY = 6;

const verifyPassword = (input, rules) => {
  const dayOfWeek = moment().day();
  if ([SATURDAY, SUNDAY].includes(dayOfWeek)) {
    throw Error("It's the weekend!");
  }
  // more code goes here...
  // return list of errors found..
  return [];
};
```
- 初始化單元測試
```javascript
// password-verifier-time00.spec.js
const moment = require('moment');
const { verifyPassword } = require('./password-verifier-time00');
const { verifyPassword2 } = require('./password-verifier-time00');
const { verifyPassword3 } = require('./password-verifier-time00');
const SUNDAY = 0; 
const SATURDAY = 6; 
const MONDAY = 2;

describe('verifier', () => {
  const TODAY = moment().day();

  // test is always executed, but might not do anything
  test('on weekends, throws exceptions', () => {
    if ([SATURDAY, SUNDAY].includes(TODAY)) {
      expect(() => verifyPassword('anything', []))
        .toThrowError("It's the weekend!");
    }
  });

  // test is not even executed on week days
  if ([SATURDAY, SUNDAY].includes(TODAY)) {
    test('on a weekend, throws an error', () => {
      expect(() => verifyPassword('anything', []))
        .toThrow("It's the weekend!");
    });
  }
});
```

## 3.3 使用大眾化的設計方法在虛設常式
> Generally accepted design approaches to stubbing

- 函式方法
	- 使用函式當成參數
  - 工廠函式 `Factory Functions`，又稱為高階函式 `Higher Order Functions`
  - 構造函式 `Constructor Functions`
- 物件導向方法
	- 類別構造一對一函式 `Class Constructor Injection`
  - 使用物件當作參數，又稱為動態型別語言 `duck typing`
  - 使用公用介面 `Common Interface` 當作參數，通常會使用 typescript

### 3.3.1 把時間使用虛設常式，用參數注入
- 新增一個參數 currentDay
```javascript
const verifyPassword2 = (input, rules, currentDay) => {
  if ([SATURDAY, SUNDAY].includes(currentDay)) {
    throw Error("It's the weekend!");
  }
  // more code goes here...
  // return list of errors found..
  return [];
};
```

```javascript
describe('verifier2 - dummy object', () => {
  test('on weekends, throws exceptions', () => {
    expect(() => verifyPassword2('anything', [], SUNDAY))
      .toThrowError("It's the weekend!");
  });
});
```

![Unit Test Injecting a stub for a time dependency.png](http://192.168.25.60:8000/files/file_storage/2ea95a11.png)

### 3.3.2 套件、注入、控制

#### 套件 `Dependencies`
- 會讓測試程式碼維護變困難，因為無法從測試當中掌控他們
##### ex：
- 時間
- 檔案系統
- 網路
- 任意值

#### 控制 `Control`
- 一個去建構套件如何行為的能力
- 創建套件的人可以說是控制他們的人，因為他們有能力在還未經過測試時配置好套件

#### 控制反轉 `Inversion of Control`
- 設計程式碼將創建套件的責任去除，並使他具體化

#### 依賴注入 `Dependency Injection`
- 透過設計的介面將套件發送到程式碼裡面使用的行為

#### 接縫 `Seam`
- 兩個軟體相互溝通，其他可以被注入的東西
- 程式碼中可以抽換不同功能的地方
##### ex：
- 使用虛設常式或模擬物件類別
- 增加一個建構函式（Constructor）參數
- 增加一個可設定的公開屬性
- 把一個方法改成可供覆寫的虛擬方法
- 把一個委派拉出來變成一個參數或屬性供類別外部來決定內容

## 3.4 注入函數技巧
> Functional Injection Techniques

### 3.4.1 注入一個函式
- 新增一個函式作為參數
```javascript
const verifyPassword3 = (input, rules, getDayFn) => {
  const dayOfWeek = getDayFn();
  if ([SATURDAY, SUNDAY].includes(dayOfWeek)) {
    throw Error("It's the weekend!");
  }
  // more code goes here...
  // return list of errors found..
  return [];
};
```

```javascript
describe('verifier3 - dummy function', () => {
  test('on weekends, throws exceptions', () => {
    const alwaysSunday = () => SUNDAY;
    expect(() => verifyPassword3('anything', [], alwaysSunday))
      .toThrowError("It's the weekend!");
  });
  test('on week days, works fine', () => {
    const alwaysMonday = () => MONDAY;

    const result = verifyPassword3('anything', [], alwaysMonday);

    expect(result.length).toBe(0);
  });
});
```

### 3.4.2 使用工廠函式
- 工廠函式或方法：函式回傳其他函式，並預配置一些內容

```javascript
const makeVerifier = (rules, dayOfWeekFn) => {
  return function (input) {
    if ([SATURDAY, SUNDAY].includes(dayOfWeekFn())) {
      throw new Error("It's the weekend!");
    }
    // use the rules, luke
    // more code goes here..
  };
};
```

```javascript
// factory method demo
const { makeVerifier } = require('./password-verifier-time01');

describe('verifier', () => {
  test('factory method: on weekends, throws exceptions', () => {
    const alwaysSunday = () => SUNDAY;
    const context = {
      verify: makeVerifier([], alwaysSunday)
    };

    expect(() => context.verify('anything'))
      .toThrow("It's the weekend!");
  });
```

### 3.4.3 使用構造函式
- 使用高階函式

```javascript
// constructor function pattern
const Verifier = function(rules, dayOfWeekFn)
{
    this.verify = function (input) {
        if ([SATURDAY, SUNDAY].includes(dayOfWeekFn())) {
            throw new Error("It's the weekend!");
        }
        //more code goes here..
    };
};
```
- 使用 new 呼叫函式
```javascript
//constructor function demo
const {Verifier} = require("./password-verifier-time01");
test('constructor function: on weekends, throws exception‘, () => {
    const alwaysSunday = () => SUNDAY;
    const verifier = new Verifier([], alwaysSunday);
    expect(()=> verifier.verify('anything'))
        .toThrow("It's the weekend!");
});
```

## 3.5 物件導向注入技巧

### 3.5.1 構造函式注入
- 透過構造一個類別去注入依賴
```javascript
const SUNDAY = 0; const MONDAY = 1; const SATURDAY = 6;
class PasswordVerifier {
  constructor (rules, timeProvider) {
    this.rules = rules;
    this.timeProvider = timeProvider;
  }

  verify (input) {
    if ([SATURDAY, SUNDAY].includes(this.timeProvider.getDay())) {
      throw new Error("It's the weekend!");
    }
    const errors = [];
    // more code goes here..
    return errors;
  }
}

module.exports = {
  SUNDAY,
  MONDAY,
  SATURDAY,
  PasswordVerifier
};
```
- 初版
```javascript
test('class constructor: on weekends, throws exception', () => {
    const alwaysSunday = () => SUNDAY;
    const verifier = new PasswordVerifier([], alwaysSunday);
    expect(()=> verifier.verify('anything'))
        .toThrow("It's the weekend!");
});
```
- 新增一個工廠函式
```javascript
describe('refactored with constructor', () => {
   const makeVerifier = (rules, dayFn) => {
        return new PasswordVerifier(rules, dayFn);
    };
    test('class constructor: on weekends, throws exceptions', () => {
        const alwaysSunday = () => SUNDAY;
        const verifier = makeVerifier([],alwaysSunday);
        expect(()=> verifier.verify('anything'))
            .toThrow("It's the weekend!");
    });
    test('class constructor: on weekdays, with no rules, passes', () => {
        const alwaysMonday = () => MONDAY;
        const verifier = makeVerifier([],alwaysMonday);
        const result = verifier.verify('anything');
        expect(result.length).toBe(0);
    });
});
```

### 3.5.2 注入一個物件而不是函式

```javascript
// time-provider.js
import moment from 'moment';

const RealTimeProvider = () => {
  this.getDay = () => moment().day();
};

module.exports = {
  RealTimeProvider
};
```

```javascript
// password-verifier-time02.js
const SUNDAY = 0; const MONDAY = 1; const SATURDAY = 6;
class PasswordVerifier {
  constructor (rules, timeProvider) {
    this.rules = rules;
    this.timeProvider = timeProvider;
  }

  verify (input) {
    if ([SATURDAY, SUNDAY].includes(this.timeProvider.getDay())) {
      throw new Error("It's the weekend!");
    }
    const errors = [];
    // more code goes here..
    return errors;
  }
}

module.exports = {
  SUNDAY,
  MONDAY,
  SATURDAY,
  PasswordVerifier
};
```

```javascript
// verifier-factory.js
const { RealTimeProvider } = require('./time-provider');
const { PasswordVerifier } = require('./password-verifier-time02');

const passwordVerifierFactory = (rules) => {
  return new PasswordVerifier(new RealTimeProvider());
};

module.exports = {
  passwordVerifierFactory
};
```
- 手寫一個虛設常式物件
```javascript
// password-verifier-time02.spec.js
const { SUNDAY, MONDAY } = require('./password-verifier-time02');
// class constructor demo
const { PasswordVerifier } = require('./password-verifier-time02');

function FakeTimeProvider (fakeDay) {
  this.getDay = function () {
    return fakeDay;
  };
}
const makeVerifier = (rules, timeProvider) => {
  return new PasswordVerifier(rules, timeProvider);
};

describe('verifier', () => {
  test('on weekends, throws exceptions, ctor function', () => {
    const verifier = makeVerifier([], new FakeTimeProvider(SUNDAY));

    expect(() => verifier.verify('anything'))
      .toThrow("It's the weekend!");
  });

  test('on weekdays, with no rules, always passes', () => {
    const verifier = makeVerifier([], new FakeTimeProvider(MONDAY));

    const result = verifier.verify('anything');
    expect(result.length).toBe(0);
  });
});
```

### 3.5.3 萃取出一個共同的介面

```javascript
// time-provider-interface.ts 
export interface TimeProviderInterface {
    getDay(): number;
}
```

```javascript
// real-time-provider.ts
import * as moment from "moment";
import {TimeProviderInterface} from "./time-provider-interface";

export class RealTimeProvider implements TimeProviderInterface {
    getDay(): number {
        return moment().day();
    }
}
```

```javascript
// password-verifier-time03.ts
import {TimeProviderInterface} from "./time-provider-interface";
export const SUNDAY = 0, SATURDAY=6;

export class PasswordVerifier {
    private _timeProvider: TimeProviderInterface;

    constructor(rules: any[], timeProvider: TimeProviderInterface) {
        this._timeProvider = timeProvider;
    }

    verify(input: string):string[] {
        const isWeekened = [SUNDAY, SATURDAY]
            .filter(x=>x=== this._timeProvider.getDay())
            .length>0;
        if (isWeekened) {
            throw new Error("It's the weekend!")
        }
        return [];
    }
}
```

```javascript
// password-verifier-time03.spec.ts
import {TimeProviderInterface} from "./time-provider-interface";
import {PasswordVerifier, SUNDAY} from "./password-verifier-time03";

class FakeTimeProvider implements TimeProviderInterface{
    fakeDay: number;
    getDay(): number {
        return this.fakeDay;
    }
}

describe('password verifier with interfaces', () => {
    test('on weekends, throws exceptions', () => {
        const stubTimeProvider = new FakeTimeProvider();
        stubTimeProvider.fakeDay = SUNDAY;
        const verifier = new PasswordVerifier([], stubTimeProvider);

        expect(()=> verifier.verify('anything'))
            .toThrow("It's the weekend!");
    });
});
```


