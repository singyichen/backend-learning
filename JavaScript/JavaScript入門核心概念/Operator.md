---
title: 3. Operator 運算子
description: 
published: true
date: 2022-11-29T01:18:57.416Z
tags: javascript
editor: markdown
dateCreated: 2022-11-25T01:51:40.249Z
---

# JavaScript
- [ ] [Operator 運算子](https://javascript.alphacamp.co/operator.html)

# Operator 運算子
運算子 ( operator ) 可以對 value 做處理，並且回傳新的 value，我們將會介紹以下運算子：

- 算術運算子（ arithmetic operators ）
- 賦值運算子（ assignment operators ）
- 比較運算子（ comparison operators ）
- 邏輯運算子 （ logical operators ）

## 算術運算子 ( arithmetic operators )
最基本的是數學四則運算的符號，讓你可以對兩組 value 進行加減乘除：

| 運算子 | 名稱 | 範例 |
|:----:|:----:|:----:|
| `+` | 加 | 6 `+` 2 // 8 |
| `-` | 減 | 6 `-` 2 // 4 |
| `*` | 乘 | 6 `*` 2 // 12 |
| `/`	| 除	| 6 `/` 2 // 3 |

此外還有比較特別的：

| 運算子 | 名稱 | 功能 |	範例 | 結果說明 |
|:----:|:----:|----|----|----|
| `%`	| remainder	| 取餘數	| 6 `%` 2 7 `%` 2	`%` | 表達的是前後數字相除後的餘數，如範例：`6 % 2` 的印出結果為  `0`，因為整除後沒有餘數`7 % 2` 的印出結果為 `1`，因為不可整除，餘數為 1|
| `++` | increment | 將值增加 1	| x `=` 3 x`++`	| 4。變數 x 的值為 3，`++` 後，值增加 1，因此 x = 4 | 
| `--` | decrement | 將值減少 1 | x `=` 3 x`--` |	2。變數 x 的值為3 ， `--` 後，值減少 1，因此 x = 2 |
| `**` | 指數 | 計算 a 的 b 次方 | 2 `**` 3 `|` | 8。這段指令意為 2 的 3 次方 |


## 賦值運算子 ( assignment operators )
目前為止我們一直使用的 `=`，是一種賦值運算子，除此之外還有：

| 運算子	| 範例（x = 3, y = 2） | 概念說明 | 結果 |
|:----:|:----:|:----:|:----:|
| `+=` |	`x +=  y`	| 等同 `x = x + y` 的意思 | x = 5 |
| `-=` | `x -= y`	| 等同 `x = x - y` 的意思 | x = 1 |
| `*=` | `x *= y`	| 等同 `x = x * y` 的意思 | x = 6 |
| `/=`	| `x /= y` | 等同 `x = x / y` 的意思	| x = 1.5 |
| `%=`	| `x %= y` | 等同 `x = x % y` 的意思	| x = 1 |

## 比較運算子 ( comparison operators )
比較運算子陳述的是邏輯關係，他會對前後的 value 進行比較，然後回傳 boolean 值，也就是 true 或是 false。

| 運算子 | 意義 | 範例 | 結果 | 說明 |
|:----:|:----:|:----:|:----:|:----:|
| `==` | 寬鬆的等於 | 1 == '1' ， 0 == '' | true，true | 請小心 JavaScript 的 `==` 不會檢查資格型別，由於特別寬鬆，請不要使用 |
| `===` | 嚴格的等於 | 1 === '1' ， 0 === '' | false，false | 請一律使用這個符號 |
| `!=` | 寬鬆的不等於 | 1 != '1' ， 0 != '' | false，false | 不會檢查資格型別，請不要使用 |
| `!==` | 嚴格的不等於 | 1 !== '1' ， 0 !== '' | true，true | 請一律使用這個符號 |
| `>` | 大於 | 3 > 13 > 3	| true，false | |	
| `<` | 小於 | 3 < 13 < 31 < 3 | false，false，true | |	
| `>=` | 大於等於 | 3 >= 13 >= 31 >= 3 | true，true，false | |	
| `<=`| 小於等於	| 3 <= 13 <= 31 <= 3 | false，true，true | |	

## 邏輯運算子 ( logical operators ) 
| 運算子 | 意義 | Example | 回傳結果 |
|:----:|:----:|:----:|:----:|
| `&&` | 「而且」；如果前後的 value 都是 true，則回傳 true | true `&&` truetrue `&&` falsefalse `&&` false | true，false，false |
| `||` | 「或者」；只要有一邊為 true，則為 true | true `||` truetrue `||` falsefalse `||` false | true，true，false |
| `!` | 「非」，將後面接的 boolean 轉成相反的值 | `!`true `!`false | false，true |