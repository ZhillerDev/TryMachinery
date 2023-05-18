### 第一章 设备驱动概述与开发环境构建

<br>

#### 无操作系统设备驱动

这是一个简易的单任务架构，他不需要完整的操作系统进行执行

```c
int main(int argc, char* argv[])
{
  while (1)
  {
    if (serialInt == 1)
    /* 有串口中断 */
    {
      ProcessSerialInt();   /* 处理串口中断 */
      serialInt = 0;        /* 中断标志变量清0 */
    }
    if (keyInt == 1)
    /* 有按键中断 */
    {
      ProcessKeyInt();      /* 处理按键中断 */
      keyInt = 0;           /* 中断标志变量清0 */
    }
    status = CheckXXX();
    switch (status)
    {
      ...
    }
    ...
  }
}
```

<br>

无操作系统情况下，必须要有设备驱动  
每一种设备驱动都会定义为一个软件模块，包含.h 文件和.c 文件，前者定义该设备驱动的数据结构并声明外部函数，后者进行驱动的具体实现

如下为无操作系统串口驱动实现

```c
/**********************
 *serial.h文件
 **********************/
extern void SerialInit(void);
extern void SerialSend(const char buf*,int count);
extern void SerialRecv(char buf*,int count);
/**********************
 *serial.c文件
 **********************/
/* 初始化串口 */
void SerialInit(void)
{
 ...
}
/* 串口发送 */
void SerialSend(const char buf*,int count)
{
 ...
}
/* 串口接收 */
void SerialRecv(char buf*,int count)
{
 ...
}
/* 串口中断处理函数 */
void SerialIsr(void)
{
 ...
 serialInt = 1;
}
```

<br>
