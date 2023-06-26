### 解耦

<br>

#### slots 解耦

当我们项目随着功能的扩展而逐渐变大时，就需要把重复使用到的，比较冗杂的功能都抽离出来  
比如我们接下来演示的，槽函数 slot 以及对应的思念好 signal 都抽离到一个单独的文件进行处理

在开始之前，我们新建一个 pushbutton，为后续实现 clicked 槽函数做准备

<br>

新项目里面，新建一个类 `Slots.cpp`，IDE 会为你自动新建一个对应的头文件 `Slots.h`

首先进入头文件

```cpp
#ifndef SLOTS_H
#define SLOTS_H

#include <QObject>

// 务必继承QObject
class Slots:public QObject
{
    Q_OBJECT // 别忘了这个

public:
    // 这里设置Slots构造函数的形参为QWidget而非QObject，是为了后续使用connect函数更加方便
    explicit Slots(QWidget *parent = nullptr);

// 定义信号
signals:

// 定义槽函数
public slots:
    void on_pushButton_clicked(); // 这个就是我们要实现的槽函数
};

#endif // SLOTS_H
```

<br>

来到 widget.h，定义 Slots

```cpp
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>
#include "Slots.h" // 引入头文件

QT_BEGIN_NAMESPACE
namespace Ui { class Widget; }
QT_END_NAMESPACE

class Widget : public QWidget
{
    Q_OBJECT

public:
    Widget(QWidget *parent = nullptr);
    ~Widget();


private:
    // 在这里定义外部类Slots
    Slots *slotss;
    Ui::Widget *ui;

};
#endif // WIDGET_H
```

然后来到 widget.cpp，实例化 Slots 类

```cpp
#include "Widget.h"
#include "ui_Widget.h"
#include <QDebug>

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);
    // 实例化
    slotss = new Slots(this);
}

Widget::~Widget()
{
    delete ui;
}
```

<br>

> 实现 connect 主要有以下两种方法，按照个人喜好去选择最适合你的一种方法

**方法一：在 Slots 构造函数内实现 connect**

打开 Slots.cpp，我们可以直接在 slots 内部实现 connect，无需放到 widget 里面实现

```cpp
// slots.cpp
#include "Slots.h"

// 别忘了导入这些头文件，不然没法用
#include <QDebug>
#include <QWidget>
#include <QPushButton>

Slots::Slots(QWidget *parent) : QObject(parent)
{
    // 参数一：parent即为继承而来的QWidget，通过findChild以及对应按钮名称找到该组件，替代ui的作用
    // 参数二：按钮点击信号
    // 参数三：指向当前类
    // 参数四：指向对应的槽函数
    connect(parent->findChild<QPushButton*>("pushButton"),&QPushButton::clicked,this,&Slots::on_pushButton_clicked);
}

// 实现槽函数，我们简单的输出一段文字
void Slots::on_pushButton_clicked(){
    qDebug() << "Slot 1 called!";
}
```

然后直接点击运行程序，点击按钮，发现输出了 `Slot 1 called!`

<br>

**方法二：在 widget.cpp 内部实现 connect**

这一步依然需要实现槽函数 on_pushButton_clicked，只不过 Slots 的构造函数不用再写 connect 了！

打开 widget.cpp

下面的代码就不做过多解释了，就和我们普通的 connect 一样来写就可以了

```cpp
Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);
    slotss = new Slots(this);

    // 使用你熟悉的ui方式来获取对应的组件
    // 信号就是&QPushButton::clicked
    // 槽函数就使用我们在Slots类内实现的方法on_pushButton_clicked
    connect(ui->pushButton,&QPushButton::clicked,slotss,&Slots::on_pushButton_clicked);
}
```

> 该方法可以少写一部分代码，但是未能做到完全解耦，仍然需要在 widget.cpp 里面进行 connect 链接信号和槽

<br>

#### signal 解耦

信号解耦十分简单，建议看完上面的槽解耦然后才来看这个，不然内容跳跃有点大不好衔接

我们仅需在上一节构建的 Slots.h 头文件内，使用 signals 注册对应的信号即可

如下图，我定义了一个叫做 normalSignal 的信号，他接受一个字符串作为形参

```cpp
class Slots:public QObject
{
    ...

signals:
    void normalSignal(QString msg);
};
```

<br>

之后在 Slots.cpp 内，直接使用 emit 即可触发对应的信号，然后再用 connect 进行信号和槽的关联即可

<br>

#### 全局变量解耦
