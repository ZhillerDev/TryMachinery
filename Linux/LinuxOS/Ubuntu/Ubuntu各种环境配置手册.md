## Vscode

<br>

### 配置 CPP 开发环境

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

### cmake
