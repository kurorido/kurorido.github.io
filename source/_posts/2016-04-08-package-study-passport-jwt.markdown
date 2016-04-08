---
layout: post
title: "Package Study - passport-jwt"
date: 2016-04-08 17:33:28 +0800
comments: true
categories: javascript
---

先查看 index.js 會輸出兩個模組，

其中 Strategy 需要實作 passport-strategy 的 authenticate 方法

{% codeblock lang:javascript index.js %}
module.exports = {
    Strategy: Strategy,
    ExtractJwt : ExtractJwt
};
{% endcodeblock %}

在 authenticate 的部分

JwtStrategy.JwtVerifier 其實是引入 verify_jwt.js 這隻檔案的模組
事實上 verify_jwt 背後只是用 jsonwebtoken 這個 package 來做 verify 而已

{% codeblock lang:javascript index.js %}

// 先取出 token
var token = self._jwtFromRequest(req);

// 如果 token 不存在就 fail
if (!token) {
    return self.fail(new Error("No auth token"));
}

// 見 JwtStrategy.JwtVerifier = require('./verify_jwt');
JwtStrategy.JwtVerifier(token, this._secretOrKey, this._verifOpts, function(jwt_err, payload) {
    if (jwt_err) { // 如果 validate 不過會 fail
        return self.fail(jwt_err);
    } // 剩下的就是驗證項目

{% endcodeblock %}

在實際使用的時候發現 self.fail 不會呼叫 passport verify 的 callback，需要改成 self.error

因此我發了一筆 issues 在 gituhb 上
https://github.com/themikenicholson/passport-jwt/issues/56