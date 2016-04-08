---
layout: post
title: "How to set android system date by adb command?"
date: 2016-04-07 14:51:32 +0800
comments: true
categories: android
---

Default SET format is "MMDDhhmm[[CC]YY][.ss]", that's (2 digits each)
month, day, hour (0-23), and minute. Optionally century, year, and second.

## Example

{% codeblock lang:bash %}
date -u 040714441916.00

# Explain:
# 04 : 四月
# 07: 七日
# 14: 下午兩點
# 44: 四十四分
# 19: 20世紀 (0-based)
# 16: 16年
# 00: 0 秒

{% endcodeblock %}