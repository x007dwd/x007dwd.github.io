---
layout:     post
title:      "NEON vs SSE"
subtitle:   " \"博学之审问之慎思之明辨之笃行之。\""
date:       2017-3-18 21：19
author:     "Bobin"
tags:
    - SIMD
---
由于代码的需要，我们需要将一段使用SSE2优化的代码改为NEON，以便于在ARM的处理器上使用。
首先介绍一些基础知识，参考[CSDN blog](http://blog.csdn.net/wendox/article/details/53393856)。
# SIMD基础知识
## 简介
Intel的CPU和ARM的CPU都有SIMD指令，可以完成CPU 指令级的并行化。这里边主要涉及CPU的汇编的知识和一些寄存器的知识。在一些耗时的SLAM优化迭代的场合，经常出现这样的指令的优化。SSE是Intel x86架构CPU的SIMD指令的简称，NEON是ARM CPU的SIMD指令的简称。

由于项目的需要，我以前的时候用过一段SSE指令，后来一段时间没有在接触过，最近在玩飞机，我们在DJI M100上加了ARM架构的TK1板，在移植slam的代码的时候，一些SSE的代码需要转换为NEON指令。因此这里做了一些两种SIMD指令的转化和比较。

## 寄存器的基础知识
一般computer中存在内存，内存就像仓库，我们不常用的东西分类放到仓库里边去。等到用的时候就会拿出来放在手边，手边的一些柜子书桌就是CPU中的寄存器。寄存器的位数和指令的位宽是一样的。我们说128位的指令位宽，那么对应的寄存器的位数就是128位，而CPU每次可以计算的数据的宽度最大也是128位。因为我们常用的数据达不到这样的宽度，这样每个指令周期就可以执行多个数据的计算。这就是所谓向量化计算。

## 汇编指令基础
我们的c语言中的加法减法（各种运算 加减乘除 与或非 等等）都有对应的指令，可以很容易转换，但是在赋值等操作中涉及load和store内存的操作，这是c语言中看不见的。
### 浮点计算 vs 整数计算
为什么要分开讲呢？因为在指令集中也是分开的，另外，由于浮点数占4个字节或者8个字节，而整数却可以分别占1,2,4个字节按照应用场合不同使用的不同，因此向量化加速也不同。因此一个指令最多完成4个浮点数计算。而可以完成16个int8_t数据的计算。

### 数据存储和读取
在c中看不到寄存器，数据在寄存器还是内存中，我们不知道，但是在汇编中，就要分的很清楚才行，因为送到运算器的数据一般是要线放到寄存器中去的的。其中涉及到两个指令load 和 store指令，前者将数据从从内存中读取出来放到寄存器里边，后者是将数据放回内存。我们在c语言中看到的各种变量都是存在内存中的。

### 优化技巧
1. 注意指令的顺序，为什么呢，因为CPU是流水线工作的，因此相邻的指令开始的执行的时间并非一个指令执行完毕之后才会开始，但是一旦遇到数据联系，这时候会发生阻塞，如果我们很好的安排指令的顺序，使得数据相关尽量少发生，或者发生的时候上一个指令已经执行完了。因此注意稍微修改指令的执行顺序就会使得代码变快，是不是特别神奇？
2. 适当的循环展开，适当注意仅仅是适当，目的还是减少数据相关，不过展开太多怕被鄙视啊。

## 关于编译
These built-in intrinsics for the ARM Advanced SIMD extension are available when the -mfpu=neon switch is used:

# ARM vs x86
## 头文件
SSE使用的头文件是xmmintrin.h
## 指令特点
SSE的指令特点是前边使用_mm_xx_xx()，而ARM我们要使用纯汇编来完成，办法使用类汇编的C来搞。

## 指令对比

|SSE | NEON |功能|
|----|---|---|
|_mm_set1_ps||Create a vector with all four elements equal to F
|_mm_load_ps||Memory TO register
|_mm_add_ps||加法运算
|_mm_mul_ps||乘法运算
|_mm_store_ps||register to Memory

# refer
1. http://zyddora.github.io/2016/02/28/neon_1/
2. http://zyddora.github.io/2016/03/16/neon_2/
3. https://gcc.gnu.org/onlinedocs/gcc-4.7.4/gcc/ARM-NEON-Intrinsics.html#ARM-NEON-Intrinsics
