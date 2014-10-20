---
layout: post
title: "Ibus bug in Ubuntu"
description: ""
category: 
tags: []
---
{% include JB/setup %}

In ibus, when typing in "pinyin" the Chinese characters produced do not correspond to what was typed in pinyin. For example, typing "jing" produces "ji neng" instead of desired words.

In my case, the way to fix this is to restart ibus-daemon by 

```
ibus-daemon -drx
```

But in some cases this cannot solve the problem. For more infomation, one can refer to [this web page](https://bugs.launchpad.net/ubuntu/+source/ibus-pinyin/+bug/1298700).
