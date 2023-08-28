---
title: Async Await
description: 
published: true
date: 2023-08-28T00:12:27.213Z
tags: promise, async, await
editor: markdown
dateCreated: 2022-05-26T03:31:40.760Z
---

# Async Await
- [ ] [Async function / Await 深度介紹](https://www.casper.tw/development/2020/10/16/async-await/)
- [ ] [我要學會 JS(三)：callback、Promise 和 async/await 那些事兒](https://noob.tw/js-async/)
- [ ] [callback, promise, async/await 使用方式教學以及介紹 Part I](https://yu-jack.github.io/2018/07/22/promise/)
- [ ] [JavaScript Visualized: Promises & Async/Await](https://medium.com/@masterrajpatel/javascript-visualized-promises-async-await-%EF%B8%8F-c51af935f7f5)
- [ ] [Difference Between Promise and Async/Await](https://medium.com/version-1/difference-between-promise-and-async-await-95e453182f55)

# 只有這四種寫法才會是 promise 的函式
```javascript
const func1 = async () => {
}

async func1() {
}

const func1 = () => {
    return  new Promise((resolve, reject) => {
    };
}

function func1() {
    return  new Promise((resolve, reject) => {
    };
}
```
- example
```javascript
// 加上 async 後函式內才能使用 await
async doAuthenticate(ad, userName , password , employeeID , res) {
    // try ... catch 只對 async 的寫法有用，new Promise 的寫法沒辦法被 try catch 攔截到
    try {
        //宣告箭頭函式，因為裡面是返回 new promise 了，所以箭頭函式不用再宣告 async
        const authenticateInfo = () => {
            // 因為要等待 callback 完成，所以一定要用 promise(resolve, reject) 的寫法，下面這個會宣告出來
            return new Promise((resolve, reject) => {
                // 第一種寫法
                // 然後把要做的事情擺在這邊，這裡面不需要加 await，因為用 callback 函式的寫法，不會是一個 promise，所以不能被 await
                ad.authenticate(userName, password, function (err, auth) {
                    ///callback 函式運行的地方，並且 callback 事情做完後才呼叫 resolve 讓整個 promise 結束
                    // ...其餘要做的事情
                    const result = 'test';
                    return resolve(result); // 前面要加 return 沒加的話，後面的程式還會繼續執行
                    console.log('上面沒有 return 的話，這裡還會執行下去');
                    console.log('一直到整個這個區塊都沒有為止');
                });
                // 第二種寫法
                // 除非 ad.authenticate 這個函式同時有被宣告成 promise 才可以用 await，但是寫法上就不能用 callback，會長得像這樣
                // const auth = await ad.authenticate(userName, password);
                // 這裡的 auth 就會對應上面 callback 拿到的 auth
                // 只有這兩種寫法，選一種寫就好，但通常定義成 callback 函式寫法的不一定會被宣告成promise，所以有可能沒辦法用第二種寫法

                // 如果在這邊拋出例外的話，會跑到下面的 catch 區塊，reject 跟 resolve 一樣要在前面加 return 
                return reject(err);
            }).catch((e) => {
                // 呈最上面說的 try catch 問題，promise 的例外處理一定要像這樣用 catch 來處理                
                console.error(e.message);
            });
        };
        await authenticateInfo();
    } catch (error) {
        console.log(error)
    }
}
```