---
layout: post
title: "android animation"
date: 2016-03-23 02:30:53 +0800
comments: true
categories: android
description: Android 主要有兩種 Animation System，這篇文章專注在介紹較新的 Property Animation
---

Android 主要有兩種 Animation System，這篇文章專注在介紹較新的 Property Animation

## Overview

> Android framework provides two animation systems: property animation (introduced in Android 3.0) and view animation.

<!-- more -->

## How Property Animation Differs from View Animation

首先先說明一下這兩者的差別

在過去 View Animation 只能夠對繼承 View 類別的物件進行屬性變化，而且還有一個致命的缺點就是 View Animation 其實只是改變 View 繪圖的座標，並非實際改變 View 類別本身，這就會發生一個 Button 被移動後，觸發 Click 事件的座標卻還留在原地的情況。

## Property Animation

A property animation changes a property's (a field in an object) value over a specified length of time.

 * Duration (屬性變化時間總長)
 * Time interpolation  (屬性變化函數)
 * Repeat count and behavior
 * Animator sets
 * Frame refresh delay

## How Property Animation Works

Property Animation 的運作方式就是使用 ValueAnimator 這個類別，給予物件動畫開始時和結束時的屬性值以及整個動畫經過的 Duration

當你呼叫 ValueAnimator.start() 就會讓動畫開始，物件的屬性會藉由內部的 Interpolator 隨著妳設定的 Duration 進行改變 (預設是線性變化)

## API Overview

你可以在 <a target="_blank" href="http://developer.android.com/intl/zh-tw/reference/android/animation/package-summary.html">android.animation</a> 這個 package 下找到大部分的 Property Animation 的類別，不過在 Interpolator 的部分則是大多與過去的 <a target="_blank" href="http://developer.android.com/intl/zh-tw/reference/android/view/animation/package-summary.html">View Animation</a> 共用

在 Property Animation 中，主要的 API 有下列幾項

* Animator
  * ValueAnimator
  * ObjectAnimator
  * AnimatorSet

* Evaluators

* Interpolators

## View Property & Quick Example

Android 當中的 View 有許多屬性可以直接進行動畫

* View.ALPHA
* View.ROTATION
* View.ROTATION_X
* View.ROTATION_Y
* View.SCALE_X
* View.SCALE_Y
* View.TRANSLATION_X
* View.TRANSLATION_Y
* View.TRANSLATION_Z (After API Level 21)
* View.X
* View.Y
* View.Z (After API Level 21)

下面是一個動畫的例子

{% codeblock lang:java %}
PropertyValuesHolder pvhX = PropertyValuesHolder.ofFloat(View.X, 50f);
PropertyValuesHolder pvhY = PropertyValuesHolder.ofFloat(View.Y, 100f);
ObjectAnimator anim = ObjectAnimator.ofPropertyValuesHolder(mTextView, pvhX, pvhY);

anim.setDuration(1000);
anim.start();
{% endcodeblock %}

更多實際的例子可以參考我的 Github Repositry: <a href="https://github.com/kurorido/Android-Animation-Example">Android Animation Example</a> 快速了解每一種動畫效果

## Animation Listeners

在 Animation 的開始、進行、結束上，可以註冊 Listener 來進行額外操作

* Animator.AnimatorListener
  * onAnimationStart() - Called when the animation starts.
  * onAnimationEnd() - Called when the animation ends.
  * onAnimationRepeat() - Called when the animation repeats itself.
  * onAnimationCancel() - Called when the animation is canceled. A cancelled animation also calls
  * onAnimationEnd(), regardless of how they were ended.

甚至是任何的 Value Update 都能夠客製化

* ValueAnimator.AnimatorUpdateListener
  * onAnimationUpdate()

## XML Directory - Animator? Animation?

為了要跟 View Animation 區別，新的 Property Animation 是放在 /res/animator/ 資料夾底下，過去的 View Animation 則是放在 /res/anim 底下

關於如何用 XML 動畫可以參考 <a target="_blank" href="http://developer.android.com/intl/zh-tw/guide/topics/resources/animation-resource.html#Property">Android Animation Resource</a>