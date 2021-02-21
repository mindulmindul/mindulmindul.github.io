---
layout: default
description: GNU Linker ld
title: tools
date: 2021-01-06
permalink: /tools/ld
---

# GCC链接器 ld

## 1. 简介
1. **ld的功能：** gcc使用ld完成编译的最后一步，将所有生成的archive文件链接起来生成一个可执行程序或库。
2. **强大的控制力：** ld可以读取一个由**AT&T's Link Editor Command Language syntax**扩展的命令集编写的命令文件（.lds），提供对链接过程的明确而又完整的控制。（关于该语法规则的更多信息查看info手册ld条目）
3. **灵活性：** ld使用BFD库来操作二进制文件。（BFD（Binary File Descriptor）是GNU binutils的子项目，BFD把目标文件抽象成一个统一的模型，比如在这个抽象的目标文件模型中，最开始有一个描述整个目标文件总体信息的"文件头"，就跟我们实际的ELF文件一样，文件头后面是一系列的段，每个段都有名字、属性和段的内容，同时还抽象了符号表、定位表、字符串表等类似的概念，使得BFD库的程序只要通过这个抽象的目标文件模型就可以实现操作所有BFD支持的目标文件格式。）
4. **与其他链接器的兼容性：** GNU ld被构造成适用于多种情况的链接器，并与其他链接器尽可能的兼容。

## 2. 使用方法
### -命令行选项的格式
#### 单字符选项
单字符选项，其参数可以直接跟在选项字符后，也可以分开写在一起，如```-l libmath.so```与```-lm```等价，```-oabc```与```-o abc```等价```。

#### 多字符选项
多字符选项，在前边可以用```-```，也可以用```--```，比如```-trace-symbol```和```--trace-symbol```等价，注意如果多字符选项第一个字符为```o```，则必须使用```--```，否则会与```-o```混淆，比如```-omagic```表示输出文件名为```magic```，```--omagic```表示使用```--omagic```选项。


#### 多字符选项的参数
多字符选项的参数可以使用```=```指定，也可以直接跟在选项之后。如，```--trace-symbol foo```和```--trace-symbol=foo```等价。多字符选项的唯一缩写也是可以被接受的。

#### 与GCC配合使用
```gcc```可以调用链接器```ld```来链接二进制文件。这种情况下，如果要给```ld```传递选项和参数，需要使用```-Wl```来指定。如:
```bash
gcc -Wl,--start-group foo.o bar.o -Wl,--end-group
# 给ld传递两个链接选项：--start-group，--end-group
# 而 foo.o 和 bar.o 是传递给 gcc 的
```
否则，```gcc```将静默调用```ld```，导致错误的链接结果。

此外，通过```gcc```给```ld```传递需要参数的选项时，使用多字母或单字母选项的**连接形式**（即选项与参数之间无空白字符）即可。比如：
```bash
gcc foo.o bar.o -Wl,-eENTRY -Wl,-Map=a.map
```
### -输入文件
输入文件可以是.o文件或者其他类型的二进制文件。通过在ld后使用```FILE_NAME```、```-l```、```-R```或者链接脚本（```.lds```）来指定。

如果没有输入文件，ld将会报错。

如果输入文件格式无法识别，该文件将被作为链接脚本文件处理，这相当于默认链接脚本或使用```-T```选项指定的链接脚本的扩充。这个功能的最终用途是动态链接的实现：链接的文件看似是二进制文件或archive，但实际上，这个文件中只有一些自定义的符号值。

输入文件在最终的输出文件中的相对位置取决于指定该文件的选项在所有文件指定选项中的相对位置。如
```bash
ld -lm main.o -lcv2 -o main
```
将指定输入文件在输出文件中的顺序为：```libmath.so main.o libcv2.so```。

## 3. 所有选项
```@file```

从文件读取ld选项。读取的选项被插入到原来的@file选项的位置。如果文件不存在或无法读取，则该选项将按字面意义处理，而不是删除。

文件中的选项用空格分隔。通过将整个选项用单引号或双引号括起来，可以在选项中包含空格字符。任何字符（包括反斜杠）都可以通过在要包含的字符前面加上反斜杠来包含。文件本身可能包含额外的@file选项；任何此类选项都将被递归处理。

```-a keyword```

HP/UX兼容性支持此选项。关键字参数必须是```archive```、```shared```或```default```字符串之一。```-aarchive```在功能上等价于```-Bstatic```，另外两个关键字在功能上等价于```-Bdynamic```。此选项可以使用任意次数。

```--audit AUDITLIB```

将AUDITLIB添加到动态部分的“DT_AUDIT”条目中。不会检查AUDITLIB是否存在，也不会使用库中指定的DT_SONAME。如果多次指定，“DT_AUDIT”将包含要使用的审核接口的冒号分隔列表。如果链接器在搜索共享库时发现具有审核条目的对象，它将在输出文件中添加相应的“DT_DEPAUDIT”条目。此选项仅在支持rtld审计接口的ELF平台上有意义。

       -b input-format
       --format=input-format
           ld may be configured to support more than one kind of object file.  If your ld is configured
           this way, you can use the -b option to specify the binary format for input object files that
           follow this option on the command line.  Even when ld is configured to support alternative
           object formats, you don't usually need to specify this, as ld should be configured to expect as
           a default input format the most usual format on each machine.  input-format is a text string,
           the name of a particular format supported by the BFD libraries.  (You can list the available
           binary formats with objdump -i.)

           You may want to use this option if you are linking files with an unusual binary format.  You can
           also use -b to switch formats explicitly (when linking object files of different formats), by
           including -b input-format before each group of object files in a particular format.

           The default format is taken from the environment variable "GNUTARGET".

           You can also define the input format from a script, using the command "TARGET";

       -c MRI-commandfile
       --mri-script=MRI-commandfile
           For compatibility with linkers produced by MRI, ld accepts script files written in an alternate,
           restricted command language, described in the MRI Compatible Script Files section of GNU ld
           documentation.  Introduce MRI script files with the option -c; use the -T option to run linker
           scripts written in the general-purpose ld scripting language.  If MRI-cmdfile does not exist, ld
           looks for it in the directories specified by any -L options.


