### 标准文件结构

<br>

#### widget.h

widget 对象的头文件

一般会直接在头文件导入所有后续在 cpp 文件内用到的类，所以 `include` 基本都会写在这里

```cpp
// 头文件标志起始
#ifndef WIDGET_H
#define WIDGET_H

// 头文件导入
#include <QWidget>

// 这一块不要动，你也动不了现在
QT_BEGIN_NAMESPACE
namespace Ui { class Widget; }
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

// 初始化定义区域，定义非信号和槽方法
public:
    Widget(QWidget *parent = nullptr);
    ~Widget();

// 定义信号的区域
signals:

// 定义槽的区域
private slots:
    void on_pushButton_clicked();

// 定义全局私有变量
private:
    Ui::Widget *ui;
};
#endif // WIDGET_H
```

<br>

#### widget.cpp

```cpp
// 头文件导入区
#include "Widget.h"
#include "ui_Widget.h"

// 主构造函数，可以自定义构造函数的参数以及继承规则
Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

    // 定义Widget被实例化后立刻执行的代码
    // 比如TCP链接或者调试输出啥的
}

// 析构函数，用于Widget被销毁前需执行的代码
Widget::~Widget()
{
    delete ui;
}

// 在这里定义信号以及槽的具体实现方法
// ...
```

<br>

#### main.cpp

主入口文件

```cpp
#include "Widget.h"

#include <QApplication>

// 主入口，代码从这里执行
int main(int argc, char *argv[])
{
    QApplication a(argc, argv);

    // 实例化widget后使用show显示他
    Widget w;
    w.show();

    // 程序结束，使用exec
    return a.exec();
}
```

<br>

#### pro 文件

该文件比较复杂，具体使用方式请查看帮助文档，这里没办法告诉你具体的使用方式

最常用的就是当你使用 TCP 链接或者任意网络请求时，必须要在第一行的末尾添加一个 network，就在下方代码第一行末尾注释区那边

```py
QT       += core gui #network

greaterThan(QT_MAJOR_VERSION, 4): QT += widgets

CONFIG += c++11

# You can make your code fail to compile if it uses deprecated APIs.
# In order to do so, uncomment the following line.
#DEFINES += QT_DISABLE_DEPRECATED_BEFORE=0x060000    # disables all the APIs deprecated before Qt 6.0.0

SOURCES += \
    main.cpp \
    Widget.cpp

HEADERS += \
    Widget.h

FORMS += \
    Widget.ui

# Default rules for deployment.
qnx: target.path = /tmp/$${TARGET}/bin
else: unix:!android: target.path = /opt/$${TARGET}/bin
!isEmpty(target.path): INSTALLS += target
```

<br>

### 信号与槽

<br>

#### 自定义信号

> 实现功能：点击按钮发射一个信号，widget 获取信号后执行对应槽函数输出一段信息（此过程含有信息的传递）

新建一个 `Widget` 文件，UI 设计图添加一个 pushbutton，重命名为 firstBtn，并且为其添加一个空的 clicked() 槽

此时的 `Widget.h` 文件应该是这样的

```cpp
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include <QDebug>

QT_BEGIN_NAMESPACE
namespace Ui { class Widget; }
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    Widget(QWidget *parent = nullptr);
    ~Widget();

// 自定义一个新的信号，其接收一个字符串参数
signals:
    void firstSignal(QString msg);

// firstEmit为自定义槽函数，用于响应自定义信号firstSignal
// on_firstBtn_clicked为按钮点击相应槽函数
private slots:
    void firstEmit(QString msg);
    void on_firstBtn_clicked();

private:
    Ui::Widget *ui;
};
#endif // WIDGET_H
```

代码清单 `Widget.cpp`

注意，如果信号顶一个 N 个形参，那么对应接收的槽也必须有对应数量与类型的形参！因为发射的信号的所有参数值都会一一传递给槽函数，所有参数都是对应关系！

```cpp
#include "Widget.h"
#include "ui_Widget.h"

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

    // 第一步，connect链接信号和槽
    // 参数一：信号发出者，这里选择当前widget
    // 参数二：欲发出的信号
    // 参数三：信号接收者，这里也是当前widget
    // 参数四：欲处理对应信号的槽函数
    connect(this,&Widget::firstSignal,this,&Widget::firstEmit);
}

Widget::~Widget()
{
    delete ui;
}

// 第二步：定义处理信号的槽函数
// 函数有一个形参，用于接收信号传递过来的参数
void Widget::firstEmit(QString msg)
{
    // 调试输出信号发射过来的参数msg
    qDebug() << msg;
}

// 第三步：定义发射信号的按钮响应槽函数
void Widget::on_firstBtn_clicked()
{
    // 使用emit发射对应名称的信号，注意我们这里传入了一个字符串作为参数
    emit firstSignal("shit");
}
```

此时保存文件，编译运行，可见点击按钮后就会在 console 里面看见我们输出的调试信息了！

<br>

### 调试

<br>

#### qDebug

一个简单的输出调试信息的 debug 方法

```cpp
// 导入头文件
#include <QDebug>

...

// 按钮点击事件槽
void Widget::on_pushButton_clicked()
{
    // 使用此格式输出调试信息
    qDebug() << "这是一条测试信息";
}
```

<br>
