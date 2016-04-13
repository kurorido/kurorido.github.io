---
layout: post
title: "Use open_jtalk on Ubuntu 14.04"
date: 2016-04-13 20:44:24 +0800
comments: true
categories: misc
---

Open Jtalk 是一套可以把日文文字用電腦唸出來的軟體工具(Japanese text-to-speech system)

<a href="http://open-jtalk.sourceforge.net/" refl="nofollow" target="_blank">官網</a>上並沒有文件說明要如何安裝，所以只得自己研究

<!-- more -->

在 Ubuntu 下最容易的安裝方法是透過 apt-get

```
sudo apt-add-repository ppa:openhri/ppa
sudo aptitude update
sudo aptitude install open-jtalk
```

安裝完後就可以直接使用 open_jtalk 指令

```
$ open_jtalk
The Japanese TTS System "Open JTalk"
Version 1.07 (http://open-jtalk.sourceforge.net/)
Copyright (C) 2008-2013 Nagoya Institute of Technology
All rights reserved.
```

要使用 open_jtalk 還需要聲音檔跟字典檔 (open_jtalk_dic_utf_8-1.09)

字典的部份可以在官網上下載到，至於聲音檔則是可以透過 apt-get 安裝

```
sudo apt-get install hts-voice-nitech-jp-atr503-m001
```

最後會安裝在 /usr/share/hts-voice/ 底下

接下來讓我們實際使用看看

```
echo "電気をつけてみました" | open_jtalk -m /usr/share/hts-voice/nitech-jp-atr503-m001/nitech_jp_atr503_m001.htsvoice -ow output.wav -x open_jtalk_dic_utf_8-1.09
```
上面的指令會輸出 output.wav ，用可以解碼 wav 的軟體就能夠聽到結果。

<audio controls>
  <source src="{{ site.url }}/downloads/open_jtalk_sample/man.wav" type="audio/wav">
  Your browser does not support the audio tag.
</audio>

透過這樣使用的 open_jtalk 是1.07版本，但此時最新的是 1.09，如果有需要也可以自己抓原始碼自行編譯

要注意的是，自行編譯的話需要也需要自行編譯 hts_engine

下面很簡短的說明如何編譯
```
$ cd hts_engine_API-1.10
$ ./configure
$ make
$ cd ..
$ cd open_jtalk-1.09
$ ./configure --with-hts-engine-header-path=hts_engine_API-1.10/include --with-hts-engine-library-path=hts_engine_API-1.10/lib --with-charset=utf-8
$ make
```
不過身為一個宅宅工程師，當然是希望可以聽到女生的聲音

幸好有人製作出了這樣的聲音檔，可以在 MMDAgent 這個計畫中找到一個取名為 mei 的女性的聲音

這邊我們下載 MMDAgent "Sample Script" version 1.6 這個連結，在裡面的 VOICE/mei/ 資料夾中有著許多 htsvoice ，分別代表不同的語氣

這邊展示一個範例

<audio controls>
  <source src="{{ site.url }}/downloads/open_jtalk_sample/mei.wav" type="audio/wav">
  Your browser does not support the audio tag.
</audio>

實際使用過後就會覺得還是女生的聲音比較好