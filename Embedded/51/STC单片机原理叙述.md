## $\color{deepskyblue} {STC单片机原理概述}$

<br>

### STC 单片机硬件知识

一个完整的嵌入式系统软件主要包括板级支持包（Board Support Package，BSP）、嵌入式实时操作系统（Real-Time OS，RTOS）和应用程序（APP）

8051 单片机语言分为四个不同的层次，包括：微指令控制序列、机器语言、汇编语言和高级语言

<br>

将在本地固化程序的方式称为在系统编程（In System Programming，ISP）；  
而将另一种固化程序的方式称为在应用编程（In Application Programming，IAP）

支持 ISP 方式的单片机，不一定支持 IAP 方式；但是，支持 IAP 方式的单片机，一定支持 ISP 方式

<br>

#### 封装类型

这些是常见的 IC 封装类型:

LQFP: Low profile Quad Flat Package。低高度四边形平面封装。

- 特点:低高度,导线引出下面,可放在 PCB 两侧,面积面积大,导线弹性好。
- 应用:大封装,功耗低的 IC。

PDIP: Plastic Dual Inline Package。塑料双排直列封装。

- 特点:导线引出两边,直线引出,安装简单。
- 应用:逻辑 IC、微控制器、MCU 等。

SOP: Small Outline Package。小轮廓封装。

- 特点:封装小,导线引出下面。
- 应用:小封装且高引脚 IC。

SOJ: Small Outline J-bend Package。小轮廓 J 弯曲封装。

- 特点:封装小,导线引出两边弯曲。
- 应用:小封装且高引脚 IC。

QFN: Quad Flat No-lead package。无引脚四边形平面封装。

- 特点:封装小,导线下引出,引脚下沉,无引脚。
- 应用:高集成度,低功耗 IC。

<br>
