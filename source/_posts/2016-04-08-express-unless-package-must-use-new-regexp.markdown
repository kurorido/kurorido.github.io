---
layout: post
title: "Express unless package must use new RegExp()"
date: 2016-04-08 19:03:30 +0800
comments: true
categories: javascript
---

最近使用了 express-unless 這套 package，發現 path 不管怎麼設定都不對

後來發現一定得用 new RegExp(); 來帶入 path，不然會沒辦法被認為是正確的

追到 lib/index.js 裡面的一行

{% codeblock lang:javascript lib/index.js %}
var ret = (typeof p === 'string' && p === url) || (p instanceof RegExp && !!p.exec(url));
{% endcodeblock %}

因為這邊 p instanceof RegExp 如果使使用一般的 string expression 就不會通過 (ex. '/\/path/g')
