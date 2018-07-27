---
layout:     post
title:      "JWT - A new user authentication mechanism"
subtitle:   " \"Introduction to the JSON Web Token mechanism and usage scenarios.\""
date:       2018-07-15 12:00:00
author:     "Nero"
toc: <li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#什麼是-jwt-"><span class="toc-nav-number">1.</span><span class="toc-nav-text">什麼是JWT?</span></a><ol class="toc-nav-child"><li class="toc-nav-item toc-nav-level-3"><a class="toc-nav-link"href="#header"><span class="toc-nav-number">1.1.</span><span class="toc-nav-text">Header</span></a></li><li class="toc-nav-item toc-nav-level-3"><a class="toc-nav-link"href="#claimspayload"><span class="toc-nav-number">1.2.</span><span class="toc-nav-text">Claims(Payload)</span></a></li><li class="toc-nav-item toc-nav-level-3"><a class="toc-nav-link"href="#signature"><span class="toc-nav-number">1.3.</span><span class="toc-nav-text">Signature</span></a></li></ol></li><li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#與傳統-session-有什麼不同"><span class="toc-nav-number">2.</span><span class="toc-nav-text">與傳統Session有什麼不同</span></a></li><li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#使用-jwt-的好處"><span class="toc-nav-number">3.</span><span class="toc-nav-text">使用JWT的好處</span></a><ol class="toc-nav-child"><li class="toc-nav-item toc-nav-level-3"><a class="toc-nav-link"href="#跨服務認證"><span class="toc-nav-number">3.1.</span><span class="toc-nav-text">跨服務認證</span></a></li><li class="toc-nav-item toc-nav-level-3"><a class="toc-nav-link"href="#提升安全性"><span class="toc-nav-number">3.2.</span><span class="toc-nav-text">提升安全性</span></a></li><li class="toc-nav-item toc-nav-level-3"><a class="toc-nav-link"href="#降低資料庫負載"><span class="toc-nav-number">3.3.</span><span class="toc-nav-text">降低資料庫負載</span></a></li></ol></li><li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#適合的使用時機"><span class="toc-nav-number">4.</span><span class="toc-nav-text">適合的使用時機</span></a></li><li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#認證流程"><span class="toc-nav-number">5.</span><span class="toc-nav-text">認證流程</span></a></li><li class="toc-nav-item toc-nav-level-2"><a class="toc-nav-link"href="#參考"><span class="toc-nav-number">6.</span><span class="toc-nav-text">參考</span></a></li>
header-img: "img/post-bg-js-version.jpg"
tags:
    - Auth
    - JWT
    - 學習筆記
---

> “簡介JSON Web Token 機制與使用情境”

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
Claims部分包含了一些跟這個 Token 有關的重要信息。JWT標準規定了一些字段，下面為部分字段：

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
    "id": "UxHeJOke5C4E"
}
```
### Signature
簽章的部分為將 Header 與 Claims 透過 Base64 轉換，再依據在 Header 中的演算類型加密，所使用的`secret`只有簽章的 Server 知道所以攻擊者無法偽造內容除非暴力解，因為是以 Base64 轉換的所以不適合把重要資訊放在當中。

```
content = base64url_encode(Header) + '.' + base64url_encode(Claims)
signature = hmacsha256.hash(content)
```

## 與傳統 Session 有什麼不同
傳統使用 Session 在 Server 端需要將用來識別使用者的 Token 紀錄在快取或資料庫中，如下圖，每次 request 進來經過 Load Balance 進到不同的 Backend Server，會造成在 Server1 可以識別，而 Server2 沒有用戶的 Session 所以無法識別出是誰的請求，可以透過一個共用的快取(Radis)或資料庫解決這個問題，但在這樣的分散式系統中這會造成維護與架構上的複雜度提高。

<p align="center">
  <img width="100%" height="100%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2018-07-13-node_js_jwt/session-load-balance-flow.png">
</p>


## 使用 JWT 的好處
### 跨服務認證
JWT 本身為`無狀態性的`（Stateless），當 Backend Server 收到 Token 再依據定義在 Header 中的 `alg` 進行解密，所以不需要使用資料庫額外紀錄才能做到識別用戶，這樣的機制方便於實作 [Single sign-on](https://zh.wikipedia.org/wiki/%E5%96%AE%E4%B8%80%E7%99%BB%E5%85%A5) 與 Load Balance 架構。

### 提升安全性 
傳統 Session 的機制如果惡意的攻擊者傳送如：刪除帳號等，危險操做的連結給用戶，用戶在不知情得其況下點擊連結，用戶的瀏覽器便會將 cookie 帶入請求中，Backend Server 收到請求便會認為是用戶正常操作，將用戶帳號刪除。而 JWT 機制在點擊惡意連結時，因連結`不帶有 Token`，所以該操做不會被執行。

### 降低資料庫負載
後端收到 Token 在 middleware 層便可以識別用戶，不需要每次去資料庫中詢問這個使用者是誰，大幅度的減少在認證過程中資料庫的負擔。

## 適合的使用時機
1. JWT 其實很單純做的只有一件事情，就是發識別牌，所以當將 Token 簽出去的瞬間變沒辦法對 Token 的內容做修該，只能等到 Token 過期，所以重設密碼或希望強制需要而外實作機制過濾 Token 但這就無法享受跨服務認證的好處，所以比較適合的方式為將 `exp` 的時間設為固定時間，確保在該時間用戶便無法使用或 `expiresIn` 時間設定短一點，讓用戶需要 `refresh` 或 `login`。
2. Token 為 Base64 加密，所以不適合在 Token Payload 中放入重要的資訊，避免任意的使用者可以透過 Base64 解密看到用戶重要的資訊。

## 認證流程
<p align="center">
  <img width="100%" height="100%" src="https://raw.githubusercontent.com/NeroCube/nerocube.github.io/master/img/in-post/2018-07-13-node_js_jwt/client-credentials-grant-flow.png">
</p>

1. client端發送登入的請求，這時可能會附上帳號密碼等驗證資訊，server端驗證登入資訊後，搭配一個密鑰(secret key)來產生JWT token
2. 將token回傳給client端。
3. client端請求資料時，在request header的Authoriaztion中加上這個token，server端驗證token的簽章是否正確，並從payload中得知user的資訊。
4. 驗證無誤的話，回傳client端請求的資料

## 參考
[JWT Official Website](https://jwt.io/)

[Passport.js](http://www.passportjs.org/docs/downloads/html/)
