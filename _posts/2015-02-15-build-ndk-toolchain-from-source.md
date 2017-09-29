---
layout: post
title: "从源代码编译Android NDK工具链"
date: 2015-02-15 17:49:19 +0800
description: ""
category: 
tags: []
---

由于工作需要，最近需要做一个代码混淆的工具，功能包括流程混淆、字符串加密、函数名HASH等功能。在Windows平台上有VMP这些加密工具可以使用，在Android和iOS平台上没有比较成熟的工具，本想按照VMP的思路实现一个，最终发现工作量极大，至少需要实现Thumb Thumb-32 ARM x86 arm64 x86-64好几个平台的指令分析，还要非常熟悉ELF MachO文件格式操作，由于没有现成的基础库和自己本身对这些平台不熟悉（相对Windows），放弃在二进制级别实现的想法。

偶然间注意到LLVM这个编译器框架，发现基于这个框架来实现加密功能，非常方便，可以跨平台，极大的减少工作量。不用解析这么多指令集，这么多文件格式。为什么？加密功能主要实现在编译器的中间部分（前端－中间（优化）－后端）。详细原理可以参考LLVM的介绍。

android-ndk-r10d（更早的版本也有）已经提供了clang编译器，将llvm代码用Visual Studio 2013编译，直接替换ndk相关的编译工具，无法运行（MacOSX下可以）。表现时as这个工具无法找到，调试发现是路径配置问题。于是就仔细阅读了NDK的编译脚本，有了这一篇介绍如何从源代码编译NDK工具链的文章。

## 环境要求 ##

- Ubuntu 14 TLS


## build command ##

	export NDK_LOGFILE=/tmp/ndk-build.log
	export NDK=`pwd`/ndk
	
	