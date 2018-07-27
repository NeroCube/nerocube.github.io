---
layout:     post
title:      "JWT - A new user authentication mechanism"
subtitle:   " \"Implement JWT auth server with Node.js in Docker.\""
date:       2018-07-15 12:00:00
author:     "Nero"
toc: <li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="##什麼是-jwt-"><span class="toc-nav-number">1.</span><span class="toc-nav-text">什麼是JWT?</span></a><ol class="toc-nav-child"><li class="toc-nav-item toc-nav-level-3"><a class="toc-nav-link"href="#header"><span class="toc-nav-number">1.1.</span><span class="toc-nav-text">Header</span></a></li><li class="toc-nav-item toc-nav-level-3"><a class="toc-nav-link"href="#claimspayload"><span class="toc-nav-number">1.2.</span><span class="toc-nav-text">Claims(Payload)</span></a></li><li class="toc-nav-item toc-nav-level-3"><a class="toc-nav-link"href="#signature"><span class="toc-nav-number">1.3.</span><span class="toc-nav-text">Signature</span></a></li></ol></li><li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#與傳統-session-有什麼不同"><span class="toc-nav-number">2.</span><span class="toc-nav-text">與傳統Session有什麼不同</span></a></li><li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#使用-jwt-的好處"><span class="toc-nav-number">3.</span><span class="toc-nav-text">使用JWT的好處</span></a></li><li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#適合的使用時機"><span class="toc-nav-number">4.</span><span class="toc-nav-text">適合的使用時機</span></a></li><li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#認證流程"><span class="toc-nav-number">5.</span><span class="toc-nav-text">認證流程</span></a></li><li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#參考"><span class="toc-nav-number">6.</span><span class="toc-nav-text">參考</span></a></li>
header-img: "img/post-bg-js-version.jpg"
tags:
    - Node.js
    - Docker
    - JWT
---

> “使用 Node.js 在 Docker 實作 JWT Auth Server”

## 什麼是 JWT ?

[JSON Web Token (JWT)](https://jwt.io/) 是 [Auth0](https://auth0.com/?utm_source=jwtio&utm_campaign=craftedby) 提構出的一個新 Token 想法，一個 JWT 的長相會像下面這樣。
```bash
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
他的構成如下。
```bash
Header.Payload.Signature
```
### Header
標頭包含了 `alg` 簽章或加密演算法與 `typ` Token 的類型。
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```
目前所支持的演算法如下。

alg Parameter Value | Digital Signature or MAC Algorithm
----------------|----------------------------
HS256 | HMAC using SHA-256 hash algorithm
HS384 | HMAC using SHA-384 hash algorithm
HS512 | HMAC using SHA-512 hash algorithm
RS256 | RSASSA using SHA-256 hash algorithm
RS384 | RSASSA using SHA-384 hash algorithm
RS512 | RSASSA using SHA-512 hash algorithm
ES256 | ECDSA using P-256 curve and SHA-256 hash algorithm
ES384 | ECDSA using P-384 curve and SHA-384 hash algorithm
ES512 | ECDSA using P-521 curve and SHA-512 hash algorithm
none | No digital signature or MAC value included

### Claims(Payload)
Claims部分包含了一些跟這個令有關的重要信息.JWT標準規定了一些字段，下面節選一些字段：

* `iss`：Token 的發行者，令牌是給誰的
* `sub`：Token 主題，令牌主題
* `exp`：到期時間。Token 過期時間，Unix時間戳格式
* `iat`：發佈時間。Token 創建時間，Unix時間戳格式
* `jti`：JWT ID。針對當前 Token 的唯一標識

Payload 格式如下。
```json
{
    "iss": "John Wu JWT",
    "iat": 1441593502,
    "exp": 1441594722,
    "aud": "www.example.com",
    "sub": "jrocket@example.com",
    "from_user": "B",
    "target_user": "A"
}
```
### Signature
簽章的部分為將 Header 與 Claims 透過 Base64 轉換，再依據在 Header 中的演算類型加密，所使用的`secret`只有簽章的 Server 知道所以攻擊者無法偽造內容除非暴力解，因為是以 Base64 轉換的所以不適合把重要資訊放在當中。

```javascript
content = base64url_encode(Header) + '.' + base64url_encode(Claims)
signature = hmacsha256.hash(content)
```

## 與傳統 Session 有什麼不同

## 使用 JWT 的好處

## 適合的使用時機

## 認證流程

## 參考
[JWT Official Website](https://jwt.io/)

[Passport.js](http://www.passportjs.org/docs/downloads/html/)
