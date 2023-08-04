## 安装报错

<br>

### Could not get lock /var/lib/dpkg/lock-frontend

> 此错误一般出现在我们实验 apt-get 安装库或者软件时出现

`could not get lock` 后面的内容是可变的，我们需要先把这一个路径复制到剪贴板上

然后终端执行以下代码  
`sudo rm -rf /var/lib/dpkg/lock-frontend`

杀死后，再次执行安装步骤，发现成功了！

<br>
