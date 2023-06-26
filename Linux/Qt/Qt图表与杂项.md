## 图表处理

<br>

### 环境准备

> 此步骤可以被省略  
> 此步是为了更好的为构建图表解耦，不然一堆代码全部挤到 widget.cpp 里面很是恶心

首先首先！！！必须要在 pro 文件添加 charts 库，如果你安装 QT 的时候没有勾选安装 QTCharts 库，那么就算是导入了也没法用  
`QT       += charts`

项目内部新建文件 ChartsComp.cpp

先看对应的头文件 `ChartsComp.h`

```cpp
#ifndef CHARTSCOMP_H
#define CHARTSCOMP_H

#include <QObject>
#include <QWidget>

// 导入所有的图标库，之后利用对应的组件就不需要一一导入了
#include <QtCharts/QtCharts>

// 继承QWidget
class ChartsComp:public QWidget
{
    Q_OBJECT
public:
    // 存储父widget的变量
    QWidget *mainWidget;
    // 主构造函数接收父widget并保存起来
    ChartsComp(QWidget *wd);

public:
    // 这是我们的一个示例函数
    QChartView *simplePie();
};

#endif // CHARTSCOMP_H
```

<br>

再来看看 `ChartsComp.cpp`

```cpp
#include "ChartsComp.h"

ChartsComp::ChartsComp(QWidget *wd)
{
    // 获取父widget并保存到对应变量，方便下面的函数直接调用
    mainWidget = wd;
}

// 添加饼状图的代码
QChartView *ChartsComp::simplePie(){
    // 数据
    QVector<QPointF> points;
    points << QPointF(1, 5) << QPointF(2, 3)
            << QPointF(3, 8) << QPointF(4, 1);

    // 创建图表
    QChart *chart = new QChart();
    chart->setTitle("Simple Line Chart");

    // 数据系列
    QLineSeries *series = new QLineSeries();
    series->setName("Line");
    foreach (QPointF point, points) {
        series->append(point);
    }

    // 添加数据系列到图表
    chart->addSeries(series);

    // 创建坐标轴
    chart->createDefaultAxes();

    // 创建图表视图和显示
    QChartView *chartView = new QChartView(chart);
    chartView->setRenderHint(QPainter::Antialiasing);
    chartView->resize(400, 300);

    return chartView;
}
```

<br>

此时通过 qtdesigner，为父 widget 添加一个 `QGridlayout`，并将其重命名为 gdLayout  
当 `QChartView` 组件被添加到 layout 里面后，就会自动显示，所以我们采用此方法

此时我们就把构建图表的代码分解出来了，之后直接来到 widget.h 导入 chartssection 类的头文件

```cpp
#ifndef WIDGET_H
#define WIDGET_H

#include <QWidget>

// 导入类
#include "ChartsComp.h"

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
    ChartsComp *ccomp; // 定义外部图表类
    Ui::Widget *ui;
};
#endif // WIDGET_H
```

然后来到 widget.cpp 添加下述代码，以把图标渲染到对应的 layout 里面去

```cpp
#include "Widget.h"
#include "ui_Widget.h"

void initWidget(Ui::Widget *ui){
    ui->gridLayout->cellRect(2,2);
}

Widget::Widget(QWidget *parent)
    : QWidget(parent)
    , ui(new Ui::Widget)
{
    ui->setupUi(this);

    // 实例化图表类
    ccomp = new ChartsComp(this);
    // 由于我们的方法直接返回一个QChartView，故使用addWidget直接往layout里面塞就可以了
    ui->gridLayout->addWidget(ccomp->simplePie());
}

Widget::~Widget()
{
    delete ui;
}
```

<br>

### 典型例图
