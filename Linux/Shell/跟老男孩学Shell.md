### Shell 初步入门

<br>

#### Shell 分类

对于 Unix/Linux 两种系统，shell 主要由以下两种类型

`Bourne shell` 其下还包括子分支 Bourne shell（sh）、Korn shell（ksh）、Bourne Again Shell（bash）三种类型

`C shell` 又包括 csh、tcsh 两种类型

> 目前主要留学的是 csh 以及 bash

<br>

#### 幻数

任意位置创建一个 sh 文件 `s1.sh`

写入以下代码

```sh
#! /bin/bash
echo tom
```

`#!` 叫做幻数，在其后面指出该文件使用的 shell 解释器  
（对于大多数 linux 系统，目前都会默认使用 bash，但这一行还是不可以省略）

保存该文件，同目录下，使用 bash 指令运行，发现输出了 tom  
运行代码 `bash s1.sh`

<br>

这是书中给出的常用 sh 开头写法

```sh
#! /bin/sh
#! /bin/bash
#! /usr/bin/awk
#! /bin/sed
#! /usr/bin/tcl
#! /usr/bin/expect      #＜==expect解决交互式的语言开头解释器。
#! /usr/bin/perl        #＜==perl语言解释器。
#! /usr/bin/env python  #＜==python语言解释器。
```

<br>

#### 注释

非常简单，使用一个 `#` 即可

<br>

### Shell 核心与实践
