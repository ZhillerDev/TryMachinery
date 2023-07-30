## 网络编程与套接字

<br>

### 理解网络编程与套接字

#### 网络编程中接收连接请求套接字过程

以下为一个服务端构建 socket 套接字需要做到的完整四个步骤

1. 调用 socket 函数创建套接字
2. 调用 bind 函数分配 IP 地址和端口号
3. 调用 listen 函数转换为可接受请求的状态
4. 调用 accept 函数受理连接请求

<br>

#### 客户端套接字处理

对于需要连接上服务端的客户端而言，相对于的代码就简洁了很多

仅需先调用 socket 函数创建套接字，之后再调用 connect 函数向服务器端发送链接请求

<br>

### 基于 linux 的文件操作

#### 文件描述符

文件描述符（file descriptor）是一个整数值，用于表示操作系统中的打开文件或者输入/输出设备。它是对打开文件或设备的引用，通过文件描述符可以进行读取、写入和其他操作。

文件和套接字一般需要经过创建过程才会被分配文件描述符

在 UNIX 系统中，对应的输入输出三个文件描述符是：
标准输入（stdin）、标准输出（stdout）和标准错误（stderr）分别有文件描述符 0、1 和 2

<br>

#### 数据操作

由于 Linux 不区分文件以及套接字，可以直接使用对应的文件处理函数来处理套接字  
比如 open、close、write

常见的以 `_t` 结尾的数据类型：

1. `size_t`: 这是一个无符号整数类型，在 <cstddef> 头文件中定义。它用于表示对象大小或容器的大小。
2. `ptrdiff_t`: 这是一个有符号整数类型，在 <cstddef> 头文件中定义。它用于表示指针之间的差异（偏移量）。
3. `time_t`: 这是一个整数类型，在 <ctime> 头文件中定义。它用于表示从特定时间点（通常是 1970 年 1 月 1 日）起经过的秒数。
4. `int32_t, int64_t, uint32_t, uint64_t`: 这些是固定宽度的整数类型，在 <cstdint> 头文件中定义。它们分别表示有符号的 32 位整数、有符号的 64 位整数、无符号的 32 位整数和无符号的 64 位整数。

<br>

这是一个简单的，使用文件操作的小案例

```c
#include <iostream>   // 标准输入输出操作
#include <fcntl.h>    // 文件控制选项
#include <unistd.h>   // 文件操作

int main(void) {
    int fd;                              // 文件描述符变量
    char buf[] = "helloworld\n";         // 要写入的数据缓冲区

    fd = open("data.txt", O_CREAT | O_WRONLY | O_TRUNC);  // 打开文件以进行写入操作
    if (fd == -1) {
        // 如果文件打开失败，进行错误处理
        std::cerr << "无法打开文件。" << std::endl;
        return 1;  // 返回非零值表示错误
    }

    std::cout << "文件描述符：" << fd << std::endl;  // 打印文件描述符

    if (write(fd, buf, sizeof(buf)) == -1) {
        // 如果写入操作失败，进行错误处理
        std::cerr << "写入文件失败。" << std::endl;
        close(fd);  // 在返回前关闭文件
        return 1;   // 返回非零值表示错误
    }

    close(fd);   // 关闭文件
    return 0;    // 返回0表示成功执行
}
```

<br>

#### 同时创建文件描述符与套接字

以下代码示例，创建文件和套接字，使用整数返回文件描述符值

```cpp
#include <iostream>   // 标准输入输出操作
#include <sys/types.h>   // socket 函数相关类型
#include <sys/socket.h>  // socket 函数
#include <fcntl.h>    // 文件控制选项
#include <unistd.h>   // 文件操作

int main(void) {
    int fd1, fd2, fd3;
    fd1 = socket(PF_INET, SOCK_STREAM, 0);  // 创建 TCP 套接字
    fd2 = open("test.txt", O_CREAT | O_WRONLY | O_TRUNC);  // 打开文件进行写入操作
    fd3 = socket(PF_INET, SOCK_DGRAM, 0);  // 创建 UDP 套接字

    close(fd1);  // 关闭套接字
    close(fd2);  // 关闭文件
    close(fd3);  // 关闭套接字

    return 0;  // 返回0表示成功执行
}
```

<br>

### 基于 windows 平台的实现

windows 下的句柄相当于 linux 的文件描述符  
但不同的是 windows 还包括了文件句柄和套接字句柄

由于 windows 下的 CPP 开发不切实际，所以这里就不多陈述了，大家可以查阅相关的资料补全这部分内容，如果大家感兴趣的话！

接下来将会把全本的笔记主要集中在 linux 开发上

<br>

## 套接字类型和协议设置

<br>

### 套接字协议及其数据传输特性

如何创建套接字？

```cpp
int socket(int domain, int type, int protocol);       // domain：采取的协议族，一般为 PF_INET；type：数据传输方式，一般为 SOCK_STREAM；protocol：使用的协议，一般设为 0 即可。
                //成功时返回文件描述符，失败时返回 -1
```

创建套接字的函数 socket 的三个参数的含义：

- `domain`：使用的协议族。一般只会用到 PF\*INET，即 IPv4 协议族。
- `type`：套接字类型，即套接字的数据传输方式。主要是两种：SOCK_STREAM（即 TCP）和 SOCK\*（即 UDP）。
- `protocol`：选择的协议。一般情况前两个参数确定后，protocol 也就确定了，所以设为 0 即可。

<br>

#### 协议族

![](./img/tcpip/t1.png)

重点关注 IPV4 对应的 `PF_INET` 协议族，绝大部分情况下使用它，即便目前 IPV6 正在推广

<br>

#### 套接字类型

`SOCK_STREAM` 面向链接的套接字，代表 TCP 协议

- 可靠传输，传输的数据不会消失。
- 按序传输。
- 传输的数据没有边界：从面向连接的字节流角度理解。接收方收到数据后放到接收缓存中，用户使用 read 函数像读取字节流一样从中读取数据，因此发送方 write 的次数和接收方 read 的次数可以不一样。

```cpp
int tcp_socket = socket(PF_INET, SOCK_STREAM, 0);
```

<br>

`SOCK_DGRAM` 面向消息的套接字，代表 UDP 协议

- 快速传输。
- 传输的数据可能丢失、损坏。
- 传输的数据有数据边界：这意味着接收数据的次数要和传输次数相同，一方调用了多少次 write（send），另一方就应该调用多少次 read（recv）。
- 限制每次传输的数据大小。

```cpp
int udp_socket = socket(PF_INET, SOCK_DGRAM, 0);
```

<br>

#### 协议最终选择
