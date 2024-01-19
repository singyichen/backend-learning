---
title: Authentication
description: 
published: true
date: 2024-01-17T06:38:33.253Z
tags: authentication, 認證
editor: markdown
dateCreated: 2023-05-18T03:22:11.625Z
---

# Authentication
- [ ] [Basics of Authentication](https://roadmap.sh/guides/basics-of-authentication)
- [ ] [Session Based Authentication](https://roadmap.sh/guides/session-based-authentication)
- [ ] [HTTP Basic Authentication](https://roadmap.sh/guides/http-basic-authentication)
- [ ] [Password, Session, Cookie, Token, JWT, SSO, OAuth - Authentication Explained - Part 1](https://blog.bytebytego.com/p/password-session-cookie-token-jwt?utm_source=profile&utm_medium=reader2)
- [ ] [Password, Session, Cookie, Token, JWT, SSO, OAuth - Authentication Explained - Part 2](https://blog.bytebytego.com/p/password-session-cookie-token-jwt-ec1)
- [ ] [The beginner’s guide to magic links](https://postmarkapp.com/blog/magic-links#how-do-magic-links-work)
- [ ] [Learn about Auth0](https://auth0.com/docs)
- [ ] [簡單易懂的 OAuth 2.0](https://speakerdeck.com/chitsaou/jian-dan-yi-dong-de-oauth-2-dot-0)
- [ ] [[Note] OAuth2.0 筆記](https://pjchender.dev/internet/note-oauth2/)
- [ ] [The OAuth 2.0 Authorization Framework](https://gist.github.com/yorkxin/6590756)
- [ ] [OAuth 2.0 筆記 (1) 世界觀](https://blog.yorkxin.org/posts/oauth2-1-introduction/)
- [ ] [認識 OAuth 2.0：一次瞭解各角色、各類型流程的差異](https://www.technice.com.tw/experience/12520/)
- [ ] [[OIDC] 瞭解 OIDC 的表層](https://blog.kevinyang.net/2022/10/30/oidc-notes/)
- [ ] [只要十分鐘，帶你看懂 TOTP 2FA 的密碼學原理](https://medium.com/starbugs/totp-2fa-algorithm-in-10-mins-25acc3c35df9)
- [ ] [How to design a secure web API access for your website?](https://blog.bytebytego.com/p/how-to-design-a-secture-web-api-access?utm_source=profile&utm_medium=reader2)
- [ ] [What is SSO (Single Sign-On)?](https://blog.bytebytego.com/p/what-is-sso-episode-7?utm_source=profile&utm_medium=reader2)
- [ ] [EP34: Session, cookie, JWT, token, SSO, and OAuth](https://blog.bytebytego.com/p/ep34-session-cookie-jwt-token-sso?utm_source=profile&utm_medium=reader2)
- [ ] [Authentication in Computer Network](https://www.geeksforgeeks.org/authentication-in-computer-network/)
- [ ] [How to Build Secure and Scalable Authentication System with Node.js and MongoDB](https://sandydev.medium.com/how-to-build-secure-and-scalable-authentication-system-with-node-js-and-mongodb-c50bf51c06b0)
- [ ] [什麼是多重要素驗證 (MFA)？](https://aws.amazon.com/tw/what-is/mfa/)
- [ ] [Add Authentication to Any Web Page in 10 Minutes](https://medium.com/@bumurzaqov2/add-authentication-to-any-web-page-in-10-minutes-ecf3171269cb)
- [ ] [EP75: How Does A Password Manager Work](https://blog.bytebytego.com/p/ep75-how-does-a-password-manager?utm_source=profile&utm_medium=reader2)
- [ ] [EP91: REST API Authentication Methods](https://blog.bytebytego.com/p/ep91-rest-api-authentication-methods?utm_source=profile&utm_medium=reader2)
- [ ] [EP93: Is Passkey Shaping a Passwordless Future?](https://blog.bytebytego.com/p/ep93-is-passkey-shaping-a-passwordless?utm_source=profile&utm_medium=reader2)
- [ ] [Netflix: What Happens When You Press Play - Part 2](https://blog.bytebytego.com/p/netflix-what-happens-when-you-press-288?utm_source=profile&utm_medium=reader2)

![API keys vs tokens.png](http://192.168.25.60:8000/files/file_storage/f429ade5.png)

# Basics of Authentication

![Explaining Sessions Token JWT SSO and OAuth in One Diagram.png](http://192.168.25.60:8000/files/file_storage/b6461645.png)

![roadmap-basic-authentication.png](http://192.168.25.60:8000/files/file_storage/dcedea3c.png)

![Session cookie JWT token SSO and OAuth.png](http://192.168.25.60:8000/files/file_storage/cdb66600.png)

![Top 4 Forms of Authentication Mechanisms.png](http://192.168.25.60:8000/files/file_storage/cf32f555.png)


| 身份驗證機制 | 優點 | 缺點 | 
|:--:|:--:|:--:|
| 2FA (雙因素認證) | - 提供額外的安全層，增加帳戶的安全性- 使用多個驗證因素（例如密碼和一次性驗證碼）進行驗證- 防止未經授權的用戶訪問帳戶- 支持多個驗證方法，如短信、驗證器應用或硬體密鑰 | - 需要用戶進行額外的身份驗證步驟，可能會增加使用者體驗的複雜度- 部署和管理2FA的基礎架構可能需要額外的成本和努力 |
| Magic Link (魔法連結) | - 簡單易用，無需輸入密碼即可進行身份驗證- 通過發送包含驗證令牌的電子郵件鏈接來進行驗證- 提供方便的使用者體驗，尤其適合移動設備使用者 | - 需要使用者擁有有效的電子郵件地址- 需要使用者訪問電子郵件帳戶以獲取驗證鏈接- 安全性取決於電子郵件的安全性和使用者的電子郵件帳戶的控制 |
| OAuth 2.0 | - 提供方便的第三方身份驗證，用戶可以使用其他服務（如Google、Facebook）的帳戶進行登錄- 適用於構建應用程序和服務之間的信任關係- 提供授權和訪問令牌，實現安全的資源共享- 支持用戶授權的範圍和權限管理 | - 需要對OAuth 2.0協議進行理解和實施- 需要與第三方身份驗證提供者進行集成- 需要管理和保護訪問令牌的安全性 |

## Single Sign On (SSO)

![roadmap-sso.png](http://192.168.25.60:8000/files/file_storage/c326ca39.png)

## Session Based Authentication

![roadmap-session-authentication.png](http://192.168.25.60:8000/files/file_storage/c326ca39.png)

## Token-Based Authentication

![roadmap-token-authentication.png](http://192.168.25.60:8000/files/file_storage/be5dae07.png)

## JWT Authentication

![roadmap-jwt-authentication.png](http://192.168.25.60:8000/files/file_storage/390c0d1d.png)

## OAuth - Open Authorization

![roadmap-oauth.png](http://192.168.25.60:8000/files/file_storage/90c093b4.png)

## Magic Link

## OTP

## Two-factor authentication (2FA)

## MFA - Multi-factor Authentication

