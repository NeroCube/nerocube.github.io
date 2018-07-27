---
layout:     post
title:      "JWT - A new user authentication mechanism"
subtitle:   " \"Implement JWT auth server with Node.js in Docker.\""
date:       2018-07-15 12:00:00
author:     "Nero"
toc: 
    1. [什麼是 JWT ?](https://nerocube.github.io/2018/07/15/node_js_jwt/#什麼是-jwt-)
        1. [Header](https://nerocube.github.io/2018/07/15/node_js_jwt/#header)
        2. [Claims(Payload)](https://nerocube.github.io/2018/07/15/node_js_jwt/#claimspayload)
        3. [Signature](https://nerocube.github.io/2018/07/15/node_js_jwt/#signature)
    2. [與傳統 Session 有什麼不同](https://nerocube.github.io/2018/07/15/node_js_jwt/#與傳統-session-有什麼不同)
    3. [使用 JWT 的好處](https://nerocube.github.io/2018/07/15/node_js_jwt/#使用-jwt-的好處)
    4. [適合的使用時機](https://nerocube.github.io/2018/07/15/node_js_jwt/#適合的使用時機)
    5. [認證流程](https://nerocube.github.io/2018/07/15/node_js_jwt/#認證流程)
    6. [參考](https://nerocube.github.io/2018/07/15/node_js_jwt/#參考)
header-img: "img/post-bg-js-version.jpg"
tags:
    - Node.js
    - Docker
    - JWT
---
<!--ts-->

<!--te-->
> “使用 Node.js 在 Docker 實作 JWT Auth Server”

## 什麼是 JWT ?

[JSON Web Token (JWT)](https://jwt.io/) 是 [Auth0](https://auth0.com/?utm_source=jwtio&utm_campaign=craftedby) 提構出的一個新 Token 想法，一個 JWT 的長相會像下面這樣。
```
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c
```
他的構成如下。
```
Header.Payload.Signature
```
### Header
標頭包含了 `alg` 簽章或加密演算法與 `typ` Token 的類型。
```
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
```
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
```
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
