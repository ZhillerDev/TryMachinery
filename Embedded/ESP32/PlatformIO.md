### 简介

PlatformIO 是一个依附于 VSCode 上的插件，他能协助我们进行 ESP32 等诸多 IoT 微处理器的开发工作

你可以使用它来代替经常配置出问题的 `arduino` 或者难搞的 `espressif-ide`

后续将会使用它来配合 `Blynk` 来实现云端控制以及对应 WIFI 控制的 App 快速开发

<br>

### 安装

首先你需要安装 vscode https://code.visualstudio.com/

之后点击 vscode 的插件安装界面，搜索 platformio，然后安装它就可以了

<br>

### ESP32 点灯实验

安装完插件，点击 vscode 左侧的 platformio 图标，刚打开时会进行初始化，初始化完毕即可打开主界面

主界面打开，我们选择“create project”

名称随便写  
板子选择 `expressif esp32 device mobile`  
工程存储文件随便选一个你顺眼的

由于我们是第一次创建对应工程，所以之后又是一段冗长的工程初始化时间，所有资源都从外网下载，就算你开了梯子也没卵用只能傻等

<br>
