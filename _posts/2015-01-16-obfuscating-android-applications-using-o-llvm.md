---
layout: post
title: "使用O-LLVM混淆Android代码"
date: 2015-01-16 17:49:19 +0800
description: "llvm"
category: llvm
tags: [llvm,android]
---

原文地址：http://fuzion24.github.io/android/obfuscation/ndk/llvm/o-llvm/2014/07/27/android-obfuscation-o-llvm-ndk/

## 环境准备 ##

- Mac OSX 10.10
- android-ndk-r10d for mac
- [OSX-NDK-Obfuscator.tar.bz2](https://github.com/Fuzion24/AndroidObfuscation-NDK/blob/master/prebuilt/android-ndk32-r10-darwin-x86_64-obfuscator.tar.bz2?raw=true)


假设Android NDK所在的目录为 $ANDROID_NDK_ROOT。

将OSX-NDK-Obfuscator.tar.bz2解压到目录$ANDROID_NDK_ROOT/toolchains

	tar zxvf OSX-NDK-Obfuscator.tar.bz2 -C $ANDROID_NDK_ROOT/toolchains/
	
在目录$ANDROID_NDK_ROOT/toolchains下增加了2个目录

	cd $ANDROID_NDK_ROOT/toolchains
	ls -l|grep obfuscator
	drwxr-xr-x@ 4 test  staff  136  2  4 13:00 arm-linux-androideabi-clang3.4-obfuscator
	drwxr-xr-x@ 6 test  staff  204  1 13 17:26 obfuscator-llvm-3.4
