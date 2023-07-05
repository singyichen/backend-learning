---
title: Array
description: 
published: true
date: 2023-05-17T02:02:07.724Z
tags: array
editor: markdown
dateCreated: 2022-05-24T00:16:47.845Z
---

# Array
- [ ] [陣列 Array](https://ithelp.ithome.com.tw/articles/10213787)
- [ ] [js-array-operations](https://github.com/tooto1985/js-array-operations)
- [ ] [用 JavaScript 學習資料結構和演算法：陣列（Array）篇](https://blog.techbridge.cc/2016/10/08/javascript-data-structure-algorithm-array/)
- [ ] [JavaScript reduce 在做什麼？](https://w3c.hexschool.com/blog/a2cb755f)
- [ ] [JavaScript之一定要瞭解的 Array 與方法](https://ithelp.ithome.com.tw/users/20104175/ironman/2584)

# 什麼是 Array？
簡單來說他就是儲存資料的箱子，而且是連續性的，所以當要拿特定位置的值非常快 `array[index]`。時間複雜度是 `Big O(1)`。但如果你忘了是哪個位置，時間複雜度就會變 `Big O(n)`。

- 遇到需要順序的問題，通常 Array 就是最佳解 (例如 Binary Search )。
- Array 壞處就是當你想要加入某一個值(或移除掉)，整個 Array 都會動到 (之後篇章會探討 Array method 的時間複雜度)

特別要注意的是

- 經過 Array **方法**後到底**回傳**了什麼
- 會不會**改變**原本的 Array

![什麼是 Array.png](http://192.168.25.60:8000/files/file_storage/e0176b04.jpg)

# 如何建立 Array
```js
// 1.literal
// 最常見到也是最推薦的
let arrLiteral = []; // Empty Array
let arrLiteral = [1, 2, 3];
    
// 2.by constructor
// 比較不建議因為效能較差而且會有一些無法預期的結果
// 例如 new Array(3) 會建立 3 個 empty Array
let arrConstructor = new Array() // Empty Array
let arrConstructor = new Array(1, 2, 3)
```
# 常見 Array 方法
陣列相當彈性也功能強大的一種資料結構，除了上面介紹的方法外還擁有許多常用方法。我們這裡也借用 [@tooto1985](https://github.com/tooto1985) 所整理的圖解 JavaScript 陣列操作方法進行講解：

![常見 Array 方法.png](http://192.168.25.60:8000/files/file_storage/4bc47c6f.png)

## Example
```js
//Example
var ary = ["A", "B", "C", "D", "E"];

//length
console.log(ary.length); //5

//push
console.log(ary.push("F")); //6

//pop
console.log(ary.pop()); //"F"

//unshift
console.log(ary.unshift("F")); //6

//shift
console.log(ary.shift()); //"F"

//splice(insert)
console.log(ary.splice(3, 0, "F")); //[]

//splice(remove)
console.log(ary.splice(3, 1)); //["F"]

//indexOf
console.log(ary.indexOf("D")); //3

//lastIndexOf
console.log(ary.lastIndexOf("D")); //3

//join
console.log(ary.join(":")); //"A:B:C:D:E"

//reverse
console.log(ary.reverse()); //["E", "D", "C", "B", "A"]

//sort
console.log(ary.sort()); //["A", "B", "C", "D", "E"]

//some
console.log(ary.some(function(item) {
	return item === "C";
})); //true

//every
console.log(ary.every(function(item) {
	return item.length === 1;
})); //true

//forEach
console.log(ary.forEach(function(item) {
	return item;
})); //undefined

//reduce
console.log(ary.reduce(function(prevItem, item) {
	return prevItem + item;
})); //"ABCDE"

//reduceRight
console.log(ary.reduceRight(function(prevItem, item) {
	return prevItem + item;
})); //"EDCBA"

//map
console.log(ary.map(function(item) {
	return item + item;
})); //["AA", "BB", "CC", "DD", "EE"]

//filter
console.log(ary.filter(function(item) {
	return item > "C";
})); //["D", "E"]

//slice
console.log(ary.slice(2, 4)); //["C", "D"]

//concat
console.log(ary.concat(["G", "H"])); //["A", "B", "C", "D", "E", "G", "H"]
```

## Array Method 整理

![Array Method 整理.png](http://192.168.25.60:8000/files/file_storage/def025ee.png)

# Basic Array Method
```js
// 初始值，以下範例都會共用這個 -----
const colors = ['red', 'yellow', 'blue'];
```
## push
> - [`Array.prototype.push(elementN)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/push)
> - method：在原本的 Array **後面** 加上 **新值**
> - return：length
> - array：改變

所以把 `push` 存在變數裡 (`newColor`) 他竟然會回傳 Array 的長度呢!
另外 `const` 不能被重新指定值但 `colors.push` 在這裡卻不會有錯誤，為什麼呢？因為 Array 跟 Object 都是 `by reference`，只要不是重新指定 ( = ) 就會在同一個 `reference` 裡。

```js
const newColor = colors.push('purple', 'green'); 

console.log(colors) // ['red', 'yellow', 'blue', 'purple', 'green']
console.log(newColor) // 5
```

## unshift
> - [`Array.prototype.unshift(elementN)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/unshift)
> - method：在原本的 Array **前面** 加上 **新值**
> - return：length
> - array：改變

```js
const newColor = colors.unshift('purple', 'green');
 
console.log(colors) // ['purple', 'green', 'red', 'yellow', 'blue' ]
console.log(newColor) // 5
```

## pop
> - [`Array.prototype.pop()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/pop)
> - method：移除原本陣列 **最後面** 的 **第一個值**
> - return：被移除掉的那個值
> - array：改變

```js
const newColor = colors.pop();
 
console.log(colors) // ['red', 'yellow']
console.log(newColor) // 'blue'
```

## shift
> - [`Array.prototype.shift()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/shift)
> - method：移除原本陣列 **最前面** 的 **第一個值**
> - return：被移除掉的那個值
> - array：改變

```js
const newColor = colors.shift();
 
console.log(colors) // ['yellow', 'blue' ]
console.log(newColor) // 'red'
```

## concat
> - [`Array.prototype.concat()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/concat)
> - method：把兩個陣列**合併**在一起
> - return：新陣列
> - array：不改變

```js
const colors2 = ['grey', 'black', 'white'];
const newColor = colors.concat(colors2);
 
console.log(colors) // ['red', 'yellow', 'blue'] 原本的
console.log(newColor) // ['red', 'yellow', 'blue', 'grey', 'black', 'white' ]
```

# Filter/Find something
```js
// 初始值 -----
const inventors = [
    { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 },
    { first: 'Isaac', last: 'Newton', year: 1643, passed: 1727 },
    { first: 'Galileo', last: 'Galilei', year: 1564, passed: 1642 },
    { first: 'Marie', last: 'Curie', year: 1867, passed: 1934 },
    { first: 'Johannes', last: 'Kepler', year: 1571, passed: 1630 },
    { first: 'Nicolaus', last: 'Copernicus', year: 1473, passed: 1543 },
    { first: 'Max', last: 'Planck', year: 1858, passed: 1947 },
    { first: 'Katherine', last: 'Blodgett', year: 1898, passed: 1979 },
    { first: 'Ada', last: 'Lovelace', year: 1815, passed: 1852 },
    { first: 'Sarah E.', last: 'Goode', year: 1855, passed: 1905 },
    { first: 'Lise', last: 'Meitner', year: 1878, passed: 1968 },
    { first: 'Hanna', last: 'Hammarström', year: 1829, passed: 1909 }
];
```

## findIndex
> - [`Array.prototype.findIndex(item, index, array)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/findIndex)
> - return：**第一個** 符合條件的位置 **`index`**，若都找不到則回傳 `-1`
> - array：不改變

```js
const findIndexEmpty = inventors.findIndex(function(item){
 
})
console.log(findIndexEmpty) // 沒有找到會回傳 -1
console.log(inventors) // 原本的 array
 
const findIndexInventors = inventors.findIndex(function(item){
    return item.year > 1870;
})
console.log(findIndexInventors) // 0
// 有好幾個是 > 1800 的，但他只會回傳第一個抓到的 index
``` 

## find
> - [`Array.prototype.find(item, index, array)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/find)
> - return：**第一個** 抓到條件為 `true` 的值
> - array：不改變

```js
const findEmpty = inventors.find(function(item){
 
})
console.log(findEmpty) // 沒有條件會 undefined
console.log(inventors) // 原本的 array
 
const findInventors = inventors.find(function(item){
    return item.year > 1870;
})
console.log(findInventors) // 有好幾個是 > 1800 的，但他只會回傳第一個抓到的
// { first: 'Albert', last: 'Einstein', year: 1879, passed: 1955 }
``` 

## filter
> - [`Array.prototype.filter(item, index, array)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/filter)
> - method：只要條件為 `true` 的就會包含在此陣列，適合拿來搜尋
> - return：一個陣列
> - array：不改變

```js
const filterEmpty = inventors.filter(function(item){
 
})
console.log(filterEmpty) // 沒有條件會 undefined
console.log(inventors) // 原本的 array
 
const filterInventors = inventors.filter(function(item){
    return item.year > 1870;
})
console.log(findInventors)
// [{first: "Albert", last: "Einstein", year: 1879, passed: 1955}
// {first: "Katherine", last: "Blodgett", year: 1898, passed: 1979},
// {first: "Lise", last: "Meitner", year: 1878, passed: 1968}]
``` 

## forEach
> - [`Array.prototype.forEach(item, index, array)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/forEach)
> - method：`forEach` 單純只執行原本陣列裡的事
> - return：不會回傳任何東西
> - array：改變

```js
const forEachInventors = inventors.forEach(function(item){
    return item.year > 1870;
})
console.log(forEachInventors) // undefined, 不會 return 東西
 
inventors.forEach(function(item){
    item.year =  1870;
})
console.log(inventors) // 全部 12 個的 year = 1870
``` 

## map
> - [`Array.prototype.map(item, index, array)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
> - method：將條件運算後重新組合
> - return：一個數量等於 `array.length` 的陣列
> - array：不改變

```js
const mapInventors = inventors.map(function(item){
    return item.year > 1870;
})
console.log(mapInventors) // [false, false, true ... 12 個]
console.log(inventors) // 原本的 array
 
const mapInventors2 = inventors.map(function(item){
    if(item.year > 1870){
        return `${item.year}歲`;
    }
    return false;
})
console.log(mapInventors2) // ["1879歲", false, false....]
// 不論是空的或是 false，都會回傳
``` 

## some
> - [`Array.prototype.some(item, index, array)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/some)
> - method：它不會 `Array` 全部找完，只要一找到相符的直就跳出
> - return：一個 `Boolean`，只要部分符合就回傳 `true`
> - array：不改變

```js
const someInventors = inventors.some(function(item){
    return item.year > 1870;
})
console.log(someInventors) // true
console.log(inventors) // 原本的 array
``` 

## every
> - [`Array.prototype.every(item, index, array)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/every)
> - return：一個 `Boolean`，需要全部符合才回傳 `true`，部分符合會回傳 `false`
> - array：不改變

```js
const everyInventors = inventors.every(function(item){
    return item.year > 1870;
})
console.log(everyInventors) // false
console.log(inventors) // 原本的 array
``` 
# Special Method
```js
// 初始值 -----
var special = [8, 0, 14];
```

## reduce
> - [`Array.prototype.reduce(accumulator, currentValue, currentIndex, array [, initialValue])`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reduce)
> - method：可以與前一個回傳值做運算
> - return：運算結果的值
> - array：不改變

- `accumulator`：前一個回傳的值，從 `initialValue` 開始．若沒有 `initialValue` 就是從陣列的 index 0 開始
- `currentValue`：現在的值
- `currentIndex`：現在的參數位置
- `array`：全部陣列
- `initialValue`：初始值

```js
// 未初始值的狀況
const reduceArray = special.reduce(
function(accumulator, currentValue, currentIndex){
    return Math.max(accumulator, currentValue);
})
console.log(reduceArray) // 14
console.log(special) // 原本的 array
``` 

![reduce 未給初始值結果.png](http://192.168.25.60:8000/files/file_storage/f2192f8a.png)

```js
// 給初始值的狀況
const reduceArray2 = special.reduce(
function(accumulator, currentValue, currentIndex){
    return Math.max(accumulator, currentValue);
},30)
console.log(reduceArray2) // 30
```

![reduce 有給初始值結果.png](http://192.168.25.60:8000/files/file_storage/60265cd2.png)

# Order 陣列排序
## sort
> - [`Array.prototype.sort(compareFunction)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/sort)
> - method：若沒有 `compareFunction`，會先自動 `.toString()` 轉成字串
> - return：一個根據 Unicode 排序 `array.length` 長度的 `array` 
> - array：改變
> - 特別要提得是，`< 10` 的 `array` 會是 stable 排序法， `> 10` 會是不穩定的排序法

```js
var fruit = ['cherries', 'apples', 'bananas'];
fruit.sort(); // ['apples', 'bananas', 'cherries']
 
var scores = [1, 10, 21, 2];
scores.sort(); // [1, 10, 2, 21]
// 跟你想的不一樣吧，因為 10 是兩個數字組成，
// 在 unicode 裡 32(數字2） > 31(1 開頭的任何數字)
 
var things = ['word', 'Word', '1 Word', '2 Words'];
things.sort(); // ['1 Word', '2 Words', 'Word', 'word']
``` 

若有 `compareFunction` 會有兩個參數 `a / b` , `Array.prototype.sort(a, b)`

```js
function compare(a, b) {
  // 若 a 比較小 排序就是 a, b
  if (a is less than b by some ordering criterion) {
    return -1;
  }
  // 若 a 比較大 排序就是 b, a  就是位置會對調
  if (a is greater than b by the ordering criterion) {
    return 1;
  }
  // 若一樣
  return 0;
}
 
var items = [
  { name: 'Edward', value: 21 },
  { name: 'Sharpe', value: 37 },
  { name: 'And', value: 45 },
  { name: 'The', value: -12 },
  { name: 'Magnetic', value: 13 },
  { name: 'Zeros', value: 37 }
];
 
// sort by value 快速寫法
items.sort(function (a, b) {
  return a.value - b.value;
});
 
// sort by name
items.sort(function(a, b) {
  var nameA = a.name.toUpperCase(); // ignore upper and lowercase
  var nameB = b.name.toUpperCase(); // ignore upper and lowercase
  if (nameA < nameB) {
    return -1;
  }
  if (nameA > nameB) {
    return 1;
  }
 
  // names must be equal
  return 0;
});
``` 

## reverse
> - [`Array.prototype.reverse()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)
> - method：相對單純的一個方法
> - return：反過來長度為 `array.length` 的陣列
> - array：改變

```js
var a = ['one', 'two', 'three'];
var reversed = a.reverse();
 
console.log(a);        // ['three', 'two', 'one']
console.log(reversed); // ['three', 'two', 'one']
``` 
# Operation
```js
// 初始值 -----
const my_array = [5, 1, 3, 8, 6, 0]
```

## slice
> - [`Array.prototype.slice(start, end)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/slice)
> - method：`slice` 就是切，把 array 頭尾切兩刀
> - return：在 `start` (包含) 跟 `end` 之間的陣列
> - array：不改變

```js
const sliceArray = my_array.slice(2,5);
console.log(sliceArray); // [3, 8, 6]
console.log(my_array); // [5, 1, 3, 8, 6, 0] 不會變
``` 

![array slice example 1.png](http://192.168.25.60:8000/files/file_storage/96081b33.png)


```js
const sliceArray = my_array.slice(2);
console.log(sliceArray); // [3, 8, 6, 0]
``` 

![array slice example 2.png](http://192.168.25.60:8000/files/file_storage/64ed6368.png)

```js
const sliceNegativeArray = my_array.slice(-1);
console.log(sliceNegativeArray); // [0]
``` 

![array slice example 3.png](http://192.168.25.60:8000/files/file_storage/b1e6d6b3.png)

## splice
> - [`Array.prototype.splice(start, deleteCount, item1, item2, …)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/splice)
> - method：`splice` 是拼接的意思你不但可以切掉某塊還可以塞東西進去
> - return：被移掉的值放在陣列裡
> - array：改變
> - `splice` 乍看下跟 `slice` 有點像，但其實幾乎完全相反(驚)。`slice` 是取 `start` `end` 裡的東西，而 `splice` 是 return 中間被移掉的東西。`slice` 不會影響本來陣列而 `splice` 會影響原本陣列。新增的 `item1`、`item2` 隻會影響本來陣列，並不會回傳到新的陣列裡
 

```js
// 假如只有 splice(start)，這時會跟 slice 行為一樣
const spliceArray1 = my_array.splice(3)
console.log(spliceArray1); // [8, 6, 0] 被移掉的值
console.log(my_array); // [5, 1, 3]，index 3 後面都移掉
 
//一樣可以接受負數
const spliceArray2 = my_array.splice(-1)
console.log(spliceArray2); // [0]
console.log(my_array); // [5, 1, 3, 8, 6] 倒數 1 個後面都移掉
 
// splice(start, deleteCount 抓幾個)
const spliceArray3 = my_array.splice(1, 3)
console.log(spliceArray3); // [1, 3, 8]
console.log(my_array); // [5, 6, 0]
// 從 index 1 開始抓 3 個 把中間值移掉
 
// splice(start, deleteCount 抓幾個, 插入 item)
const spliceArray4 = my_array.splice(1, 3, 'Hana', 'Mike')
console.log(spliceArray4); // [1, 3, 8]
// 不會有 'Hana', 'Mike' 字眼出現
 
console.log(my_array); // [5, "Hana", "Mike", 6, 0]
// 從 index 1 開始抓 3 個 把中間值移掉 並塞進 "Hana", "Mike"
``` 

## indexOf
> - [`Array.prototype.indexOf(searchElement)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/indexOf)
> - return：所在位置的 `index`，如果找不到會回傳 `-1`
> - array：不改變

```js
const indexArray = my_array.indexOf(3) // 2
console.log(indexArray) // 2
console.log(my_array) // [5, 1, 3, 8, 6, 0]
 
const indexArray2 = my_array.indexOf(100)
console.log(indexArray) // -1
``` 

## join
> - [`Array.prototype.join(separator)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/join)
> - method：把所有陣列裡的值加上 `separator`
> - return：一個字串
> - array：不改變

```js
const joinArray = my_array.join('-') // 2
console.log(joinArray) // '5-1-3-8-6-0'
console.log(my_array) // [5, 1, 3, 8, 6, 0]
 
const joinArray2 = my_array.join('') // 2
console.log(joinArray2) // '513860'
``` 

## includes
> - [`Array.prototype.includes(searchElement, fromIndex)`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/includes)
> - method：看 `searchElement` 有沒有在 `array` 裡
> - return：Boolean
> - array：不改變

```js
const includesArray = my_array.includes(3)
console.log(includesArray) // true
console.log(my_array) // [5, 1, 3, 8, 6, 0]
 
my_array.includes(5,3) // false 從 index 第三個之後找 5
``` 

