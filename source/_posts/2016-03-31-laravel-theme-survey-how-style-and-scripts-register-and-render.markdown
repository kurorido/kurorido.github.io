---
layout: post
title: "laravel theme survey - how style and scripts register and render?"
date: 2016-03-31 15:55:27 +0800
comments: true
categories: laravel
description: 最近因為要在專案動態套用不同 Theme，所以研究了一下 Laravel Theme 大致上如何運作，基本上只研究了包含 Scripts, Styles 的部分，其他 Widget, Breadcumb 則因為沒用到所以沒有研究。
---
## Introduction

最近因為要在專案動態套用不同 Theme，所以研究了一下 <a href='https://github.com/teepluss/laravel-theme' rel='nofollow' target='_blank'>Laravel Theme</a> 大致上如何運作，基本上只研究了包含 Scripts, Styles 的部分，其他 Widget, Breadcumb 則因為沒用到所以沒有研究。

<!-- more -->

## 基本的 call flow

1. Theme->uses()->layout() 初始化 Theme
2. Theme->asset()->usePath()->add() 加入 Style, Scripts
3. 最後在 blade 的部分透過 Theme::asset()->styles() 輸出 styles

## 實際看下面的 code flow

{% codeblock lang:php 加入 Styles, Scripts  %}
// Theme.php
public function asset()
{
    return $this->asset;
}

// Asset.php
public function __call($method, $parameters)
{
    return call_user_func_array(array(static::container(), $method), $parameters);
}

// 這邊取得相對應的 AssetContainer instance
public static function container($container = 'default')
{
    if ( ! isset(static::$containers[$container]))
    {
        static::$containers[$container] = new AssetContainer($container);
    }

    return static::$containers[$container];
}

// AssetContainer.php
protected function added($name, $source, $dependencies = array(), $attributes = array())
{
    // skip... 這邊會串到 style 或是 script function 註冊 resources
}

// 以 script 為例
public function script($name, $source, $dependencies = array(), $attributes = array())
{
    // Prepaend path to theme.
    if ($this->isUsePath())
    {
        $source = $this->evaluatePath($this->getCurrentPath().$source);

        // Reset using path.
        $this->usePath(false);
    }

    $this->register('script', $name, $source, $dependencies, $attributes);

    return $this;
}
{% endcodeblock %}

## 最後來看如何輸出

{% codeblock lang:php 在 Blade 內輸出 Styles, Scripts  %}
// 一樣透過 Asset.php 的 magic method 操作 AssetContainer
public function __call($method, $parameters)
{
    return call_user_func_array(array(static::container(), $method), $parameters);
}

// AssetContainer.php
public function styles()
{
    return $this->group('style');
}

protected function asset($group, $name)
{
    // ... skip
    return $this->html($group, $asset['source'], $asset['attributes']);
}
{% endcodeblock %}