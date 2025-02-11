---
layout: post
title: Build NDK From Source Code
date: 2017-09-02 17:49:19 +0800
---


## Software Require
Operating System:Ubuntu 17.04

## Download Source Code
```shell
cd ~
mkdir ndk-dev
cd ndk-dev
repo init -u htpps://android.googlesource.com/platform/manifest -b ndk-r14
repo sync
```
## Install Build Tools
- bison
- flex
- python-nose
- python-lxml
- bpzip2
- zip
- texinfo
- mingw-w64
## Compile
```shell
ANDROID_NDK_ROOT=/home/tom/ndk-dev/ndk
cd /home/tom/ndk-dev/ndk
python checkbuild.py --system linux
```