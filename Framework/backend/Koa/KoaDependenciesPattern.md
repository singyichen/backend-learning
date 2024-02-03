---
title: Koa Dependencies ( Pattern )
description: 排版套件
published: true
date: 2023-05-17T06:24:20.930Z
tags: koa, framework
editor: markdown
dateCreated: 2022-07-26T03:24:49.971Z
---

# 排版套件介紹 ( Pattern Dependencies )
## prettier
> - Prettier 是一個 Code formatter，能夠將 JavaScript, TypeScript, CSS 程式碼格式化，進而統一程式碼風格（Coding Style）
> - 自動整理程式碼格式
> - 負責修程式碼格式

### Install
> [reference](https://prettier.io/docs/en/install.html) 
> [reference](https://www.npmjs.com/package/prettier)
{.is-info}

```shell
npm install --save-dev --save-exact prettier
```

### Usage
> [reference](https://www.robinwieruch.de/how-to-use-prettier-vscode/)
{.is-info}

- 新增一個設定檔讓其他工具知道正在使用 prettier ➔ 新增以下 json
```shell
echo {}> .prettierrc.json
```
```json
// .prettierrc
{
   "singleQuote": true, 
   "trailingComma": "all", 
   "printWidth": 80, 
   "tabWidth": 2, 
   "useTabs": false, 
   "quoteProps": "as-needed", 
   "bracketSpacing": true, 
   "jsxSingleQuote": true, 
   "arrowParens": "always"
}
```
```json
// .prettierrc
{
  "singleQuote": true, // 使用單引號
  "trailingComma": "all", // 尾逗號
  "printWidth": 80, // 單行程式碼字元數限制
  "tabWidth": 2, // tab鍵縮排相當於2個空格
  "useTabs": false, // 行縮排使用空格
  "quoteProps": "as-needed", // 僅僅當必須的時候才會加上雙引號
  "bracketSpacing": true, // 在括號和物件的文字之間加上一個空格
  "jsxSingleQuote": true, // 在 JSX 中使用單引號
  "arrowParens": "always" // 箭頭函式參數總是有括弧
}
```
- 手動新增一個檔案 .prettierignore
```json
// .prettierignore
# Ignore artifacts:
build
coverage
node_modules

# Ignore all HTML files:
*.html
```
- 使用 ctrl + p 搜尋檔案 setting.json ➔ 修改及新增以下 script ➔ 儲存會自動排版
```json
// setting.json
// enable globally (here: format on save)
"editor.formatOnSave": true,
// enable per-language (here: Prettier as formatter)
"[javascript]": {
   "editor.defaultFormatter": "esbenp.prettier-vscode"
}
```
- 完整版 setting.json
```json
// setting.json
{
    "workbench.colorTheme": "Default Dark+",
    "workbench.iconTheme": "vscode-icons",
    "javascript.updateImportsOnFileMove.enabled": "always",
    "git.autofetch": true,
    "bracket-pair-colorizer-2.depreciation-notice": false,
    "prettier.singleQuote": true,
    "window.zoomLevel": 1,
    "editor.codeActionsOnSave": {
        "source.fixAll.eslint": true
      }, //把設定存在專案(工作區)可切開每個專案不同的設定，才不會每換專案就一直改設定，設定如下可在存檔時自動檢查並修正
    "eslint.alwaysShowStatus": true,
    "eslint.options": {
        "extensions": [".html", ".js",]
    },
    "eslint.validate": [
        "html",
        "javascript"
    ],
    "editor.formatOnSave": true, // enable globally (here: format on save)
    "[javascript]": {
        "editor.defaultFormatter": "esbenp.prettier-vscode"
    }, // enable per-language (here: Prettier as formatter)
    "prettier.printWidth": 70,
    "files.autoSave": "onWindowChange", // 切換視窗自動存檔
}
```

## eslint
> - ESLint（ECMAScript + lint）是用來檢查 JavaScript 程式碼的工具。可在 commit 前檢查語法錯誤、提示潛在的 bug，藉此有效提高程式碼質量，和統一基本的 coding style
> - 用來檢查語法的工具
> - 負責程式碼潛在質量問題

> [reference](https://eslint.org/docs/user-guide/getting-started) 
> [reference](https://pjchender.dev/webdev/note-eslint/) 
{.is-info}

### Install
> [reference](https://www.npmjs.com/package/eslint) 
{.is-info}
```shell
npm install eslint --save-dev
```
### Usage
> [reference](https://hackmd.io/@Heidi-Liu/note-eslint) 
{.is-info}
```shell
npm init @eslint/config
```
- options
```shell
√ How would you like to use ESLint? · problems    
√ What type of modules does your project use? · commonjs
√ Which framework does your project use? · none
√ Does your project use TypeScript? · No / Yes
√ Where does your code run? · browser
√ What format do you want your config file to be in? · JSON
```
- 自動產生一個 .eslintrc.json
```json
// .eslintrc.json
{
    "env": {
        "browser": true,
        "commonjs": true,
        "es2021": true
    },
    "extends": "eslint:recommended",
    "parserOptions": {
        "ecmaVersion": "latest"
    },
    "rules": {
    }
}
```
- 配合使用 eslint-config-prettier 修改 .eslintrc.json 如下
> [reference for rules](https://eslint.org/docs/rules/)
{.is-info}
```json
// .eslintrc.json
{
    "env": {
        "browser": true,
        "commonjs": true,
        "es2021": true,
        "node": true,
        "jest": true
    },
    "extends": ["eslint:recommended", "eslint-config-prettier"],
    "parserOptions": {
        "ecmaVersion": "latest"
    },
    "rules": {
        "semi": ["error", "always"],
        "no-extra-semi": "off",
        "no-underscore-dangle": "off"
    },
    "ignorePatterns": [".eslintrc.json"],
    "root": true
}
```

## eslint-config-prettier
> 可以自動判斷並關閉 ESLint 沒有必要的規則

> [reference](https://www.npmjs.com/package/eslint-config-prettier)
{.is-info}
### Install
```shell
npm install eslint-config-prettier --save-dev 
```

## eslint-plugin-prettier
> 將 prettier 整合進 eslint，作為 eslint 一條規則來執行 prettier

> [reference](https://www.npmjs.com/package/eslint-plugin-prettier)
{.is-info}
### Install
```shell
npm install eslint-plugin-prettier --save-dev
```

## prettier-eslint-cli
> eslint 會調整 code 為我們希望的風格，而 semi column 和 spance 等 Prettier 的工作則透過 prettier-eslint-cli 去完成，prettier-eslint 每次只能 prettier 一行或一個檔案，要使用在整個專案，便需要 prettier-eslint-cli

> [reference](https://github.com/prettier/prettier-eslint-cli)
{.is-info}

### Install
> [reference](https://www.npmjs.com/package/prettier-eslint-cli)
{.is-info}
```shell
npm install prettier-eslint-cli --save-dev
```
### Usage
scripts params meaning
- write：讓 prettier-eslint-cli 可以直接修改你的檔案，沒有加的話，只會將修改的結果 log 在 terminal 上，不管跑幾次你的 code 實際上都不會改變
```javascript
// package.json
"scripts": {
    "format": "prettier-eslint --write \"src/**/*.js\""
},
```





