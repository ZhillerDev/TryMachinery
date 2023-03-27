### 安装与配置

> 由于使用 windowsserver 不可避免的需要支付版权费，导致对应的服务器价格较高，故最好选用开源的 linux 操作系统

首先前往官网安装对应镜像（目前最新镜像 22 版）：https://ubuntu.com/

安装过程大家可以自行百度搜索，我这边是标准安装（包含浏览器和其他工具），中文页面

<br>

#### 配置 ssh

默认情况下 ubuntu 并不支持 ssh，此时我们需要安装一个库：  
`sudo apt-get install openssh-server`

此时即可通过 xshell 连接到 ubuntu（不过此时只能普通用户链接，root 默认无法连接，后续会有介绍）

如果 ssh 服务器出错，请执行此代码重启该服务：`sudo service ssh restart`

<br>

#### 配置 root

> 参考文献：https://blog.csdn.net/m0_46128839/article/details/116133371

一般的，ubuntu 会默认创建一个 root 账户，但为避免后续使用 root 是的权限配置繁杂问题，我们这里一次性解决

首先任意位置开启终端，配置 root 密码：

```sh
sudo passwd root
```

找到该路径下的文件 `/usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf`  
赋予其修改权限：`sudo chmod 777 /usr/share/lightdm/lightdm.conf.d/50-ubuntu.conf`  
使用 gedit 打开该文件，文件末尾添加两个配置：

```
greeter-show-manual-login=true
all-guest=false
```

<br>

对以下两个文件修改权限（你可以到指定路径检查一下看有没有这两个文件）

```sh
sudo  chmod  777 /etc/pam.d/gdm-autologin
sudo  chmod  777 /etc/pam.d/gdm-password

```

使用 gedit 打开这两个文件，均注释掉`auth required pam_success_if.so user!=root quiet_success`

<br>

进入根目录下的 root 文件夹；  
注意开启“显示隐藏文件”  
故使用 root 身份后 gedit 开始编辑该文件

```sh
sudo -i
gedit /root/.profile
```

把文件最后一行注释掉，添加这一行：`tty -s&&mesg n || true`

<br>

#### ssh 链接 root

首先使用 root：`sudo -i`

sshd_config 是专门管理 ssh 的文件；

使用 gedit 开启该文件：`gedit /etc/ssh/sshd_config`

找到 Authentication 这一行，在他下面依次添加如下所示代码，添加完毕后保存即可

```
# Authentication:
LoginGraceTime 120
PermitRootLogin yes
StrictModes yes
```

之后别忘了重启 ssh 服务：`/etc/init.d/ssh restart`

然后使用本地终端测试一下能否 ssh 链接：`ssh localhost`  
刚开始可能叫你输入密码啥的，直接 root 密码搞进去就行了

<br>

### cockpit

> 简介：Cockpit 是红帽开发的网页版图像化服务管理工具，优点是无需中间层，且可以管理多种服务。

<br>

#### 基础

安装 cockpit 并开启对应端口

```sh
apt install cockpit
systemctl enable --now cockpit.socket
```

<br>

一般的，linux 防火墙会自动阻止端口的开启，我们要让防火墙允许对应端口

这里有两种方式，分别对应两种不同的防火墙插件（ubuntu 默认使用 ufw）

```sh
# ufw方法，非常简单直接允许即可
ufw allow 9090

# firewall方法，需要允许指定端口后重新加载才可以
firewall-cmd --add-service=cockpit --permanent
firewall-cmd --reload
```

<br>

使用自带的 firefox，转到网址 localhost:9090，即可进入 cockpit 后台管理页面

<br>

### Vs2022 配置 Ubuntu 开发环境

首先 vmware（或者其他虚拟机平台）安装好 ubuntu  
确认网络连接的方式为 NAT

在 ubuntu 下安装以下包：

```sh
# gdb在较高版本的ubuntu已经默认安装了，此处可忽略
sudo apt install gdb

# ssh配置的方法请看上文
sudo apt install ssh

sudo apt install g++
sudo apt install gdbserver
```

<br>

打开 VS2022，创建新的 linux console 项目

新项目创建完毕，点击 debug 按钮（即绿色小三角），弹出窗口让你选择远程主机的地址、用户名、密码  
按照虚拟机中的来填即可

<br>
