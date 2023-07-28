### JSON

<br>

#### 常用库

`QJsonArray` 用于表示 JSON 中的数组结构  
`QJsonObject` 用于表示 JSON 中的对象结构  
`QJsonValue` 表示一个任意的值，一般在解析 JSON 时会用到

`QJsonDocument` 用于将 QJsonObject 对象转换为 JSON 对象

<br>

#### 简单示例

新建一个 `qt console application` 项目，即命令行项目

```c
#include <QCoreApplication>  // 包含Qt库的核心应用程序类

#include <QJsonArray>  // 包含Qt的JSON数组类
#include <QJsonObject>  // 包含Qt的JSON对象类
#include <QJsonDocument>  // 包含Qt的用于读写JSON文档的类
#include <QJsonValue>  // 包含Qt的JSON值类

#include <QFile>  // 包含Qt的文件类
#include <QDebug>  // 包含Qt的调试类

void writeJSON(){  // 创建一个名为writeJSON的函数
    // 创建根对象
    QJsonObject obj;  // 创建一个QJsonObject对象，用作JSON的根对象
    obj.insert("name","cn");  // 向根对象中加入一个名为"name"，值为"cn"的属性

    // 插入info对象
    QJsonObject info;  // 创建一个QJsonObject对象，用作子对象
    info.insert("capital","beijing");  // 向子对象中加入一个名为"capital"，值为"beijing"的属性
    info.insert("asian",true);  // 向子对象中加入一个名为"asian"，值为true的属性
    info.insert("founded",1949);  // 向子对象中加入一个名为"founded"，值为1949的属性
    obj.insert("info",info);  // 向根对象中加入一个名为"info"，值为子对象info的属性

    // 插入provinces数组
    QJsonArray provinces;  // 创建一个QJsonArray对象，用作子对象数组
    QJsonObject zj;  // 创建一个QJsonObject对象，用作数组元素
    QJsonObject sh;  // 创建一个QJsonObject对象，用作数组元素
    zj.insert("name","zhejiang");  // 向元素中加入一个名为"name"，值为"zhejiang"的属性
    zj.insert("capital","nanjing");  // 向元素中加入一个名为"capital"，值为"nanjing"的属性
    sh.insert("name","shanghai");  // 向元素中加入一个名为"name"，值为"shanghai"的属性
    sh.insert("capital","shanghai");  // 向元素中加入一个名为"capital"，值为"shanghai"的属性
    provinces.append(zj);  // 将元素zj添加到数组中
    provinces.append(sh);  // 将元素sh添加到数组中
    obj.insert("provinces",provinces);  // 向根对象中加入一个名为"provinces"，值为子对象数组provinces的属性

    // 将JSONobj对象转换为json字符串
    QJsonDocument json(obj);  // 创建一个QJsonDocument对象，用于将QJsonObject对象转换为JSON格式
    QByteArray buff = json.toJson();  // 将QJsonDocument对象转换为QByteArray对象，即JSON格式的字符串
    qDebug()<< QString(buff).toUtf8().data() << Qt::endl;  // 输出JSON格式的字符串
}

int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);  // 创建一个Qt应用程序

    writeJSON();  // 调用writeJSON函数

    return a.exec();  // 运行Qt应用程序并返回结果
}
```

<br>

这个案例很简单，最终我们会得到如下的输出

```json
{
	"info": {
		"asian": true,
		"capital": "beijing",
		"founded": 1949
	},
	"name": "cn",
	"provinces": [
		{
			"capital": "nanjing",
			"name": "zhejiang"
		},
		{
			"capital": "shanghai",
			"name": "shanghai"
		}
	]
}
```

<br>

#### JSON 解析

和生成一个 JSON 的方法类似，反操作而已

```c
void parseJSON(){
    // 打开JSON文件
    QFile file("g:\\demo.json");
    file.open(QFile::ReadOnly);
    QByteArray buff = file.readAll();
    file.close();

    // 将bytearray转换为json对象
    QJsonDocument doc = QJsonDocument::fromJson(buff);
    if(!doc.isObject()){
        qDebug() << "真是扯淡，你给我的JSON不是一个对象！！！";
        return;
    }

    // json转换为obj
    QJsonObject obj = doc.object();
    // 获取当前obj下所有的keys
    QStringList keys = obj.keys();
    // 进一步根据keys解析对应的属性
    foreach (QString key, keys) {
        qDebug() << key ;

        // QJsonValue用于提取一整个属性下的内容，故需要我们人工进行属性类型的判断
        QJsonValue val = obj.value(key);
        if(val.isArray()){

            // 一旦判断其为数组类型，就将其转换为数组，并保存为JSONobj的类型
            QJsonArray arr = val.toArray();
            // 使用foreach遍历该obj，然后逐一获取对应属性
            for(int i=0;i<arr.size();i++){
                QJsonObject o = arr[i].toObject();
                QString name = o["name"].toString();
                QString capital = o["capital"].toString();
            }
        }else if(val.isBool()){

        }else if(val.isString()){

        }
    }
}
```

<br>

输出内容不太重要，大家只需呀理解上面这一大坨代码就好了

JSON 解析其实很简单，就是一个逆过程，只需要按照 JSON 的深度依次拆解后赋予对应的类型即可提取出对应属性及其值

<br>

### XML

#### 常见 xml 库及其成员函数

三个重要的 xml 库

1. QDomDocument 类：QDomDocument 类是 Qt 中最常用的 XML 文档类之一，用于创建、读取和操作 XML 文档。

   - setContent()：从一个 QIODevice 对象或 QString 对象中加载 XML 文档。
   - createElement()：创建一个 XML 元素。
   - createAttribute()：创建一个 XML 属性。
   - createTextNode()：创建一个 XML 文本节点。
   - appendChild()：将一个 XML 元素或文本节点添加到另一个 XML 元素中。
   - toByteArray()：将 QDomDocument 对象转换为 QByteArray 对象。

2. QXmlStreamReader 类：QXmlStreamReader 类是另一个用于读取 XML 文档的类。

   - setDevice()：将 QXmlStreamReader 对象与 QIODevice 对象关联。
   - readNext()：移动到下一个 XML 标记。
   - isStartElement()：检查当前标记是否是一个起始元素。
   - attributes()：返回当前元素的属性列表。
   - text()：返回当前元素的文本。

3. QXmlStreamWriter 类：QXmlStreamWriter 类是另一个用于写入 XML 文档的类。它提供了一组 API，可以让您轻松地创建 XML 元素、属性和文本，并将它们写入到一个 QIODevice 对象中。

   - setDevice()：将 QXmlStreamWriter 对象与 QIODevice 对象关联。
   - writeStartElement()：写入一个起始元素。
   - writeAttribute()：写入一个元素属性。
   - writeCharacters()：写入一个文本节点。
   - writeEndElement()：写入一个结束元素。

<br>

#### 写 XML 文件

首先需要在 pro 文件添加 `QT += xml`

下面一个简单的案例展示了写 XML 文件到当前目录下的函数

```cpp
void Widget::on_pushButton_clicked()
{
    // 创建一个QDomDocument对象
    QDomDocument doc;

    // 创建根元素
    QDomElement root = doc.createElement("root");
    doc.appendChild(root);

    // 添加一个子元素
    QDomElement child = doc.createElement("child");
    root.appendChild(child);

    // 添加一个属性
    child.setAttribute("id", "1");

    // 添加一些文本
    QDomText text = doc.createTextNode("Hello World!");
    child.appendChild(text);

    // 将文档保存到文件中
    QFile file("example.xml");
    if (!file.open(QIODevice::WriteOnly | QIODevice::Text)){
        return;
    }

    QTextStream stream(&file);
    stream << doc.toString();
    file.close();
}
```

最终打开 xml 文件是这样的

```xml
<!-- example.xml -->
<root>
 <child id="1">Hello World!</child>
</root>
```

<br>

#### 读 XML 文件

以上一小节生成的 example.xml 作为蓝本，用下面的代码读取对应内容

```cpp
#include <QDomDocument>
#include <QFile>
#include <QDebug>

int main()
{
    QDomDocument doc;
    QFile file("example.xml");

    if (!file.open(QIODevice::ReadOnly | QIODevice::Text)) {
        qDebug() << "Failed to open file";
        return 1;
    }

    if (!doc.setContent(&file)) {
        file.close();
        qDebug() << "Failed to parse the XML file";
        return 1;
    }

    file.close();

    // 获取根元素
    QDomElement root = doc.documentElement();

    // 遍历子元素
    QDomNodeList children = root.childNodes();
    for (int i = 0; i < children.count(); i++) {
        QDomNode node = children.at(i);
        if (node.isElement()) {
            QDomElement child = node.toElement();
            qDebug() << "Child element name: " << child.tagName();
            qDebug() << "Child element id attribute: " << child.attribute("id");
            qDebug() << "Child element text: " << child.text();
        }
    }

    return 0;
}
```

<br>

观察控制台输出

```
Child element name:  "child"
Child element id attribute:  "1"
Child element text:  "Hello World!"
```

<br>

### 网络请求

<br>

#### 实用概念

`QNetworkAccessManager`是 Qt 网络模块提供的一个用于发送网络请求和接收网络响应的类。它是一个高层次的网络 API，提供了一种方便的方式来发送和接收 HTTP、FTP 和其他网络协议的数据。`QNetworkAccessManager`的主要功能包括：

1. 发送网络请求：`QNetworkAccessManager`提供了多种发送网络请求的方法，包括`get()`、`put()`、`post()`、`deleteResource()`等。

2. 接收网络响应：`QNetworkAccessManager`可以异步接收网络响应，并在响应到达时发出信号。

3. 处理网络错误：`QNetworkAccessManager`可以处理网络错误，例如连接超时、网络不可用等。

4. 处理网络代理：`QNetworkAccessManager`可以使用网络代理来发送和接收网络请求。

使用`QNetworkAccessManager`时，通常需要创建一个`QNetworkRequest`对象来描述网络请求的 URL、HTTP 头、请求方法等信息，并将其传递给`QNetworkAccessManager`的发送请求方法中。

使用`QNetworkAccessManager`时，可以通过连接`QNetworkAccessManager`的信号，例如`finished()`和`sslErrors()`，来处理网络响应和错误。在接收到网络响应时，可以使用`QNetworkReply`类来访问响应的 HTTP 头、数据和状态码等信息。

<br>

`QNetworkReply`是 Qt 网络模块提供的一个用于处理网络响应的类。它提供了访问响应 HTTP 头、数据和状态码等信息的方法，并通过信号和槽机制提供了响应处理的异步方式。

`QNetworkReply`的主要功能包括：

1. 接收网络响应：`QNetworkReply`可以异步接收网络响应，并在响应到达时发出信号。

2. 访问响应数据：`QNetworkReply`提供了多种方法来访问响应数据，例如`read()`、`readAll()`、`bytesAvailable()`等。

3. 访问响应 HTTP 头：`QNetworkReply`提供了多种方法来访问响应的 HTTP 头信息，例如`header()`、`hasRawHeader()`、`rawHeaderList()`等。

4. 访问响应状态码：`QNetworkReply`提供了方法来访问响应的状态码。

使用`QNetworkReply`时，通常需要连接`QNetworkReply`的信号，例如`readyRead()`和`finished()`，来处理响应数据和完成响应处理。在处理响应数据时，可以使用`QIODevice`的读取函数来读取数据。在响应处理完成后，需要调用`QNetworkReply`的`deleteLater()`方法来释放资源。

需要注意的是，由于网络请求和响应的处理是异步的，所以在处理响应时需要注意线程安全和对象生命周期等问题。

<br>

#### 接口请求完整步骤

##### pro 注册

再开始之前，我们需要在项目的 pro 文件内添加网络库

即 `QT       += core gui network`

<br>

##### 配置头文件

> 此处我们需要请求一个天气接口，该接口免费且无需 token 直接使用

天气接口 API： `http://t.weather.itboy.net/api/weather/city/101010100`  
（接口 URL 最末尾的一串数字为城市代码，大家可以上网搜，我这给出的代码是北京的代码）

<br>

需要在头文件内定义一些关键性的内容

代码清单 `MainWindow.h`

```cpp
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QMessageBox>

// 导入必备的两个网络操作库
#include <QNetworkAccessManager>
#include <QNetworkReply>

QT_BEGIN_NAMESPACE
namespace Ui { class MainWindow; }
QT_END_NAMESPACE

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private:
    Ui::MainWindow *ui;

protected:
    // 获取指定城市代码天气信息的函数
    void getWeatherInfo(QString cityCode);

private:
    // 当接收到接口传回的响应时，对应的处理函数
    void onReplied(QNetworkReply *reply);

private:
    // 网络操作对象，用于执行GET或者PUT等基本网络请求
    QNetworkAccessManager *networkManager;
};
#endif // MAINWINDOW_H

```

<br>

##### 主体文件

> 接下来的代码均写在 mainwindow.cpp 里面去

首先应当在构造函数内执行网络管理对象的初始化

```cpp
MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    // 实例化网络管理对象
    networkManager = new QNetworkAccessManager(this);

    // 配置当接收响应时（QNetworkAccessManager::finished），触发的回调函数onReplied
    connect(networkManager,&QNetworkAccessManager::finished,this,&MainWindow::onReplied);

    // 设置我们需要获取的天气的城市代码
    getWeatherInfo("101010100");
}
```

<br>

看一下获取城市天气信息函数的实现

```cpp
void MainWindow::getWeatherInfo(QString cityCode)
{
    // 配置接口API
    QUrl url("http://t.weather.itboy.net/api/weather/city/"+cityCode);
    // 直接使用GET请求
    networkManager->get(QNetworkRequest(url));
}
```

<br>

对应的响应回调函数的实现

```cpp
void MainWindow::onReplied(QNetworkReply *reply)
{
    qDebug() << "请求成功!!!"; // 输出请求成功的信息

    int statusCode = reply->attribute(QNetworkRequest::HttpStatusCodeAttribute).toInt(); // 获取响应状态码

    qDebug() << "状态码：" << statusCode; // 输出状态码
    qDebug() << "URL：" << reply->url(); // 输出请求的URL
    qDebug() << "操作：" << reply->operation(); // 输出操作类型
    qDebug() << "原始头信息：" << reply->rawHeaderList(); // 输出原始头信息

    if(reply->error()!=QNetworkReply::NoError||statusCode!=200){ // 如果响应出现错误或状态码不为200
        qDebug() << reply->errorString().toLatin1().data(); // 输出错误信息
        QMessageBox::critical(this,"错误","无法请求接口，请检查网络是否连接正确",QMessageBox::Ok); // 弹出错误提示框
    }else{ // 如果响应正常
        QByteArray buff = reply->readAll(); // 读取响应数据
        qDebug() << buff.data(); // 输出响应数据
    }

    reply->deleteLater(); // 释放reply对象
}
```

<br>

此时运行程序，就会发现我们成功的请求了接口，并且获取到了对应数据

下面展示的即为接口回馈的 JSON 信息

```json
{
	"message": "success感谢又拍云(upyun.com)提供CDN赞助",
	"status": 200,
	"date": "20230710",
	"time": "2023-07-10 07:42:05",
	"cityInfo": {
		"city": "北京市",
		"citykey": "101010100",
		"parent": "北京",
		"updateTime": "07:31"
	},
	"data": {
		"shidu": "53%",
		"pm25": 12.0,
		"pm10": 37.0,
		"quality": "优",
		"wendu": "29",
		"ganmao": "各类人群可自由活动",
		"forecast": [
			{
				"date": "10",
				"high": "高温 39℃",
				"low": "低温 27℃",
				"ymd": "2023-07-10",
				"week": "星期一",
				"sunrise": "04:55",
				"sunset": "19:44",
				"aqi": 91,
				"fx": "东北风",
				"fl": "2级",
				"type": "多云",
				"notice": "阴晴之间，谨防紫外线侵扰"
			},
			{
				"date": "11",
				"high": "高温 35℃",
				"low": "低温 28℃",
				"ymd": "2023-07-11",
				"week": "星期二",
				"sunrise": "04:55",
				"sunset": "19:44",
				"aqi": 97,
				"fx": "东南风",
				"fl": "2级",
				"type": "多云",
				"notice": "阴晴之间，谨防紫外线侵扰"
			},
			{
				"date": "12",
				"high": "高温 31℃",
				"low": "低温 25℃",
				"ymd": "2023-07-12",
				"week": "星期三",
				"sunrise": "04:56",
				"sunset": "19:43",
				"aqi": 78,
				"fx": "东风",
				"fl": "1级",
				"type": "中雨",
				"notice": "记得随身携带雨伞哦"
			},
			{
				"date": "13",
				"high": "高温 34℃",
				"low": "低温 24℃",
				"ymd": "2023-07-13",
				"week": "星期四",
				"sunrise": "04:57",
				"sunset": "19:43",
				"aqi": 91,
				"fx": "西南风",
				"fl": "2级",
				"type": "小雨",
				"notice": "雨虽小，注意保暖别感冒"
			},
			{
				"date": "14",
				"high": "高温 34℃",
				"low": "低温 24℃",
				"ymd": "2023-07-14",
				"week": "星期五",
				"sunrise": "04:57",
				"sunset": "19:43",
				"aqi": 98,
				"fx": "西北风",
				"fl": "3级",
				"type": "晴",
				"notice": "愿你拥有比阳光明媚的心情"
			},
			{
				"date": "15",
				"high": "高温 35℃",
				"low": "低温 25℃",
				"ymd": "2023-07-15",
				"week": "星期六",
				"sunrise": "04:58",
				"sunset": "19:42",
				"aqi": 69,
				"fx": "西北风",
				"fl": "3级",
				"type": "晴",
				"notice": "愿你拥有比阳光明媚的心情"
			},
			{
				"date": "16",
				"high": "高温 36℃",
				"low": "低温 22℃",
				"ymd": "2023-07-16",
				"week": "星期日",
				"sunrise": "04:59",
				"sunset": "19:41",
				"aqi": 60,
				"fx": "东北风",
				"fl": "2级",
				"type": "晴",
				"notice": "愿你拥有比阳光明媚的心情"
			},
			{
				"date": "17",
				"high": "高温 37℃",
				"low": "低温 23℃",
				"ymd": "2023-07-17",
				"week": "星期一",
				"sunrise": "05:00",
				"sunset": "19:41",
				"aqi": 59,
				"fx": "东南风",
				"fl": "2级",
				"type": "晴",
				"notice": "愿你拥有比阳光明媚的心情"
			},
			{
				"date": "18",
				"high": "高温 39℃",
				"low": "低温 25℃",
				"ymd": "2023-07-18",
				"week": "星期二",
				"sunrise": "05:01",
				"sunset": "19:40",
				"aqi": 85,
				"fx": "东风",
				"fl": "2级",
				"type": "小雨",
				"notice": "雨虽小，注意保暖别感冒"
			},
			{
				"date": "19",
				"high": "高温 38℃",
				"low": "低温 24℃",
				"ymd": "2023-07-19",
				"week": "星期三",
				"sunrise": "05:01",
				"sunset": "19:40",
				"aqi": 77,
				"fx": "东风",
				"fl": "2级",
				"type": "晴",
				"notice": "愿你拥有比阳光明媚的心情"
			},
			{
				"date": "20",
				"high": "高温 39℃",
				"low": "低温 25℃",
				"ymd": "2023-07-20",
				"week": "星期四",
				"sunrise": "05:02",
				"sunset": "19:39",
				"aqi": 54,
				"fx": "东南风",
				"fl": "2级",
				"type": "小雨",
				"notice": "雨虽小，注意保暖别感冒"
			},
			{
				"date": "21",
				"high": "高温 36℃",
				"low": "低温 23℃",
				"ymd": "2023-07-21",
				"week": "星期五",
				"sunrise": "05:03",
				"sunset": "19:38",
				"aqi": 65,
				"fx": "东南风",
				"fl": "2级",
				"type": "多云",
				"notice": "阴晴之间，谨防紫外线侵扰"
			},
			{
				"date": "22",
				"high": "高温 38℃",
				"low": "低温 25℃",
				"ymd": "2023-07-22",
				"week": "星期六",
				"sunrise": "05:04",
				"sunset": "19:37",
				"aqi": 47,
				"fx": "东风",
				"fl": "2级",
				"type": "中雨",
				"notice": "记得随身携带雨伞哦"
			},
			{
				"date": "23",
				"high": "高温 38℃",
				"low": "低温 24℃",
				"ymd": "2023-07-23",
				"week": "星期日",
				"sunrise": "05:05",
				"sunset": "19:37",
				"aqi": 49,
				"fx": "北风",
				"fl": "2级",
				"type": "暴雨",
				"notice": "关好门窗，外出避开低洼地"
			},
			{
				"date": "24",
				"high": "高温 30℃",
				"low": "低温 21℃",
				"ymd": "2023-07-24",
				"week": "星期一",
				"sunrise": "05:06",
				"sunset": "19:36",
				"aqi": 46,
				"fx": "东北风",
				"fl": "1级",
				"type": "中雨",
				"notice": "记得随身携带雨伞哦"
			}
		],
		"yesterday": {
			"date": "09",
			"high": "高温 38℃",
			"low": "低温 25℃",
			"ymd": "2023-07-09",
			"week": "星期日",
			"sunrise": "04:54",
			"sunset": "19:45",
			"aqi": 101,
			"fx": "西北风",
			"fl": "2级",
			"type": "多云",
			"notice": "阴晴之间，谨防紫外线侵扰"
		}
	}
}
```

##### 完整代码结构

头文件 MainWindow.h

```cpp
#ifndef MAINWINDOW_H
#define MAINWINDOW_H

#include <QMainWindow>
#include <QMessageBox>

#include <QNetworkAccessManager>
#include <QNetworkReply>

QT_BEGIN_NAMESPACE
namespace Ui { class MainWindow; }
QT_END_NAMESPACE

class MainWindow : public QMainWindow
{
    Q_OBJECT

public:
    MainWindow(QWidget *parent = nullptr);
    ~MainWindow();

private:
    Ui::MainWindow *ui;

protected:
    void getWeatherInfo(QString cityCode);

private:
    void onReplied(QNetworkReply *reply);

private:
    QNetworkAccessManager *networkManager;
};
#endif // MAINWINDOW_H

```

<br>

主文件 MainWindow.cpp

```cpp
#include "MainWindow.h"
#include "ui_MainWindow.h"

MainWindow::MainWindow(QWidget *parent)
    : QMainWindow(parent)
    , ui(new Ui::MainWindow)
{
    ui->setupUi(this);

    networkManager = new QNetworkAccessManager(this);
    connect(networkManager,&QNetworkAccessManager::finished,this,&MainWindow::onReplied);

    getWeatherInfo("101010100");
}

MainWindow::~MainWindow()
{
    delete ui;
}

void MainWindow::getWeatherInfo(QString cityCode)
{
    QUrl url("http://t.weather.itboy.net/api/weather/city/"+cityCode);
    networkManager->get(QNetworkRequest(url));
}

void MainWindow::onReplied(QNetworkReply *reply)
{
    qDebug() << "request successfully!!!";

    int statusCode = reply->attribute(QNetworkRequest::HttpStatusCodeAttribute).toInt();

    qDebug() << "status code：" << statusCode;
    qDebug() << "url：" << reply->url();
    qDebug() << "operation：" << reply->operation();
    qDebug() << "raw header：" << reply->rawHeaderList();

    if(reply->error()!=QNetworkReply::NoError||statusCode!=200){
        qDebug() << reply->errorString().toLatin1().data();
        QMessageBox::critical(this,"错误","无法请求接口，请检查网络是否连接正确",QMessageBox::Ok);
    }else{
        QByteArray buff = reply->readAll();
        qDebug() << buff.data();
    }

    reply->deleteLater();
}


```

<br>
