---
layout: post
title: Android NDK Inline Assembly
date: 2016-12-02 17:49:19 +0800
---

### 格式
asm volatile("asm code":output:input:changed);

- asm 内嵌汇编关键字
- volatile 告诉编译器不要优化内嵌汇编
- output:ASM --> C

__asm__ __volatile__(
    "asm code"
    :"constraint"(variable)
);

-- r:使用任何可用的通用寄存器
-- m:使用变量的内存地址
-- +:可读可写
-- =:只写
-- &:该输出操作数不能使用输入部分使用过的寄存器。只能+&或=&方式使用

- input: C --> ASM

__asm__ __volatile__(
    "asm code"
    :
    :"constraint"(variable/immediate)
);

-- r:使用任何可用的通用寄存器（变量和立即数都可使用）
-- m:使用变量的内存地址（不能用立即数）
-- i:使用立即数（不能使用变量）

- changed:告诉编译器你修改过的寄存器，编译器会自动把保存这些寄存器值的指令加在内嵌汇编之前，再把恢复寄存器值的指令加在内嵌汇编之后。
