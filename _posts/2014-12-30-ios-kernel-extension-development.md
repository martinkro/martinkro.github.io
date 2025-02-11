---
layout: post
title: "iOS Kernel Extension Development"
date: 2014-12-30 17:49:19 +0800
description: ""
category: iOS
tags: [iOS kernel]
---

### 1 MacOSX

1 kernel extension path

/System/Library/Extensions

2 Load kernel extension

(1) copy test.kext to /System/Library/Extensions

(2) kextstat

- kextload, which loads a KEXT into the kernel
- kextunload, which stops a loaded KEXT and unloads it from the kernel
- kextutil, which is a developer-oriented utility for loading KEXTs into the kernel and can provide diagnostic information detailing why a kernel extension failed to load and can produce symbols that are useful when debugging an active kernel extension
- kextstat, which displays a list of all KEXTs loaded into the kernel

load kext

	kextload HelloWorld.kext

unload kext

	kextunload HelloWorld.kext
	
list kext

	kextstat
	
	sudo chown -R root:wheel HelloWorld.kext
	
### Create Kernel Extension by Xcode

1 