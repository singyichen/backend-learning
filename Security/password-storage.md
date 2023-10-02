---
title: Password Storage
description: 
published: true
date: 2023-09-26T08:59:46.583Z
tags: security
editor: markdown
dateCreated: 2023-09-25T01:31:00.457Z
---

# Password Storage
- [ ] [後端基礎：資安細節隨手筆記](https://hugh-program-learning-diary-js.medium.com/%E5%BE%8C%E7%AB%AF%E5%9F%BA%E7%A4%8E-%E8%B3%87%E5%AE%89%E7%B4%B0%E7%AF%80%E9%9A%A8%E6%89%8B%E7%AD%86%E8%A8%98-deb1e6252944)
- [ ] [How Dropbox securely stores your passwords](https://dropbox.tech/security/how-dropbox-securely-stores-your-passwords)
- [ ] [Password Storage Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Password_Storage_Cheat_Sheet.html)
- [ ] [聽說不能用明文存密碼，那到底該怎麼存？](https://medium.com/starbugs/how-to-store-password-in-database-sefely-6b20f48def92)
- [ ] [一次搞懂密碼學中的三兄弟 — Encode、Encrypt 跟 Hash](https://medium.com/starbugs/what-are-encoding-encrypt-and-hashing-4b03d40e7b0c)
- [ ] [[資訊安全] 密碼存明碼，怎麼不直接去裸奔算了？淺談 Hash , 用雜湊保護密碼](https://medium.com/@brad61517/%E8%B3%87%E8%A8%8A%E5%AE%89%E5%85%A8-%E5%AF%86%E7%A2%BC%E5%AD%98%E6%98%8E%E7%A2%BC-%E6%80%8E%E9%BA%BC%E4%B8%8D%E7%9B%B4%E6%8E%A5%E5%8E%BB%E8%A3%B8%E5%A5%94%E7%AE%97%E4%BA%86-%E6%B7%BA%E8%AB%87-hash-%E7%94%A8%E9%9B%9C%E6%B9%8A%E4%BF%9D%E8%AD%B7%E5%AF%86%E7%A2%BC-d561ad2a7d84)

# 密碼學
## 編碼（Encode）
> - 只是換個方式表達資料
> - 不需要 Key 即可解碼（不安全）

## 加密（Encrypt）
> - 用 Key 來保護資料的機密性
> - 加密跟解密都需要 Key

## 雜湊（Hash）
> - 把資料丟進一串公式計算出一個結果
> - 無法反推回原字串

# 三種演算法的比較
| 類型 | 不可逆演算法 | 對稱加密演算法 | 非對稱加密演算法 |
|---|:--:|:--:|:--:|
| 可逆性 | 不可逆 | 可逆 | 可逆 |
| 加密速度 | 快 | 快 | 慢 |
| 解密速度 | 快 | 快 | 慢 |
| 安全性 | 高 | 高 | 高 |
| 應用場景 | 密碼雜湊存儲 | 資料加密、檔案加密、磁碟加密 | 數位簽名、電子商務、身份驗證 |


[密碼加密 Mind](https://xmind.app/m/pE4BX4)



