---
layout: post
title: "基于数据挖掘的反外挂识别技术"
date: 2015-01-11 17:49:19 +0800
description: ""
category: Data Mining
tags: [Data Mining]
---

### 概要

本文以DNF游戏为例阐述如何通过数据挖掘技术识别自动挂机的玩家。

原始数据：

- training-target.txt:作弊玩家的QQ号
- training.txt:训练数据
- test.txt:测试数据

### 数据预处理

由于原始数据不能只用使用，需要先转换为建模需要的数据格式。

- 从数据属性描述看，前2列数据无用，剔除。
- 将作弊玩家的QQ号关联，增加一列，异常数据对应1，正常数据对应0。

数据处理脚本：data-pre.py

### 建模

数据属性中的UIN在建模中是无用的。

建模工具采用Orange

采用的模型：决策树
