---
layout: post
title: "How to use repo"
date: 2016-06-16 17:49:19 +0800
description: "how to use repo"
category: repo android git
tags: []
---

# repo init #

## First phases ##

Repo 的 bootstrap 脚本只支持两个命令 help 和 init，而 init 也只完成 repo 版本库克隆（即安装 repo 完整工具），之后就转移控制权。 在 Repo bootstrap 执行 init 可以提供很多参数，但实际上第一阶段初始化，只用到两个参数（而且都有默认值）

* 参数：--repo-url=URL repo 工具本身的 git 库地址。缺省为：git://android.git.kernel.org/tools/repo.git
* 参数：--repo-branch=REVISION 使用 repo 的版本库，即 repo git 库的分支或者里程碑名称。缺省为 stable

## Second phases ##
<pre>
repo init -u git://android.git.kernel.org/platform/manifest.git
</pre>

* 如果不提供 -b REVISION 或者 --manifest-branch=REVISION参数，则检出 manifest Git 库的 master 分支
* 如果不提供 -m NAME.xml 或者 --manifest-name=NAME.xml 参数，则使用缺省值 default.xml
* 如果提供 --mirror 参数，则后续同步操作会有相应的体现

# Command #
* repo branch
* repo status
* repo upload

<pre>
repo init -u ~/aosp/mirror/platform/manifest.git -b android-4.0.1_r1
</pre>

查看android各版本分支

<pre>
git --git-dir .repo/manifests/.git/ branch -a
</pre>

