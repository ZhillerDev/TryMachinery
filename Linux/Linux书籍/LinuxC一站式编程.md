### 程序基本概念

> 使用 ubuntu22.0 作为演示环境（vmware 虚拟机搭设）

<br>

#### 配置开发环境

> 配置完基础开发环境后，可以直接下载一个 vscode 作为初始 LDE 使用（后续逐渐熟悉 Vim 后再转，刚开始不要一步登天）

开发需要编译器、头文件以及对应的标准库和文档  
必须下载 `gcc gdb make`

● gcc: The GNU C compiLer  
● Libc6-dev: GNU C Library: DeveLopment Libraries and Header FiLes  
● manpages-dev: ManuaL pages about using GNU/Linux for deveLopment  
● manpages-posix-dev: ManuaL pages about using a POSLX system for deveLopment  
● binutiLs: The GNU assembLer, linker and binary utiLities  
● gdb: The GNU Debugger  
● make: The GNU version of the "make" utiLity

<br>

#### 第一个程序

创建文件夹，新建文件 `main.c`，并使用 `gedit` 编辑它

```sh
mkdir linuxc
cd linuxc
touch main.c
gedit main.c
```

为 `main.c` 添加简单的代码

```c
#include <stdio.h>

int main(void){
	printf("%s\n","helloworld");
	return 0;
}
```

使用 gcc 编译得到默认文件输出 a.out，然后直接调用 a.out 文件即可执行！

```sh
gcc main.c
./a.out
```

<br>

gcc 编译特定名称 `gcc xxx.c -o main.out`  
gcc 编译回显所有警告 `gcc -Wall xxx.c`

<br>

#### C 复习

复习个鬼，自己找 `cprimerplus` 和 c++`primer` 读去

下一节直接上手 gdb

<br>

### gdb

#### 单步执行与跟踪

首先编写一份简单的 c 文件：main.c

```c
#include <stdio.h>

int add_range(int Low, int high)
{
    int i, sum;
    for (i = Low; i <= high; i++)
        sum = sum + i;
    return sum;
}
int main(void)
{
    int result[100];
    result[0] = add_range(1, 10);
    result[1] = add_range(1, 100);
    printf("result[0]=%d\nresult[1]=%d\n", result[0], result[1]);
    return 0;
}
```

如果我们想要 gdb 调试该代码，gcc 编译时必须添加-g 参数，表示将源码（的引用）插入到编译后的文件内  
`gcc -g main.c -o main`

然后使用 gdb 运行编译后文件  
`gdb main`

> -g 参数并不是直接把源码拼到编译后文件内，我们在使用 gdb 调试时当前文件夹下依然需要源码文件存在，单纯地编译后文件是无法执行的！

<br>

查看源代码，一次 10 行 `list 1`  
按回车可以快速执行上一条命令

列出指定函数 `l [函数名]`

退去 gdb（会询问你一次） `quit`  
强制退出 `exit`

<br>

开始执行程序 `start`  
下一步 `n`|`next`  
深层模式（可进入执行的函数内部） `s`|`step`

查看函数调用帧栈 `bt`  
查看函数局部变量 `i`  
选择指定栈帧 `f [栈帧号]`

打印变量值 `p [变量名]`  
运行直到当前函数结束 `finish`

<br>

#### 断点

准备测试代码

```c
#include <stdio.h>

int main(void)
{
    int sum = 0, i = 0;
    char input[5];

    while (1) {
        scanf("%s", input);
        for (i = 0; input[i] != '\0'; i++)
            sum = sum＊10 + input[i] - '0';
        printf("input=%d\n", sum);
    }
    return 0;
}
```

标注变量（被标注的变量会在每次运行时都显示一次） `display [变量名]`  
取消标注变量 `undisplay [变量名]`  
清除所有标注 `clear`

为指定行打断点 `b [行号]`
暂时关闭断点 `disable b [断点号]`  
删除所有断点 `delete breakpoints`  
持续运行代码直到遇到断点后停止 `c`  
从头开始运行 `r`  
查看所有断点详情 `i breakpoints`

断点还可以设置条件，当满足该条件时才激活断点  
`break 9 if sum!=0`

<br>

#### 观察点

```c
#include <stdio.h>

int main(void)
{
    int sum = 0, i = 0;
    char input[5];

    while (1) {
        sum = 0;
        scanf("%s", input);
        for (i = 0; input[i] != '\0'; i++)
            sum = sum＊10 + input[i] - '0';
        printf("input=%d\n", sum);
    }
    return 0;
}
```

设置观察点 `watch [欲观察的变量名]`

<br>

#### 段错误

```c
#include <stdio.h>

int main(void)
{
    int man = 0;
    scanf("%d", man);
    return 0;
}
```

很明显，上面这一段代码汇总的 scanf 中对应的 man 前面缺少了一个&符号  
此时使用 gdb 进行 run 调试，会直接在对应行进行报错；

对于部分错误，可能不会在逐行运行的时候直接抛出，可能会在 return 执行的时候才抛出，这个要注意！

<br>

### 数据类型分析

#### 浮点型

浮点数在不同平台上实现不同  
有的处理器有浮点运算单元（Floating Point Unit，FPU），称为硬浮点（Hard-float）实现  
有的处理器没有浮点运算单元，只能做整数运算，需要用整数运算来模拟浮点运算，称为软浮点（Soft-float）实现

在 x86 平台上，大多数编译器实现的 long double 型是 80 位  
gcc 实现的 long double 型是 12 字节（96 位）

<br>

#### 类型转换

C 中有转换级别机制，以下类型转换级别（Rank）越来越高：char、short、int、long、long long

<br>

### 运算符分析

#### 移位问题

避免不同类型数值赋值操作；  
因为 C 语言中不存在 8 位整数的二进制位运算，所有位运算执行之前都被提升为 int 类型

在一定的取值范围内  
左移 1 位=乘以 2  
右移 1 位=除以 2

<br>

#### 异或运算特性

一个数和自己做异或的结果是 0

和 0 做异或保持原值不变，和 1 做异或得到原值的相反值

可用于奇偶校验：例如 a1 ^ a2 ^ a3 ^ … ^ an 的结果是 1，则表示 a1、a2、a3…an 之中 1 的个数为奇数个，否则为偶数个

`x ^ x ^ y == y`

<br>

### 计算机体系结构

CPU 的核心功能包括这部分

1. 寄存器：特殊寄存器、通用寄存器
2. 程序计数器 PC：特殊寄存器，保存着 CPU 取下一条指令的地址
3. 指令译码器：负责解释从 CPU 中取出的指令对应段的含义（比如内存地址、寄存器编号等等）
4. 算术逻辑单元 ALU：译码器转换的运算指令给 ALU 进行运算
5. 地址与数据总线 Bus

<br>

内存映射 LO 定义  
无论是在 CPU 外部接总线的设备还是在 CPU 内部接总线的设备都有各自的地址范围，都可以像访问内存一样访问

<br>

#### MMU 内存管理单元

MMU 工作原理  
CPU 发出获取内存地址请求，此时传递`虚拟地址 VA` 给 MMU，MMU 将 VA 转换成`物理地址 PA` 给 CPU 外部的指定芯片引脚

如果 MMU 不工作，那么 CPU 发出的内存地址请求均为 PA，直接对应外部芯片引脚

MMU 管理一张虚拟页表，一一对应物理内存上的物理页表内容  
每次 CPU 访问内存时，都会触发 MMU 的查表和地址转换操作

<br>

MMU 存在的意义

- 内存保护机制。MMU 可以拦截不同用户组的请求，根据其拥有的权限选择是否拦截（不转换地址）或者放行（转换地址）
- 有效避免内核空间和用户空间地址污染，使二者独立

<br>

### 汇编基本

#### 最简汇编程序

> 汇编程序根据编译器的不同，使用 asm 或者 s 作为后缀；  
> 首先要将汇编源文件使用汇编器翻译成机器指令，生成后缀为 o 的文件，然后再通过链接器编译成可执行文件

```s
.section .data
.section .text
.globl _start

_start:
movl $1,%eax #this is the Linux kerneL command

movl $4,%ebx #this is the status number we wiLL

int $0x80   #this wakes up the kerneL to run
```

`#` 汇编中表示单行注释

汇编程序中以.开头的名称并不是指令的助记符，不会被翻译成机器指令，而是给汇编器一些特殊指示，称为`汇编指示（Assembler Directive）`或`伪操作（Pseudo-operation）`

`.section .text` section 表示开始划分段的标志，text 表示后续的代码都属于 text 段

`.globl` 可理解为设置全局变量

`_start` 汇编程序入口点，必须被设置为全局变量  
`_start:` 在这里开始写主入口程序

<br>

`movl $1,%eax`  
movl 其实是 mov+l 的结合，l 表示该变量类型为 long  
$1 表示立即数1（$加任意数字都可以表示一个立即数）  
%eax 表示寄存器 eax（所有寄存器都必须加%）

不难得出移位的格式为 `movl [立即数],[欲保存到的寄存器]`

`int $0x80` 软中断指令，可使程序故意产生一个异常导致程序终止运行；可以将其视为程序出口点

<br>

#### 汇编语法分异

x86 汇编存在两种主流语法：

1. AT&T 派：数据传送指令 mov 这样写 `movl $1,%eax`
2. Lntel 派：数据传输指令这样写 `mov eax,edx`（寄存器不加%且存取位置互换）
3. UNLX 平台一般采用 AT&T 语法

<br>

#### x86 寄存器

x86 通用寄存器：`eax、ebx、ecx、edx、edi、esi`

某些特殊场景下，他们会变得不那么“通用”，此时寄存器会有一个或者多个限制  
（比如进行除法运算时）

x86 特殊寄存器：`ebp、esp、eip、efLags`  
efLags 保存着计算过程中产生的标志位  
ebp 和 esp 用于维护函数调用的栈帧

<br>

#### 求最值汇编

```s
# 定义数据存储段data
.section .data
# 类似于数组名
data_items:
# 定义数组类型，.long表示32位，.byte表示8位
.long 3,67,34,222,45,75,54,34,44,33,22,11,66,0

# 主程序段text
.section .text
# 程序入口点与全局变量
.globl _start
_start:
movl $0,%edi    #move 0 into the index register
movl data_items(,%edi,4), %eax # Load the first byte of data
movl %eax,%ebx   #since this is the first item,%eax is

# 循环开始，开头定义一个start_loop
start_loop:
cmpl $0,%eax  # 比较寄存器eax是否等于0，如果为0表示已到末尾，需要跳出循环
je loop_exit  # je即比较，如果上方代码相等，那么跳转到对应标志位

incl %edi   # edi寄存器移到下一位（即加载下一个数据）
movl data_items(,%edi,4), %eax
cmpl %ebx,%eax
jle start_loop

movl %eax,%ebx
jmp start_loop

# 循环结束，结尾定义一个loop_exit
loop_exit:

movl$1,%eax
int $0x80
```
