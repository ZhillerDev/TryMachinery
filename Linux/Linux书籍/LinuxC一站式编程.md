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
