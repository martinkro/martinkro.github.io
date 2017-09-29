---
layout: post
title: "Floating View for Android"
date: 2016-07-28 17:49:19 +0800
description: ""
category: 
tags: []
---

### Introduction

WindowManager.LayoutParams 中有一个属性 type。它定义了窗口的类型。有 3 种主要类型：

- Application windows：取值在 FIRST_APPLICATION_WINDOW(1) 和 LAST_APPLICATION_WINDOW(99) 之间。是通常的、顶层的应用程序窗口。必须将 token 设置成 activity 的 token 。
- Sub-windows：取值在 FIRST_SUB_WINDOW(1000) 和 LAST_SUB_WINDOW(1999) 之间。与顶层窗口相关联，token 必须设置为它所附着的宿主窗口的 token。
- System windows：取值在 FIRST_SYSTEM_WINDOW(2000) 和 LAST_SYSTEM_WINDOW(2999) 之间。用于特定的系统功能。它不能用于应用程序，使用时需要特殊权限。

通常的悬浮窗都是 System windows，也就是通常见到的 TYPE_SYSTEM_ALERT 与 TYPE_SYSTEM_ERROR。
但是我不需要全局，我只需要在应用内显示，于是我打起来第一和第二点的注意。经过查看 type 属性，找到了一个好像还不错的值 TYPE_APPLICATION_ATTACHED_DIALOG。（对话框。类似于 TYPE_APPLICATION_PANEL，绘制类似于顶层窗口，而不是宿主的子窗口。）然后我就将 WindowManager.LayoutParams 的 type 属性改为 TYPE_APPLICATION_ATTACHED_DIALOG。接着问题就来了……
程序异常退出

<pre>
activity.getWindow().getDecorView().getWindowToken();
</pre>

#### WindowManager

在Android应用开发中，其实整个Android的窗口机制是基于一个叫做WindowManager的一个系统服务接口，WindowManager可以添加view到屏幕，也可以从屏幕删除view。它面向的对象一端是屏幕，另一端就是View，其实就连我们常用的Activity和Diolog的底层实现都是通过WindowManager， WindowManager是全局的，整个系统就只用一个Windowmanager服务，我们需要向系统获取服务才能调用它，而它就是显示View的最底层。
