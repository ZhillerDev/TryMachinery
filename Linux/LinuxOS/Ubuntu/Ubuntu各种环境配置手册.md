## IDE

### QT

#### 安装配置

首先确保 linux 系统存在 g++  
如果没有安装的话，现在安装一个：`apt install g++`

然后前往 QT 官网下载啊 5.12 版本，因为可以离线安装，更方便  
[官方下载地址](https://download.qt.io/archive/qt/5.12/5.12.12/)

选择下载 `qt-opensource-linux-x64-5.12.12.run`

<br>

下载完毕后，使用 xftp 把文件上传到 linux 里面去

打开文件所在文件夹，在该文件夹下打开命令行

执行如下代码，运行安装程序（由于安装前需要登陆 QT 账户，所以你还是需要短暂的连一下网）

```sh
# 首先获取root权限
su

# 常用套路，更改文件权限
chmod 777 qt-opensource-linux-x64-5.12.12.run

# 开始执行文件
sudo ./qt-opensource-linux-x64-5.12.12.run
```

之后就按照指示一路安装下去即可！

<br>

#### 执行

找到我们的 QT 安装目录，我这边的路径如下，QT 为我按照的根目录  
`QT/Tools/QtCreator`

在该目录下找到 `qtcreator.sh`，直接运行 `sh qtcreator.sh` 即可启动 QT  
如果没有任何报错，那么下面的你就不需要看了

<br>

#### 常见报错

```
qt.qpa.plugin: Could not load the Qt platform plugin “xcb“ in ““ even though it was found.
```

此时先打开终端，添加环境变量：`export QT_DEBUG_PLUGINS=1`

紧接着安装这个依赖：`sudo apt-get install libxcb-xinerama0`

然后就可以解决该问题，直接运行 QT 即可成功打开

<br>

### Vscode

<br>

#### 配置 CPP 开发环境

> vscode 安装过程不示范，大体流程就是 firefox 找到 vscode 官网，根据自己的 linux 发行版（debian 还是 redhat）来选择适合的压缩包，然后安装

首先安装三大插件：

- c/c++ extension pack
- c/c++ compile run
- c/c++ runner

<br>

然后记得安装 linux 下必不可少的 gcc 和 g++（ubuntu 默认自带 gcc，你不需要再次安装）
`apt install g++`

之后创建一个简单的 cpp 文件

```cpp
#include <bits/stdc++.h>

using namespace std;

int main(){
    cout<<"fuck"<<endl;
    return;
}
```

然后按照 compile run 插件简述所说，直接按下 f6 键即可自动执行编译+执行的过程

编译完毕的可执行文件存放在 cpp 文件目录下的 output 文件夹内

<br>

## 开发语言与支持

<br>

### pycharm

推荐直接使用 pycharm 社区版，免费

前往官网下载 tar.gz 后缀的压缩包，不管你用的是 debian 还是 redhat 都一样
[点击下载](https://www.jetbrains.com/pycharm/download/other.html)

<br>

下载完毕后，使用 xftp 连接到虚拟机上，将压缩包发送到 `home/username/downloads/` 文件夹下  
或者任意选择一个你喜欢的文件夹

在压缩包所造目录下打开命令行（右键点击空白处，弹出窗口选择运行命令行即可）

解压：`tar -zxvf xxx.tar.gz`

<br>

之后进入 bin 文件夹，连续执行以下两步

```sh
cd xxx # xxx表示解压出来的文件夹名字
cd bin # 进入bin文件夹
```

之后直接 sh 运行即可打开我们的 pycharm 了！

`sh pycharm.sh`

<br>

### docker

<br>

### cmake
