---
layout: post
title: "建立自己的第一個 Github Page"
date: 2016-03-23 00:54:15 +0800
comments: true
categories: misc
---

研究了主要兩種 Solution, 第一種是 jekyll 另外一種是基於 jekyll 的框架 Octpress。

兩者架起來都不難，但是 jekyll 的初始版本比較陽春，octpress 則會先包好一些第三方的套件 (尤其是社交以及追蹤分析的部分)

<!-- more -->

octpress 比較難找到 theme 可以套，但是 jekyll 比較多，不過套別人現成 theme 也是很困難的一件事情...改用 octpress 就是因為套 theme 不順利

總之有一個有 code block 的 blog 而且好客製化是第一步...

各種 blog function 測試
***

[I'm an inline-style link with title](https://www.google.com "Google's Homepage")

> Blockquotes are very handy in email to emulate reply text.
> This line is part of the same quote.

{% blockquote %}
Last night I lay in bed looking up at the stars in the sky and I thought to myself, where the heck is the ceiling.
{% endblockquote %}

{% include_code test.js %}

* Java Code Block
{% codeblock lang:java java %}
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);

    mTextView = (TextView) findViewById(R.id.target_text);
    mButton = (Button) findViewById(R.id.action_button);

    mButton.setOnClickListener(new View.OnClickListener() {
        @Override
        public void onClick(View v) {
            startAnimation();
        }
    });
}
{% endcodeblock %}

* PHP Code block
{% codeblock lang:php php %}
$keywords = array();
$links = array();
foreach($keywordLinks as $keywordLink) {
    $keyword = $keywordLink->keyword;
    $link = $keywordLink->link;
    $target = $keywordLink->target;
    $keywords[] = $keyword;
    $links[] = \Html::link($link, $keyword, array('target' => $target, 'class' => 'blog-keyword'));
}
{% endcodeblock %}