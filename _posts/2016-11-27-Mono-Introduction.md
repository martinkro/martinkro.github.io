---
layout: post
title: Mono Introduction
date: 2016-11-27 17:49:19 +0800
---

### Source Code Structure

- mcs :Mono实现的基于Ecma标准的C#编译器
- class :CLI的C#级的实现。类似于Android中的Java层，应用程序看到的是这一层提供的接口。这一层是平台无关的。
- ilasm :反汇编器，将Native code反汇编成bytecode。
- mono/mini :JIT编译器，将bytecode编译成native code。
- mono/metadata :Mono的runtime，CLI的Native级的实现。
- io-layer :与操作系统相关的接口实现，像socket，thread，mutex这些。
- libgc :GC实现的一部分。

### GC

### Managed Code and Non-managed Code

- Internal Call(icall)
- P/Invoke

### Thread Pool

### References
1. [Mono Introduction](http://campolake.github.io/2015/01/27/mono_source_code_analyze/)
2. [Working With SGen](http://www.mono-project.com/Working_With_SGen)
3. [Garbage Collection](http://msdn.microsoft.com/en-us/library/vstudio/0xy59wtx%28v=vs.100%29.aspx)
4. [Interop with Native Libraries](http://www.mono-project.com/DllImport)
5. [Generational GC](http://www.mono-project.com/Generational_GC#Fixed_Heap)
6. [Embedding Mono](http://www.mono-project.com/Embedding_Mono)
7. [Mostly Software](http://schani.wordpress.com/tag/mono/)
8. [Implementing a Dispose Method](http://msdn.microsoft.com/en-us/library/fs2xkftw.aspx)
9. [Implementing Finalize and Dispose to Clean Up Unmanaged Resources]( http://msdn.microsoft.com/en-us/library/vstudio/b1yfkh5e%28v=vs.100%29.aspx)
10.[OpCodeEmulation](https://monoruntime.wordpress.com/tag/icall/)
11.[Debugging](http://www.mono-project.com/Debugging)
12.[Dtrace](http://www.mono-project.com/SGen_DTrace)
13.[Marshalling In Runtime](https://monoruntime.wordpress.com/tag/runtime-invoke-wrapper/)
14.[Performance Tips](http://www.mono-project.com/Performance_Tips)
