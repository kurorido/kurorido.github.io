---
layout: post
title: "What I have learned from JSConf Topic What the heck is the event loop anyway"
date: 2016-03-24 17:35:42 +0800
comments: true
categories: javascript
---

觀看 Youtube 上 JSConf 的影片稍微整理了一下筆記

這個影片的內容簡單的介紹了 Javascript 運作的方法

但我覺得還是沒講到很深的 event loop 到底是什麼...

<!-- more -->

<div class="video-container">
<iframe width="100%" height="480" src="https://www.youtube.com/embed/8aGhZQkoFbQ" frameborder="0" allowfullscreen></iframe>
</div>

首先要先理解 Javascript 跑在瀏覽器上時大致分成三個層面

* Call Stack
* Web Apis
* Callback Queue

Javascript 本身是 single thread 在運作的，所以所有的 Function Call 都會進入 Call Stack，當中只要有需要跑很久的 Task 在中間的話就會 Block 住下一個 Function Call

這時候可以透過 WebApis 的 setTimeout(cb, 0) 來將 function call 變成非同步(Async)，可以讓需要跑很久的 Task 在運作完成後推入 Callback Queue，並且在適當的時間點推回 Call Stack

主講人介紹了他製作的 <a href="http://latentflip.com/loupe/?" target="_blank">Call Stack 視覺化工具</a>

實際在視覺化工具上跑下面這段 code 並且觀察 call stack 和 callback queue 的變化

{% codeblock lang:javascript %}
// Synchronous
[1,2,3,4].forEach(function(i)) {
    console.log(i);
})

// Asynchronous
function asyncForEach(array, cb) {
    array.forEach(function() {
        setTimeout(cb, 0);
    });
}

asyncForEach([1,2,3,4], function(i) {
    console.log(i);
});
{% endcodeblock %}

另外值得注意的是主講人有提到 setTimeout 的 timeout 並非是保證多久之後會執行的時間...