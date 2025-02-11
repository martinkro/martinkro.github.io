---
layout: post
title: "Google Breakpad"
date: 2016-11-07 17:49:19 +0800
---

## Google Breakpad For Android

```shell
$ ./dump_syms ./test > test.sym
$ head -n1 test.sym
$ ./minidump_stackwalk 5ee28168-f798-caba-749b962b-312eaf19.dmp ./symbols
```