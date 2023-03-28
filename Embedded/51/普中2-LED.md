### 点亮一个 LED

#### 电阻的表示形式

如电阻 `103`，表示 `10x10^3`，即 `10000 欧姆`

同理，473 表示 `47000 欧姆`

<br>

#### 编写程序

![](./images/pz2/p1.png)

点击上图所示按钮，进入 `options for target1`

在 output 选项卡勾选 `create hex file`  
这样每次 build 后都会自动生成 hex 文件，方便我们直接调试

<br>

![](./images/pz2/p2.png)

在 `source group1` 下新建 `main.c` 文件，作为我们的主入口；  
每个工程都必须存在一个主入口

<br>

#### main.c 代码清单

```c
#include <REGX52.H>

// 主入口
void main(){
	// 1表示高电平，0表示低电平，低电平亮灯
	// 由于有8个LED，故1111 1110表示7个关灯，一个亮灯
	P2=0xFE; // 对应二进制1111 1110
}
```

<br>

### LED 闪烁

#### 生成延迟函数

STC-ISP 自带了一个帮我们生成延迟函数的小工具

按下图所示找到该工具，下面是我们需要注意的几个关键点

1. 系统频率：查看自己开发板中晶振频率，一般会写在上面
2. 定时长度
3. 8051 指令集：选择 STC-Y1 指令集，因为这个刚好对应 STC89C52

![](./images/pz2/p3.png)

<br>

#### main.c

```c
#include <REGX52.H>
#include <INTRINS.H> // 注意哦，这里导入了一个新的头文件

// 生成的延迟函数
void Delay500ms()		//@11.0592MHz
{
	unsigned char i, j, k;

	_nop_();
	i = 4;
	j = 129;
	k = 119;
	do
	{
		do
		{
			while (--k);
		} while (--j);
	} while (--i);
}


void main()
{
	// 在循环中调用延迟函数，实现灯泡闪烁
	while(1)
	{
		P2=0xFE;
		Delay500ms();
		P2=0xFF;
		Delay500ms();
	}
}
```

<br>

### 流水灯
