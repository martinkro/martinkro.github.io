---
layout: post
title: grep:查找含有某字符串的所有文件
date:   2017-10-15 15:35:19 +0800
categories: "blog"
---

```shell
grep -rn "hello,world" *
```

- *:表示当前目录所有文件
- -r 是递归查找
- -n 是显示行号
- -R 查找所有文件包含子目录
- -i 忽略大小写



