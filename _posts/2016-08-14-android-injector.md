---
layout: post
title: "Android Injector"
date: 2016-08-14 17:49:19 +0800
description: ""
category: 
tags: []
---

### System call

#### waitpid

<pre>
#include <sys/types.h>
#include <sys/wait.h>
pid_t waitpid(pid_t pid, int* status, int options)
</pre>


高2字节用于表示导致子进程的退出或暂停状态信号值(WTERMSIG)，低2字节表示子进程是退出(0x0)还是暂停(0x7f)状态(WEXITSTATUS)。

如:0xb7f就表示子进程为暂停状态，导致它暂停的信号量为11即sigsegv错误。


> 其中两个宏:

> WEXITSTATUS(*statusPtr):
> 
> if the child process terminates normally, this macro evaluates to the lower 8 bits of the value passed to the exit or _exit function or returned from main.
WTERMSIG(*statusPtr)

> if the child process ends by a signal that was not caught, this macro evaluates to the number of that signal.

**options**

指定了waitpid的额外行动.选项有:

**WNOHANG:**

告诉waitpid不等程序中止立即返回status信息.
正常情况是当主进程对子进程使用了waitpid,主进程就会阻塞直到waitpid返回status信息;如果指定了WNOHANG选项,主进程就不会阻塞了.
如果还没有可用的status信息,waitpid返回0.

**WUNTRACED:**

告诉waitpid，如果子进程进入暂停状态或者已经终止，那么就立即返回status信息,正常情况是紫禁城终止的时候才返回.
如果是被ptrace的子进程，那么即使不提供WUNTRACED参数，也会在子进程进入暂停状态的时候立即返回。
对于使用ptrace_cont运行的子进程，它会在3种情况下进入暂停状态：①下一次系统调用；②子进程退出；③子进程的执行发生错误。