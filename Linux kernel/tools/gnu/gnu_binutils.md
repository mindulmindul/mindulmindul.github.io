---
layout: default
description: GNU Binutils
title: tools
date: 2021-01-06
permalink: /tools/binutils
---


# GNU Binutils

GNU Binutils包含三大部件：

1. [ld](/tools/ld)      - GNU 链接器
2. [as](/tools/as)      - GNU 汇编器
3. [gold](/tools/gold)  - 更新更快的，只支持ELF的链接器

此外还包括以下小部件：

部件         |  功能
-:          |   :-
gprof       |	性能分析工具程序
addr2line   |   从目标文件的虚拟地址获取文件的行号或符号
ar          |   可以对静态库做创建、修改和取出的操作。
c++filt     |   解码 C++ 的符号
dlltool     |   创建Windows 动态库
nlmconv     |   可以转换成NetWare Loadable Module目标文件格式
nm          |   显示目标文件内的符号
[objcopy](/tools/binutils/objcopy)     |   复制目标文件，过程中可以修改
objdump     |   显示目标文件的相关信息，亦可反汇编
ranlib 	    |   产生静态库的索引
readelf     |   显示ELF文件的内容
size        |   列出总体和section的大小
strings     |   列出任何二进制档内的可显示字符串
strip       |   从目标文件中移除符号
windmc      |   产生Windows消息资源
windres     |   Windows 资源档编译器