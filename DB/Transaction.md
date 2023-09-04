---
title: Transaction
description: 
published: true
date: 2023-09-01T01:43:16.173Z
tags: db
editor: markdown
dateCreated: 2023-09-01T00:38:47.804Z
---

# Transaction
- [ ] [Database Transaction & ACID](https://oldmo860617.medium.com/database-transaction-acid-156a3b75845e)
- [ ] [Transaction 併發錯誤與隔離層級 ](https://oldmo860617.medium.com/transaction-%E4%BD%B5%E7%99%BC%E9%8C%AF%E8%AA%A4%E8%88%87%E9%9A%94%E9%9B%A2%E5%B1%A4%E7%B4%9A-51b8af6178ae)
- [ ] [資料庫 Transaction & Lock 筆記](https://hackmd.io/@Burgess/SkDnHKMNr)
- [ ] [SQL - Transactions](https://www.tutorialspoint.com/sql/sql-transactions.htm)
- [ ] [SQL | TRANSACTIONS](https://www.geeksforgeeks.org/sql-transactions/)
- [ ] [Transactions in SQL Server for beginners](https://www.sqlshack.com/transactions-in-sql-server-for-beginners/)
- [ ] [What is SQL Transaction](https://www.tutorialgateway.org/sql-transaction/)

# 什麼是 Transaction ?
> - Transaction，中文翻作交易或事務，是資料庫執行過程中的一個「邏輯單位」，一個 transaction 中包含多個對資料庫操作的行為，每個 transaction 有兩種可能的結局：全部執行成功 or 全部不執行（只要其中一個行為失敗就全部回滾）
> - Transaction 主要是解決需要一起發生的事件但事件或事件的參與者不同時或不一致的問題。
> - 交易通常用於資料庫管理系統中，以確保資料的一致性。例如，在轉帳操作中，如果要從一個帳戶轉出錢，同時要將錢轉入另一個帳戶，這兩個操作必須同時成功，否則轉帳操作就會失敗。如果在轉帳過程中發生錯誤，例如網路中斷，導致轉出操作成功但轉入操作失敗，資料庫管理系統會回滾轉帳操作，將錢退回到原來的帳戶中。

![什麼是 Transaction.png](http://192.168.25.60:8000/files/file_storage/1ceaff02.png)

# Transaction 常見指令
交易通常由以下幾個步驟組成：

1. 開始交易：標記交易的開始。
2. 準備階段：檢查交易的條件是否滿足，並對數據進行操作前的準備工作。
3. 提交階段：將交易的結果提交到資料庫中。
4. 回滾階段：如果交易失敗，則回滾交易，將數據恢復到開始交易之前的狀態。

## START TRANSACTION
> 建立一個 transaction

## COMMIT
> TRANSACTION 中的操作結束時進行 commit

## ROLLBACK
> 也就是上面提到的可以將交易回滾

## SAVEPOINT
> 建立儲存點

## RELEASE SAVEPOINT
> 刪除儲存點

## ROLLBACK TO SAVEPOINT
> rollback 回某個儲存點

```sql
BEGIN TRANSACTION 
INSERT INTO Person 
VALUES('Mouse', 'Micky','500 South Buena Vista Street, Burbank','California',43)
SAVE TRANSACTION InsertStatement
DELETE Person WHERE PersonID=3
SELECT * FROM Person 
ROLLBACK TRANSACTION InsertStatement
COMMIT
SELECT * FROM Person
```

# Transaction 四大特性：ACID

![Transaction 四大特性 ACID.png](http://192.168.25.60:8000/files/file_storage/6165c150.png)

## Atomicity 原子性
> transaction 內所有操作全部完成或全部不完成，中間錯誤會被 rollback

![Transaction 四大特性 ACID Atomicity 原子性.png](http://192.168.25.60:8000/files/file_storage/33bf124d.png)

## Consistency 一致性
> 保持資料完整一致性

![Transaction 四大特性 ACID Consistency 一致性.png](http://192.168.25.60:8000/files/file_storage/1395b130.png)

## Isolation 隔離性
> 允許多筆 transaction 同時對資料讀寫跟修改能力，且避免多筆 transaction 執行時由於交叉執行而導致數據的不一致(race condition)

![Transaction 四大特性 ACID Isolation 隔離性.png](http://192.168.25.60:8000/files/file_storage/7200f9a2.png)

## Durability 永久性
> 對資料的修改是永久的，即便系統故障也不會丟失

![Transaction 四大特性 ACID Durability 永久性.png](http://192.168.25.60:8000/files/file_storage/9ab720d0.png)




