## IDE

### GIT

#### 配置 GITHUB

安装 GIT：`sudo apt-get install git`  
安装 SSH：`sudo apt-get install openssh-server`

<br>

配置你注册 github 时的用户信息（如果你想要在后续同时配置 github 和 gitee 仓库，那么请保证下面填写的信息和 gitee 中的账户信息完全一致！）

终端输入：

```sh
git config --global user.email "你注册github的邮箱"
git config --global user.name "你注册github的用户名"
```

<br>

首先来到家目录，也就是你打开终端是最初的所在路径  
新建一个文件夹：`mkdir .ssh` （新建之前你可以使用`cd .ssh`试探一下该文件夹是否存在，如果存在就不需要新建了！）

在当前目录下生成密钥文件"github_rsa.pub"  
`ssh-keygen -t rsa -C "在这里输入任何内容，最好是邮箱"`

注意，ssh-keygen 会生成两个文件：`id-rsa`和`id-rsa.pub`  
我们需要查看第二个后缀为 pub 的文件，这就是密钥文件

查看文件，并复制文件内的所有内容：`cat id-rsa.pub`

<br>

进入 github 官网，登录用户后依次点击：右上角头像->settings->SSH and GPG Keys

点击`new ssh key`  
新建 key 的 title 可以瞎写  
然后最下方的 key 就粘贴进我们复制的 pub 文件内的所有内容，然后点击 `add ssh key`

> 如果复制的内容末尾有空格或者换行啥的，务必删掉！！！

<br>

最后！！！最关键的一步

当然是连接到我们的 github 上面去啦 `ssh -T git@github.com`

<br>

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

### QEMU

> QEMU 在模拟 linux 操作系统上作用巨大，可以帮助你在不购买 linux 开发板的情况下依然可以完美运行操作系统来完成实验

<br>

#### 完整安装流程

准备一个 ubuntu 系统，可以是虚拟机也可以是物理机

首先设置允许使用虚拟化，终端直接执行：

```sh
egrep -c '(vmx|svm)' /proc/cpuinfo
```

安装运行 QEMU 必备的环境与软件包

```sh
sudo apt install qemu-kvm qemu-system qemu-utils python3 python3-pip libvirt-clients libvirt-daemon-system bridge-utils virtinst libvirt-daemon virt-manager -y
```

安装的软件包主要关注以下这几个：

- `qemu-kvm` QEMU-KVM 将 QEMU 和 KVM 结合在一起，利用 KVM 提供的硬件虚拟化扩展加速虚拟化过程，同时利用 QEMU 提供的设备模拟功能来管理虚拟机的硬件资源，给予使用者几乎原生的体验。
- `libvirt-daemon` 其为 libvirt 库的一部分，它是一种用于管理虚拟化平台的后台守护进程，负责与虚拟化平台进行通信，并提供统一的接口供其他应用程序或工具使用
- `virt-manager` 一个基于图形界面的虚拟机管理工具，用于管理和控制基于 libvirt 的虚拟化平台

<br>

执行以下代码，确保已经打开了 libvirt 守护进程

```sh
sudo apt install libvirt-daemon
sudo systemctl enable libvirtd
sudo systemctl start libvirtd
```

查看当前 libvirt 运行状态

```sh
sudo systemctl status libvirtd.service
```

<br>

配置默认网络

```sh
sudo virsh net-autostart default # 设置自动开启
sudo virsh net-list --all # 列出当前network状态
```

<br>

安装完毕，运行图形化虚拟机管理程序

```sh
virt-manager
```

看见下面的界面后，双击`QEMU/KVM`，如果链接过程没有任何报错的话，那就表明安装成功了！

<br>

#### 部分错误处理

部分情况下会出现：明明已经确认开启了 libvirt 服务，但是在 virt-manager 链接时总是报错说 libvirt 服务未开启

直接以 root 身份进入文件夹：`/var/run/libvirt` 后修改文件 `libvirt-sock`的权限为 777

```sh
sudo -s
cd /var/run/libvirt
chmod 777 libvirt-sock
```

权限修改完毕，确认 libvirt 服务器已开启后，链接虚拟机，成功！

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
