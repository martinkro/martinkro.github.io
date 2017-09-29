---
layout: post
title: "Xposed Module Development"
date: 2016-08-06 17:49:19 +0800
description: ""
category: 
tags: []
---

### Introduction

Xposed是GitHUB上rovo89设计的一个针对Android平台的动态劫持项目，通过替换/system/bin/app_process程序控制zygote进程，使得app_process在启动过程中会加载XposedBridge.jar这个jar包，从而完成对系统应用的劫持。

因为Xposed工作原理是在/system/bin目录下替换文件，在install的时候需要root权限，但是运行时不需要root权限。

Xposed在线资源可以参照 : http://forum.xda-developers.com/showthread.php?t=1574401

XposedBridge会在XposedInstaller的目录下生成log文件，该log文件的路径为：/data/data/de.robv.android.xposed.installer/log/debug.log

