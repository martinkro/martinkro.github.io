---
layout: post
title: "Android Hook LD_PRELOAD"
date: 2016-08-05 17:49:19 +0800
description: ""
category: 
tags: []
---

### Command Line Program

<pre>
export LD_PRELOAD=/data/local/tmp/test.so
./test
</pre>

### UI Program

<pre>
setprop wrap.com.test.game "LD_PRELOAD=/data/local/tmp/test.so"
am start -n com.test.game/com.test.game.Main
</pre>