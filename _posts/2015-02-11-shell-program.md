---
layout: post
title: "SHELL SCRIPTS 编程"
date: 2015-02-11 17:49:19 +0800
description: ""
category: 
tags: []
---

## 1 变量 ##
### 1.1 特殊变量 ###

$0：当前脚本的文件名

$num：num为从1开始的数字，$1是第一个参数，$2是第二个参数，${10}是第十个参数

$#：传入脚本的参数的个数

$*：所有的位置参数(作为单个字符串) 

$@：所有的位置参数(每个都作为独立的字符串)。

$?：当前shell进程中，上一个命令的返回值，如果上一个命令成功执行则$?的值为0，否则为其他非零值，常用做if语句条件

$$：当前shell进程的pid

$!：后台运行的最后一个进程的pid

$-：显示shell使用的当前选项

$_：之前命令的最后一个参数