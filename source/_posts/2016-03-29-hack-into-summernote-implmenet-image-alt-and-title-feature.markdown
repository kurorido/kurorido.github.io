---
layout: post
title: "Hack into Summernote - Implmenet image alt and title feature"
date: 2016-03-29 23:03:52 +0800
comments: true
categories: javascript
---

## Introduction

<a href='https://github.com/summernote/summernote'>Summernote</a> is an open source WYSIWYG editor like tinymce or ckeditor. I like it because it already implements almost the necessary features.  However, it lacks of function to add 'alt' & 'title' attributes, which are important to SEO, on image elements.  This articles is to tell you how to hack into summernote and implement this feature by yourself.

<!-- more -->

## Prerequisite

* know how to build up summernote with Grunt
* I'm using the version base on developer branch commit (251e68d) - after version 0.8.1

## Summernote Structure Brief

Almost all core source code is put under src/js directory and the core of summernote is under src/js/base directory.

## Steps

you can see all commit change <a href='https://github.com/kurorido/summernote/commit/f8beafc227d68c8ebe9514235b43648a40383716'>here</a> and the following is the reason why change those file.

* Step 1 - Create our own ImagePropertiesDialog.js under bs3/module directory.
* Step 2 - Modify bs3/settings.js to register this module.
* Step 3 - Modify bs3/module/Button.js to give an entry point for this module.
* Step 4 - Implement communication function between editor and dialog in base/module/Editor.js