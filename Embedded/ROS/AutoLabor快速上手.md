## 踩坑

<br>

### root 权限问题

#### vscode 权限不够无法读写文件

报错场景：直接使用普通用户启动 vscode，在编辑由 root 用户创建的文件是，提示没有权限

由于 vscode 扯淡的特性，在 root 下无法直接打开 vscode，你需要使用如下一行代码来启动 vscode

`sudo code --no-sandbox --disable-gpu-sandbox --user-data-dir=/root/.vscode/`

<br>
