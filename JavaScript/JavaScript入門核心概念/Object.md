---
title: 5. Object 物件
description: 
published: true
date: 2022-11-29T01:21:28.190Z
tags: javascript
editor: markdown
dateCreated: 2022-11-29T00:21:21.080Z
---

# JavaScript
- [ ] [Object 物件](https://javascript.alphacamp.co/object.html)

# Object 物件
不同於陣列是一個有序列的清單，物件是 key-value pairs 的集合。操作物件時，你需要知道 key。

![物件與陣列.png](http://192.168.25.60:8000/files/file_storage/0afc5bb2.png)

JavaScript 的物件是一個博大精深的學習主題，在這個單元裡，我們會先把學習範圍縮限在物件的基礎操作上，也就是存取資料。

## 宣告物件
你可以使用大括號 `{}` 來宣告物件，基礎的語法會是 `{key: value}`，這種寫法叫物件實字 ( object literal )。

在以下範例中，有一個外國人正在學習中文，他宣告了一個 `flashcardEntry` 物件來儲存中文單字：

```javascript
let flashcardEntry = {
  word: '薄',
  pronunciation: ['bo2','bao2'],
  meaning: 'thin',
  attempts: 2
}
```

當我們想用人類語言描述這個物件時，我們會說 `flashcardEntry` 有 `word`、`pronunciation`、`meaning`、`attempts` 等屬性 ( property )；而 `flashcardEntry` 的 `word` 屬性的值是 `'薄'`。

## 讀取物件內容
你需要透過 key 來存取物件的屬性，在寫法上有兩種方式：

```javascript
// dot notation
flashcardEntry.word
// bracket notation
flashcardEntry['word']
```

如果使用 dot notation，表示你在呼叫物件的屬性，或者可以說，你在呼叫 `flashcardEntry` 物件的 `word` 方法 ；如果使用 bracket notation，意義比較像你使用 `'word'` 這個 key 想取出 `flashcardEntry` 的內容。

關於屬性和方法這些稱呼，我們學到物件導向時，會深入釐清這個主題，目前你只需要依個人偏好，選擇自己喜歡的寫法。

但請你特別注意，使用 bracket notation 時，需要用字串的形式來表達 key，否則會視為變數，就如下圖的示範，當我在 console 裡執行 `lashcardEntry[word]` 時，程式回傳了錯誤訊息「word is note defined」，表示他以為 `word` 是一個變數，但我們並沒有使用 `let` 之類的關鍵字定義這個變數，因此有了錯誤訊息：

![使用 bracket notation 時，需要用字串的形式來表達 key，否則會視為變數.png](http://192.168.25.60:8000/files/file_storage/f145396b.png)

但請你注意，這就是 bracket notation 和 dot notation 最大的差別，dot notation 寫起來比較簡潔，受到大部分人的偏愛，但如果你想要使用變數來存取物件，你只能使用 bracket notation。

例如以下的例子：

![如果你想要使用變數來存取物件，你只能使用 bracket notation.png](http://192.168.25.60:8000/files/file_storage/c716859a.png)

我們透過變數 `lookFor` 來指定想查找的 key 值，再使用 bracket notation 來傳入變數，因此成功讀取了 `flashcardEntry` 的 `attempts` 屬性。

```javascript
let flashcardEntry = {
  word: '薄',
  pronunciation: ['bo2','bao2'],
  meaning: 'thin',
  attempts: 2
}
```

## 更新物件內容
回到我們的情境，這個正在學習中文的老外忽然發現「薄」這個字也有 flimsy (易壞) 的意思，他想要把這個資訊新增到字卡的 meaning 屬性裡。所以他重新使用 = 指派了 flashcardEntry['meaning'] 的值：

```javascript
flashcardEntry['meaning'] = 'thin, flimsy'
```

如果是要操作數字，例如每次查看字卡之後，`attempts` 屬性就要加 1，下列的寫法都能達成目的：

```javascript
// 方法 1
flashcardEntry['attempts'] = 3
// 方法 2
flashcardEntry['attempts'] = flashcardEntry['attempts'] +1
// 方法 3
flashcardEntry['attempts']++
```

但 `++` 運算子是最優雅的方法。

## 追加物件屬性
現在，我需要更新這個 APP，讓他能紀錄 `owner`，這很簡單，你只要用同樣的方式寫入內容就好了：

```javascript
let flashcardEntry = {
  word: '薄',
  pronunciation: ['bo2','bao2'],
  meaning: 'thin, flimsy',
  attempts:2
}
flashcardEntry['owner'] = 'Hawk'
```

在 console 裡查看新的物件內容，你會發現內容已經更新了

![追加物件屬性.png](http://192.168.25.60:8000/files/file_storage/587ea06b.png)

