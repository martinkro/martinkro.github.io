---
layout: post
title: "Android ERROR:Read Only FileSystem"
date: 2016-08-05 17:49:19 +0800
description: ""
category: 
tags: []
---

### mount

<pre>
mount -o rw,remount /system
mount -o ro,remount /system
</pre>

根目录和/sbin目录位于一个RAMDisk中，该文件系统是一个只读的，位于内存的文件系统，从设备磁盘的boot分区加载而来。运行时发生的变化不会被物理地写回去。
