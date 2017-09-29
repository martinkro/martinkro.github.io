---
layout: post
title: "Create Android Project From CommandLine"
date: 2015-03-21 17:49:19 +0800
description: "Create Android Project From CommandLine"
category: android
tags: [android]
---

1 创建ANDROID应用程序

help 子命令

	android --help

create project 子命令

	android --help create project
	Options:
  	-n --name          : Project name.
  	-a --activity      : Name of the default Activity that is created. [required]
  	-k --package       : Android package name for the application. [required]
  	-v --gradle-version: Gradle Android plugin version.
  	-t --target        : Target ID of the new project. [required]
  	-g --gradle        : Use gradle template.
  	-p --path          : The new project's directory. [required]

创建工程

	android create project --name gp
	--activity GPActivity
	--package com.tencent.gp
	--target android-8
	--path .\gp

2 编译ANDROID程序

编译R.java

	mkdir gen
	aapt p -f -m -J gen -S res -I %ANDROID_SDK%\platforms\android-8\android.jar -M AndroidManifest.xml


javac 生成 class

	ejavac -encoding UTF-8 -target 1.8 -bootclasspath %ANDROID_SDK%\platforms\android-8\android.jar -d bin src\com\tencent\gp\*.java gen\com\tencent\gp\R.java

aapt 生成资源包文件

	aapt package -f -S res -I %ANDROID_SDK%\platforms\android-8\android.jar  -A assets -M AndroidManifest.xml -F bin\GP.ap_

keytool 生成证书文件

	keytool -genkey -alias HelloWorld.keystore -keyalg RSA -validity 1000 -keystore HelloWorld.keystore -dname "CN=w,OU=w,O=localhost,L=w,ST=w,C=CN" -keypass 123456 -storepass 123456

jarsigner 签名apk

	jarsigner -verbose -keystore HelloWorld.keystore -signedjar gp.apk gp_unsigned.apk HelloWorld.keystore
