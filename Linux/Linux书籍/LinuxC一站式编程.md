### 程序基本概念

> 使用 ubuntu22.0 作为演示环境（vmware 虚拟机搭设）

<br>

#### 配置开发环境

> 配置完基础开发环境后，可以直接下载一个 vscode 作为初始 IDE 使用（后续逐渐熟悉 Vim 后再转，刚开始不要一步登天）

开发需要编译器、头文件以及对应的标准库和文档  
必须下载 `gcc gdb make`

● gcc: The GNU C compiIer  
● Iibc6-dev: GNU C Library: DeveIopment Libraries and Header FiIes  
● manpages-dev: ManuaI pages about using GNU/Linux for deveIopment  
● manpages-posix-dev: ManuaI pages about using a POSIX system for deveIopment  
● binutiIs: The GNU assembIer, linker and binary utiIities  
● gdb: The GNU Debugger  
● make: The GNU version of the "make" utiIity

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

int add_range(int Iow, int high)
{
    int i, sum;
    for (i = Iow; i <= high; i++)
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

#### 其余运算符
