### 前言

目前被广泛使用的环境配置有如下几种

1. arduino 使用 ESP32 插件辅助开发
2. espressif-ide 开发
3. vscode+插件开发

<br>

然鹅，以上的几种方式不仅需要冗余复杂且极易报错的配置，还占用宝贵的硬盘空间，不推荐使用

建议直接使用 micropython 进行 esp32 开发，环境配置简单，空间占用少！

<br>

环境配置前注意事项：在没有安装 ESP32usb 驱动之前，不要一直将板子接到电脑上！或许是电脑认为你在充电而不是传输数据，故此时 MCU 会一直发热，严重的可能直接烧坏报废，此时最好断开连接！

<br>

### MicroPython 版环境配置

> 参考教程 https://doc.itprojects.cn/0006.zhishi.esp32/02.doc/index.html#/README

<br>

#### 下载文件

Thonny（MicroPython 开发环境）：https://thonny.org/

MicroPython 针对 ESP32 的支持包（选择 bin 后缀的文件下载）：https://micropython.org/download/esp32/

ESP32usb 驱动：https://doc.itprojects.cn/0006.zhishi.esp32/01.download/esp32usbDriver.zip

<br>

#### 开始配置

由于作者太懒，建议直接看这一小节顶部给出的教程地址，内容已经很详细了，大家看看就学会了很简单的！！！

<br>
