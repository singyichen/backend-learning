---
title: Unit Test CH2
description: The Art of Unit Testing Third Edition v7
published: true
date: 2022-07-05T00:17:39.629Z
tags: unit test
editor: markdown
dateCreated: 2022-06-28T05:50:36.692Z
---

# CH2 A first unit test
> - 用 Jest 寫第一個測試
> - 測試結構 & 命名原則
> - 使用斷言 `assertion` API
> - 重構測試與減少重複的程式碼

## 2.1 關於 Jest
> About Jest

- 由 Facebook 開發的開放原始碼測試框架
- 原本被使用在測試前端物件庫 React，目前被廣泛的使用在後端與前端測試

### 2.1.1 準備開發環境

https://nodejs.org/en/download/

### 2.1.2 準備開發資料夾
```shell
mkdir ch2
cd ch2
npm init --yes
//or
yarn init –yes
git init
```

### 2.1.3 安裝 Jest
```shell
npm install --save-dev jest
//or
yarn add jest –dev
```
```shell
npm install -g jest
```

### 2.1.4 建一個 test 資料夾
- hellojest.test.js
```javascript
test('hello jest', () => {
  expect('hello').toEqual('hello');
});
```

### 2.1.5 執行 Jest
- 開發者模式
```shell
jest --watch
```

## 2.3 單元測試框架提供什麼
> What unit testing frameworks offer

### 框架可以提供的部分
- 結構 ( Structure )：可以寫一個有完善定義的測試，每個人都可以理解、閱讀
- 可重複性 ( Repeatability )：可以重複執行測式，可以理解錯誤並知道為什麼
- 經得起時間的考驗 ( Confidence through time savings )：使用現有的框架會較少有 bug
### 執行一個或多個單元測試
- 辨識出測試程式碼
- 自動化執行測試
- 執行時顯示狀態
### 可以提供測試結果
- 有幾個測試執行
- 有幾個測試未執行
- 有幾個測試失敗
- 哪些測試失敗
- 測試失敗的理由
- 在程式的第幾行出錯
- 在發生例外的地方進行偵測，並讓你知道呼叫了哪些方法

### 2.3.1 xUnit 框架
- CppUnit：C++
- JUnit：Java
- NUnit：.NET
- HUint：Haskell

### 2.3.2 XUnit,TAP 與 Jest 結構


## 2.4 介紹密碼驗證專案
> Introducing the Password Verifier Project

- 密碼驗證函式：verifyPassword(rules)

```javascript
// password-verifier0.js
const verifyPassword = (input, rules) => {
  const errors = [];
  rules.forEach((rule) => {
    const result = rule(input);
    if (!result.passed) {
      errors.push(`error ${result.reason}`);
    }
  });
  return errors;
};

module.exports = {
  verifyPassword,
};
```

## 2.5 第一個密碼驗證的測試
> The first Jest test for verifyPassword

```javascript
// password-verifier0.spec.js
const { verifyPassword } = require('../password-verifier0');
```
- 使用 AAA 模式：Arrrage、Act、Assert
	- Arrrage：安排：所有使系統達到測試所要模擬的情境的程式。這可能包含實體化某個待測單元的建構子、新增 DB 的資料、mocking/stubbing 物件和其他準備程式
	- Act：執行：執行測試單元。通常為一行程式
	- Assert：斷言：確保得到的值符合期待。通常為一行程式

```javascript
test('v1: the first test', () => {
  // Arrrage：setup the scenario under test
  const fakeRule = (input) => ({ passed: false, reason: 'fake reason' });

  // Act：invoke the entry point with inputs
  const errors = verifyPassword('any value', [fakeRule]);

  // Assert：check the exit point
  expect(errors[0]).toMatch('fake reason');
});
```
### 2.5.1 執行測試

### 2.5.2 使用命名
- 第一個測試名稱沒有說明是要完全什麼測試
- 測試名稱重新命名，讓人不需要看程式碼也可以知道在進行怎樣的測試

```javascript
test('v1.1 verifyPassword, given a failing rule, returns errors', () => {
  const fakeRule = (input) => ({ passed: false, reason: 'fake reason' });

  const errors = verifyPassword('any value', [fakeRule]);

  expect(errors[0]).toContain('fake reason');
});
```

### 2.5.3 字串比較與可維護性
- [.toMatch(regexp | string)](https://jestjs.io/docs/expect#tomatchregexp--string)
- [.toContain(item)](https://jestjs.io/docs/expect#tocontainitem)

### 2.5.4 使用 describe( )
- describe( )：為相同函式的測試內容做分類

```javascript
describe('v1.2: verifyPassword', () => {
  test('given a failing rule, returns errors', () => {
    const fakeRule = (input) => ({ passed: false, reason: 'fake reason' });

    const errors = verifyPassword('any value', [fakeRule]);

    expect(errors[0]).toContain('fake reason');
  });
});
```
### 2.5.5 結構代表著情境
- 使用多層 describe( )進行分類
```javascript
describe('v1.3: verifyPassword', () => {
  describe('with a failing rule', () => {
    test('returns errors', () => {
      const fakeRule = (input) => ({
        passed: false,
        reason: 'fake reason',
      });
      const errors = verifyPassword('any value', [fakeRule]);

      expect(errors[0]).toContain('fake reason');
    });
  });
});
```

- 將共用的部分提出
```javascript
describe('v1.4: verifyPassword', () => {
  describe('with a failing rule', () => {
    const fakeRule = (input) => ({
      passed: false,
      reason: 'fake reason',
    });
    test('returns errors', () => {
      const errors = verifyPassword('any value', [fakeRule]);

      expect(errors[0]).toContain('fake reason');
    });
  });
});
```

### 2.5.6 It 函式
- it( ) 跟 test( )為相同函式 

```javascript
describe('v1.5: verifyPassword', () => {
  describe('with a failing rule', () => {
    it('returns errors', () => {
      const fakeRule = (input) => ({
        passed: false,
        reason: 'fake reason',
      });
      const errors = verifyPassword('any value', [fakeRule]);

      expect(errors[0]).toContain('fake reason');
    });
  });
});
```

### 2.5.7 兩種 Jest 測試寫法
- TDD：
	- 先寫測試再開發。
	- 依循「紅燈／綠燈／重構」循環（Red/Green/Refactor）。
	- 在初期就確保測試程式的撰寫，而且容易在初期定義出更貼近使用方的介面。
- BDD：
	- 在寫測試前先寫測試規格書。
	- 使用更接近人類語意的方式來描述測試情境。
	-	這份規格書是一份「可以被執行的規格」，可以被轉成自動化測試，甚至產生報表。
  
### 2.5.8 重構產品的程式碼
- 將 function 重構為一個有狀態的類別
```javascript
// password-verifier1.js
class PasswordVerifier1 {
  constructor() {
    this.rules = [];
  }

  addRule(rule) {
    this.rules.push(rule);
  }

  verify(input) {
    if (this.rules.length === 0) {
      throw new Error('There are no rules configured');
    }
    const errors = [];
    this.rules.forEach((rule) => {
      const result = rule(input);
      if (result.passed === false) {
        errors.push(result.reason);
      }
    });
    return errors;
  }
}

module.exports = { PasswordVerifier1 };
```
- 測試有狀態的工作單元
```javascript
// password-verifier1.spec.js
const { PasswordVerifier1 } = require('../password-verifier1');
```
```javascript
describe('PasswordVerifier', () => {
  describe('with a failing rule', () => {
    it('has an error message based on the rule.reason', () => {
      const verifier = new PasswordVerifier1();
      const fakeRule = (input) => ({
        passed: false,
        reason: 'fake reason',
      });

      verifier.addRule(fakeRule);
      const errors = verifier.verify('any value');

      expect(errors[0]).toContain('fake reason');
    });
  });
});
```
- 新增確認只有單一一個錯誤：expect(errors.length).toBe(1);
```javascript
describe('v2 PasswordVerifier', () => {
  describe('with a failing rule', () => {
    it('has an error message based on the rule.reason', () => {
      const verifier = new PasswordVerifier1();
      const fakeRule = input => ({
        passed: false,
        reason: 'fake reason'
      });

      verifier.addRule(fakeRule);
      const errors = verifier.verify('any value');
			// if this line fail,the test runner will move on to the next case,then the next line never executes
      expect(errors.length).toBe(1);
      expect(errors[0]).toContain('fake reason');
    });
  });
});
```
- 確認同一個出口點的其他結果：將測試進行拆分
```javascript
describe('v3 PasswordVerifier', () => {
  describe('with a failing rule', () => {
    it('has an error message based on the rule.reason', () => {
      const verifier = new PasswordVerifier1();
      const fakeRule = input => ({
        passed: false,
        reason: 'fake reason'
      });

      verifier.addRule(fakeRule);
      const errors = verifier.verify('any value');

      expect(errors[0]).toContain('fake reason');
    });
    it('has exactly one error', () => {
      const verifier = new PasswordVerifier1();
      const fakeRule = input => ({
        passed: false,
        reason: 'fake reason'
      });

      verifier.addRule(fakeRule);
      const errors = verifier.verify('any value');

      expect(errors.length).toBe(1);
    });
  });
});
```

### 2.6 使用 beforeEach( )
> Trying the beforeEach( ) route

scoping
- 每個 .test.js 測試檔案的內容就如一般的 JavaScript ，依照 Function 的範圍分成全域及區域的執行。
- 在測試中，執行範圍會影響到的除了變數外還有另外幾個 Jest 提供的函式：
	- beforeAll ：所在區域內會第一個執行。
	- beforeEach ：每一次的測試前會先執行。
	- afterAll ：所在區域內最後一個執行。
	- afterEach ：每一次的測試後會馬上執行。
  
```javascript
describe('v4 PasswordVerifier', () => {
  let verifier;
  // 將共同的部分提出
  beforeEach(() => verifier = new PasswordVerifier1());
  describe('with a failing rule', () => {
    let fakeRule, errors;
    // 將共同的部分提出
    beforeEach(() => {
      fakeRule = input => ({ passed: false, reason: 'fake reason' });
      verifier.addRule(fakeRule);
    });
    it('has an error message based on the rule.reason', () => {
      errors = verifier.verify('any value');

      expect(errors[0]).toContain('fake reason');
    });
    it('has exactly one error', () => {
      const errors = verifier.verify('any value');

      expect(errors.length).toBe(1);
    });
  });
});
```

### 2.6.1

```javascript
describe('v5 PasswordVerifier', () => {
  let verifier;
  beforeEach(() => verifier = new PasswordVerifier1());
  describe('with a failing rule', () => {
    let fakeRule, errors;
    beforeEach(() => {
      fakeRule = input => ({ passed: false, reason: 'fake reason' });
      verifier.addRule(fakeRule);
      // 將共同的部分提出
      errors = verifier.verify('any value');
    });
    it('has an error message based on the rule.reason', () => {
      expect(errors[0]).toContain('fake reason');
    });
    it('has exactly one error', () => {
      expect(errors.length).toBe(1);
    });
  });
});
```
- 此時 beforeEach() 有重複的現象
```javascript
describe('v6 PasswordVerifier', () => {
  let verifier;
  beforeEach(() => verifier = new PasswordVerifier1());
  describe('with a failing rule', () => {
    let fakeRule, errors;
    beforeEach(() => {
      fakeRule = input => ({ passed: false, reason: 'fake reason' });
      verifier.addRule(fakeRule);
      errors = verifier.verify('any value');
    });
    it('has an error message based on the rule.reason', () => {
      expect(errors[0]).toContain('fake reason');
    });
    it('has exactly one error', () => {
      expect(errors.length).toBe(1);
    });
  });
  describe('with a passing rule', () => {
    let fakeRule, errors;
    beforeEach(() => {
      fakeRule = input => ({ passed: true, reason: '' });
      verifier.addRule(fakeRule);
      errors = verifier.verify('any value');
    });
    it('has no errors', () => {
      expect(errors.length).toBe(0);
    });
  });
  describe('with a failing and a passing rule', () => {
    let fakeRulePass, fakeRuleFail, errors;
    beforeEach(() => {
      fakeRulePass = input => ({ passed: true, reason: 'fake success' });
      fakeRuleFail = input => ({ passed: false, reason: 'fake reason' });
      verifier.addRule(fakeRulePass);
      verifier.addRule(fakeRuleFail);
      errors = verifier.verify('any value');
    });
    it('has one error', () => {
      expect(errors.length).toBe(1);
    });
    it('error text belongs to failed rule', () => {
      expect(errors[0]).toContain('fake reason');
    });
  });
});
```

## 2.7 嘗試使用工廠方法 
> Trying the factory method route

- 工廠方法：建立物件或特殊的狀態，在多處重複使用相同的邏輯

```javascript
describe('v7 PasswordVerifier', () => {
  let verifier;
  beforeEach(() => verifier = new PasswordVerifier1());
  describe('with a failing rule', () => {
    let errors;
    beforeEach(() => {
      verifier.addRule(makeFailingRule('fake reason'));
      errors = verifier.verify('any value');
    });
    it('has an error message based on the rule.reason', () => {
      expect(errors[0]).toContain('fake reason');
    });
    it('has exactly one error', () => {
      expect(errors.length).toBe(1);
    });
  });
  describe('with a passing rule', () => {
    let errors;
    beforeEach(() => {
      verifier.addRule(makePassingRule());
      errors = verifier.verify('any value');
    });
    it('has no errors', () => {
      expect(errors.length).toBe(0);
    });
  });
  describe('with a failing and a passing rule', () => {
    let errors;
    beforeEach(() => {
      verifier.addRule(makePassingRule());
      verifier.addRule(makeFailingRule('fake reason'));
      errors = verifier.verify('any value');
    });
    it('has one error', () => {
      expect(errors.length).toBe(1);
    });
    it('error text belongs to failed rule', () => {
      expect(errors[0]).toContain('fake reason');
    });
  });

  // These methods are inside the describe block
  // so that they can live alongside the helpers in the next example.
  const makeFailingRule = (reason) => {
    return (input) => {
      return { passed: false, reason: reason };
    };
  };
  const makePassingRule = () => (input) => {
    return { passed: true, reason: '' };
  };
});
```

### 2.7.1 使用工廠方法將 beforeEach() 完全取代
```javascript
const makeVerifier = () => new PasswordVerifier1();
const passingRule = (input) => ({ passed: true, reason: '' });

const makeVerifierWithPassingRule = () => {
  const verifier = makeVerifier();
  verifier.addRule(passingRule);
  return verifier;
};

const makeVerifierWithFailedRule = (reason) => {
  const verifier = makeVerifier();
  const fakeRule = input => ({ passed: false, reason: reason });
  verifier.addRule(fakeRule);
  return verifier;
};

describe('v8 PasswordVerifier', () => {
  describe('with a failing rule', () => {
    it('has an error message based on the rule.reason', () => {
      const verifier = makeVerifierWithFailedRule('fake reason');
      const errors = verifier.verify('any input');
      expect(errors[0]).toContain('fake reason');
    });
    it('has exactly one error', () => {
      const verifier = makeVerifierWithFailedRule('fake reason');
      const errors = verifier.verify('any input');
      expect(errors.length).toBe(1);
    });
  });
  describe('with a passing rule', () => {
    it('has no errors', () => {
      const verifier = makeVerifierWithPassingRule();
      const errors = verifier.verify('any input');
      expect(errors.length).toBe(0);
    });
  });
  describe('with a failing and a passing rule', () => {
    it('has one error', () => {
      const verifier = makeVerifierWithFailedRule('fake reason');
      verifier.addRule(passingRule);
      const errors = verifier.verify('any input');
      expect(errors.length).toBe(1);
    });
    it('error text belongs to failed rule', () => {
      const verifier = makeVerifierWithFailedRule('fake reason');
      verifier.addRule(passingRule);
      const errors = verifier.verify('any input');
      expect(errors[0]).toContain('fake reason');
    });
  });
});
```

## 2.8 回到僅使用 test() 
> Going Full Circle to test()
- 拿掉巢狀的 describe()
```javascript
// v9 tests
test('pass verifier, with failed rule, ' +
          'has an error message based on the rule.reason', () => {
  const verifier = makeVerifierWithFailedRule('fake reason');
  const errors = verifier.verify('any input');
  expect(errors[0]).toContain('fake reason');
});
test('pass verifier, with failed rule, has exactly one error', () => {
  const verifier = makeVerifierWithFailedRule('fake reason');
  const errors = verifier.verify('any input');
  expect(errors.length).toBe(1);
});
test('pass verifier, with passing rule, has no errors', () => {
  const verifier = makeVerifierWithPassingRule();
  const errors = verifier.verify('any input');
  expect(errors.length).toBe(0);
});
test('pass verifier, with passing  and failing rule,' +
          ' has one error', () => {
  const verifier = makeVerifierWithFailedRule('fake reason');
  verifier.addRule(passingRule);
  const errors = verifier.verify('any input');
  expect(errors.length).toBe(1);
});
test('pass verifier, with passing  and failing rule,' +
          ' error text belongs to failed rule', () => {
  const verifier = makeVerifierWithFailedRule('fake reason');
  verifier.addRule(passingRule);
  const errors = verifier.verify('any input');
  expect(errors[0]).toContain('fake reason');
});
```

## 2.9 重構為參數測試
> Refactoring to parameterized test

- 密碼規則
```javascript
// password-rules.js
const oneUpperCaseRule = (input) => {
  return {
    passed: (input.toLowerCase() !== input),
    reason: 'at least one upper case needed'
  };
};

module.exports = {
  oneUpperCaseRule
};
```

```javascript
const { oneUpperCaseRule } = require('../password-rules');
```
- 有重複的地方需要調整
```javascript
describe('one uppercase rule', () => {
  test('given no uppercase, it fails', () => {
    const result = oneUpperCaseRule('abc');
    expect(result.passed).toEqual(false);
  });
  test('given one uppercase, it passes', () => {
    const result = oneUpperCaseRule('Abc');
    expect(result.passed).toEqual(true);
  });
  test('given a different uppercase, it passes', () => {
    const result = oneUpperCaseRule('aBc');
    expect(result.passed).toEqual(true);
  });
});
```
- 使用 [test.each](https://jestjs.io/docs/api#testeachtablename-fn-timeout)：同一個測試使用不同參數時可以使用
```javascript
//parameterized
describe('v2 one uppercase rule', () => {
  test('given no uppercase, it fails', () => {
    const result = oneUpperCaseRule('abc');
    expect(result.passed).toEqual(false);
  });

  test.each([
    'Abc',
    'aBc'
  ])('given one uppercase, it passes', (input) => {
    const result = oneUpperCaseRule(input);
    expect(result.passed).toEqual(true);
  });
});
```
- 重構 test.each
```javascript
//parameterized
describe('v3 one uppercase rule', () => {
  test.each([
    ['Abc', true],
    ['aBc', true],
    ['abc', false],
  ])('given %s, %s ', (input, expected) => {
    const result = oneUpperCaseRule(input);
    expect(result.passed).toEqual(expected);
  });
});
```

```javascript
describe('v4 one uppercase rule, with the fancy jest table input', () => {
  test.each`
    input | expected
    ${'Abc'} | ${true}
    ${'aBc'} | ${true}
    ${'abc'} | ${false}
    `('given $input', ({ input, expected }) => {
    const result = oneUpperCaseRule(input);
    expect(result.passed).toEqual(expected);
  });
});
```
- 使用 test.each 
```javascript
describe('v5 one uppercase rule, with vanilla JS test.each', () => {
  const tests = {
    Abc: true,
    aBc: true,
    abc: false
  };

  for (const [input, expected] of Object.entries(tests)) {
    test(`given ${input}, ${expected}`, () => {
      const result = oneUpperCaseRule(input);
      expect(result.passed).toEqual(expected);
    });
  }
});
```

## 2.10 檢查預期會丟出的錯誤
> Checking for expected thrown errors

```javascript
// password-verifier1.js
class PasswordVerifier1 {
  constructor () {
    this.rules = [];
  }

  addRule (rule) {
    this.rules.push(rule);
  }

  verify (input) {
    if (this.rules.length === 0) {
      throw new Error('There are no rules configured');
    }
    const errors = [];
    this.rules.forEach(rule => {
      const result = rule(input);
      if (result.passed === false) {
        errors.push(result.reason);
      }
    });
    return errors;
  }
}

module.exports = { PasswordVerifier1 };
```

- 使用 fail()
```javascript
test('verify, with no rules, throws exception', () => {
  const verifier = makeVerifier();
  try {
    verifier.verify('any input');
    fail('error was expected but not thrown');
  } catch (e) {
    expect(e.message).toContain('no rules configured');
  }
});
```

- 使用 expect.toThrowError()
```javascript
test('verify, with no rules, throws exception', () => {
  const verifier = makeVerifier();
  expect(() => verifier.verify('any input'))
    .toThrowError(/no rules configured/);
});
```

## 2.11 設置測試類別
> Setting Test Categories

設置 jest.config.js 將 config 從 package.json 抽離出來

