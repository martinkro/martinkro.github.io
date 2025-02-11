---
layout: post
title: "在Windows 10 上搭建PHP开发环境"
date: 2016-04-16 17:49:19 +0800
description: "web develope"
category: web
tags: [HTML5]
---

### 1 软件准备
- Visual C++ Redistributable For Visual Studio 2015
- httpd-2.4.17-win64-VC14.zip
- php-5.5.30-Win32-VC11-x64.zip


### 2 安装
- 安装VS2015运行时环境
	- vc_redist.x86.exe
	- vc-redist.x64.exe
- 解压httpd到d:\Apache24
- 解压php到d:\PHP


### 3 配置
- 安装服务
	- httpd.exe -k install -n "HTTPD" -f "d:\Apache24\conf\httpd.conf"
- 启动服务
	- 使用Windows服务管理器启动服务


### 4 测试
1. http://localhost:9999/index.html
2. http://localhost:9999/test.php
