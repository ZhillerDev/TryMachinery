### 标准文件结构

<br>

#### widget.h

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
