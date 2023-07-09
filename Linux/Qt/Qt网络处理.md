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

### HTTP
