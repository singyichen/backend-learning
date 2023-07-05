---
title: Unit Test CH9
description: The Art of Unit Testing Third Edition v7
published: true
date: 2022-07-28T03:49:53.029Z
tags: unit test
editor: markdown
dateCreated: 2022-07-28T01:01:50.971Z
---

# CH9 Readability
> - 寫可閱讀的測試
> - 發掘單元測試命名的方便性

- 單元測試的命名
- 變數的命名
- 建立好的斷言訊息
- 從動作中分離出斷言

## 9.1 命名單元測試
> Naming unit tests

### 9.1.1 命名的三個原則
- 工作單元的進入點或測式的功能
- 測試的劇本
- 預期的出口點

```javascript
// Same information, different variations
test('verifyPassword, with a failing rule, returns error based on rule.reason', () => {…}
describe(verifyPassword, () => {
  describe('with a failing rule', () => {
    it(returns error based on the rule.reason', () => {…}
       
   verifyPassword_withFailingRule_returnsErrorBasedonRuleReason()
```

```javascript
// Names with missing infomation
Examples:
test(failing rule, returns error based on rule.reason', () => {…}
“Excuse me, what is the thing under test?”
 
test('verifyPassword, returns error based on rule.reason', () => {…}
“Sorry, when is this supposed to happen?”
 
test('verifyPassword, with a failing rule', () => {…}
“Um, what’s supposed to happen then?”
```

## 9.1 神奇的值與變數命名
> Magic Values and Naming variables

- 沒有明確命名會造成閱讀困難

```javascript
// A test with magic values
describe('password verifier', () => {
  test('on weekends, throws exceptions', () => {
    // contains three magic values
    expect(() => verifyPassword(!', [], 0))
      .toThrowError("It's the weekend!");
  });
});
```
- 將神奇的值用有意義的變數命名表示

```javascript
// Fixing magic values
describe("verifier2 - dummy object", () => {
  test("on weekends, throws exceptions", () => {
    const SUNDAY = 0, NO_RULES = [];
    expect(() => verifyPassword2("anything",
                                 NO_RULES,
                                 SUNDAY))
                        .toThrowError("It's the weekend!");
  });
});
```

## 9.3 從動作中分離出斷言
> Separating asserts from actions

- 將斷言從動作中分離出來，可以增加閱讀性

```javascript
// separating assert from action
// #Example 1
expect(verifier.verify("any value")[0]).toContain("fake reason");
 
// #Example 2
const result = verifier.verify("any value");
expect(result[0]).toContain("fake reason");
```

## 9.4 配置與拆毀
> Setting up and tearing down

- 使用 beforeEach 來配置模擬物件或虛設常式，會造成模擬物件會在哪裡使用，或測試當中他們會有怎樣的預期出現位置

```javascript
// Using a setup/beforeEach function for mock setup
describe("password verifier", () => {
  let mockLog;
  beforeEach(() => {
    mockLog = Substitute.for<IComplicatedLogger>();
  });

  test("verify, with logger & passing,calls logger with PASS",() => {
   const verifier = new PasswordVerifier2([], mockLog);
   verifier.verify("anything");

   mockLog.received().info(
     Arg.is((x) => x.includes("PASSED")),
     "verify"
   );
  });
});
```

- 直接在測試中初始化模擬物件，會比較有可讀性

```javascript
// avoiding a setup function
describe("password verifier", () => {
  test("verify, with logger & passing,calls logger with PASS",() => {
    const mockLog = Substitute.for<IComplicatedLogger>();

    const verifier = new PasswordVerifier2([], mockLog);
    verifier.verify("anything");

    mockLog.received().info(
      Arg.is((x) => x.includes("PASSED")),
      "verify"
    );
  });
```

- 將建立模擬物件重構在一個輔助函式，讓多個測試可以重複使用

```javascript
// using a helper function
describe("password verifier", () => {
  test("verify, with logger & passing,calls logger with PASS",() => {
    // helper function
    const mockLog = makeMockLogger(); //defined at top of file

    const verifier = new PasswordVerifier2([], mockLog);
    verifier.verify("anything");

    mockLog.received().info(
      Arg..((x) => x.includes("PASSED")),
      "verify"
    );
  });
```














