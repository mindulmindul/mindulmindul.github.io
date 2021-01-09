---
layout: default
description: GNU Linker ld
title: tools
date: 2021-01-06
permalink: /tools/ld
---

# GCC链接器 ld

## 简介
1. **ld的功能：** gcc使用ld完成编译的最后一步，将所有生成的archive文件链接起来生成一个可执行程序或库。
2. **强大的控制力：** ld可以读取一个由**AT&T's Link Editor Command Language syntax**扩展的命令集编写的命令文件（.lds），提供对链接过程的明确而又完整的支持。（关于该语法规则的更多信息查看info手册ld条目）
3. **灵活性：** ld使用BFD库来操作二进制文件。（BFD（Binary File Descriptor）是GNU binutils的子项目，BFD把目标文件抽象成一个统一的模型，比如在这个抽象的目标文件模型中，最开始有一个描述整个目标文件总体信息的"文件头"，就跟我们实际的ELF文件一样，文件头后面是一系列的段，每个段都有名字、属性和段的内容，同时还抽象了符号表、定位表、字符串表等类似的概念，使得BFD库的程序只要通过这个抽象的目标文件模型就可以实现操作所有BFD支持的目标文件格式。）
4. **与其他链接器的兼容性：** GNU ld被构造成适用于多种情况的链接器，并与其他链接器尽可能的兼容。


   
    