---
layout: post
title: "Build Android NDK Toolchain From Source Code"
date: 2015-10-12 17:49:19 +0800
description: "Build Android NDK Toolchain"
category: android
tags: []
---

I. Environment
------

	mkdir ndkdev
	cd ndkdev
	
	git clone /root/aosp/mirror/platform/development.git
	git clone /root/aosp/mirror/platform/ndk.git
	git clone /root/aosp/mirror/platform/prebuilt.git
	
	mkdir toolchain
	cd toolchain
	repo init -u /root/aosp/mirror/toolchain/manifest.git
	repo sync
	
	mkdir prebuilts
	cd prebuilts
	mkdir gcc
	mkdir tools
	
Build host toolchains
---------------------

