---
layout: post
title: "LD Version Scripts"
date: 2016-11-04 17:49:19 +0800
---

## Android NDK LDFLAGS

libhello.map

```Make
VERS_1.1 {
    global:test;
    local:*;
};
```

Android.mk

```Make
LOCAL_PATH := $(call my-dir)

include $(CLEAR_VARS)
LOCAL_MODULE:=hello

LOCAL_SRC_FILES:=main.cpp

LOCAL_CFLAGS += -Os -fvisibility=hidden

LOCAL_LDLIBS:= -llog

LOCAL_LDFLAGS:= -Wl,--version-script=$(LOCAL_PATH)/libhello.map

include $(BUILD_SHARED_LIBRARY)
```