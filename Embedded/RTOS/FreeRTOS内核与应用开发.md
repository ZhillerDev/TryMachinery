### 新建 FreeRTOS 工程

<br>

#### axf 报错

keli 默认使用 object 下的 axf 文件，他默认会报错，我们需要切换为以下路径的对应文件  
`.\RTE\Device\ARMCM3\ARMCM3_ac5.sct`

如下图，点击 keli 菜单上的魔法棒图标，进入 linker 选项卡，取消勾选 `use memory layout from target dialog`，之后在当前项目文件夹下找到下图所示的 axf 文件，然后点击确认

之后直接 rebuild 重新编译，即可消除该错误！！！

![](./img/freertos/ft1.png)

<br>
