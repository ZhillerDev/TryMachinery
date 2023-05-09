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

<br>

#### 变量

变量名加等号即可赋值

使用美刀符号输出变量值

```sh
#! /bin/bash

value="helloworld"

echo $value
```

<br>

终端模式下，可以使用以下三个命令获取对应作用域内的变量  
`set` 命令输出所有的变量，包括全局变量和局部变量；  
`env` 命令只显示全局变量；  
`declare` 命令输出所有的变量、函数、整数和已经导出的变量

<br>

常见的系统环境变量  
`$HOME`：用户登录时进入的目录。  
`$UID`：当前用户的 UID（用户标识），相当于 id -u。  
`$PWD`：当前工作目录的绝对路径名。  
`$SHELL`：当前 SHELL。  
`$USER`：当前用户。

<br>

#### 引号输出

`a=123` 不加引号直接赋值，值被解析后输出

`a='123'` 单引号，不作任何解析，有什么就输出什么

`a="123"` 双引号，引号里的变量及命令会经过解析后再输出内容

<br>
