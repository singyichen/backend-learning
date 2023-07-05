---
title: Function Programming CH4
description: Grokking Simplicity
published: true
date: 2022-07-01T03:51:01.669Z
tags: fp
editor: markdown
dateCreated: 2022-06-30T00:24:55.590Z
---

# CH4 Extracting calculations from actions
**大綱：學習如何重構程式碼，以增加程式碼的再利用性與可測試性**
> - 觀察資訊如何進入與離開一個函式
> - 發掘函式技術讓程式碼變得更有可測試性與再利用性
> - 學習如何將計算從動作分離出來

## 1. 歡迎來到 MegaMart.com!
> Welcome to MegaMart.com!

- MegaMart.com 是一個線上購物商店
- 他有一個主要具競爭力的功能，能在購物的同時呈顯目前總共消費金額
- 不需要保密協議 `DNA ( Non-disclosure agreement )`

![FP MegaMart.png](http://192.168.25.60:8000/files/file_storage/fd2741c5.png)

```javascript
// global variables for the cart and total
var shopping_cart = [];
var shopping_cart_total = 0;

function calc_cart_total() {
  shopping_cart_total = 0;
  for(var i = 0; i < shopping_cart.length; i++) {
    // sum all the item prices
    var item = shopping_cart[i];
    shopping_cart_total += item.price;
  }
  // update DOM to reflect new total
  set_cart_total_dom();
}

function add_item_to_cart(name, price) {
  // add a record to cart array to add items to cart
  shopping_cart.push({
    name: name,
    price: price
  });
	// update total because cart just changed
  calc_cart_total();
}
```
- 文件物件模型 DOM ( document object model )：把一份 HTML 文件內的各個標籤，包括文字、圖片等等都定義成物件，而這些物件最終會形成一個樹狀結構

## 2. 計算免運
> Calculating free shipping

![FP MegaMart calculating free shipping.png](http://192.168.25.60:8000/files/file_storage/216ffcb5.png)

- MegaMart 預計訂單金額超過 $20 即可免運，要新增一個按鈕表示若加入此項目可以超過 $20

```javascript
function update_shipping_icons() {
  // get all buy buttons on the page,then loop through them
  var buy_buttons = get_buy_buttons_dom();
  for(var i = 0; i < buy_buttons.length; i++) {
    var button = buy_buttons[i];
    var item = button.item;
    // figure out if they get free shipping
    if(item.price + shopping_cart_total >= 20)
      // show the icon appropriately
      button.show_free_shipping_icon();
    else
      // hide the icon appropriately
      button.hide_free_shipping_icon();
  }
}
```

```javascript
function calc_cart_total() {
  shopping_cart_total = 0;
  for(var i = 0; i < shopping_cart.length; i++) {
    var item = shopping_cart[i];
    shopping_cart_total += item.price;
  }
  set_cart_total_dom();
  // add a line to update the icons
  update_shipping_icons();
}
```

## 3. 計算稅
> Calculating tax

- 需要在每一次購物車總金額變動時，計算稅

```javascript
function update_tax_dom() {
  // update the dom, multiply the total by 10%
  set_tax_dom(shopping_cart_total * 0.10);
}
```

- 更新 calc_cart_total( )
```javascript
function calc_cart_total() {
  shopping_cart_total = 0;
  for(var i = 0; i < shopping_cart.length; i++) {
    var item = shopping_cart[i];
    shopping_cart_total += item.price;
  }
  set_cart_total_dom();
  update_shipping_icons();
  // add a line to update the tax on the page
  update_tax_dom();
}
```

## 4. 讓程式碼更具可測試性
> We need to make it more testable






## 5. 讓程式碼更具可重複使用性
> We need to make it more reusable














