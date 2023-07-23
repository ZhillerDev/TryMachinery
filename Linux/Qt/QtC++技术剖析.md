## 类模板特化

<br>

### 类模板特化

下面是一个标准的类模板，他没有使用任何的模板特化技术  
故无论模板参数 T 是什么类型，Stack 都具有相同的行为

```cpp
template <typename T>
class Stack {
private:
    std::vector<T> elems;
...
}
```

<br>

类模板特化有两种形式：

1. 完全类模板特化
2. 部分类模板特化

<br>

#### 完全特化

定义：指为所有模板参数都确定的情况下，为某个特定类型提供一个单独的实现

下方代码表示当模板参数为特定的 `std::string` 时，使用序号 2 后面的所有代码来对该类进行实例化

全特化要求

- template 内容应置空
- class 定义需要特化的数据类型
- 序号三填入对特化的数据类型需要执行的所有操作

```cpp
template<>                    ①
class Stack<std::string> {   ②
private:
    std::deque<std::string> elems;  ③
public:
    void push(std::string const & elem) {
        elems.push_back(elem);
    }
    void pop(){
        if (elems.empty()) return;
        elems.pop_back();
    }
    std::string top() const{
        if (elems.empty()) return NULL;
        return elems.back();
    }
}
```

<br>

#### 部分特化

定义：为一部分模板参数确定的情况下，为某个特定类型提供一个单独的实现

下方代码表示当我们传入的模板参数 `T` 都具有 `T*` 特征时，使用以下代码执行类实例化

```cpp
template <typename T>        ①
class Stack<T*>  {            ②
private:
    std::list<T*> list;     ③
public:
    void push(T* & elem) {
    list.push_front(elem);
    }
    void pop(){
    if (list.empty()) return;
    list.pop_front();
    }
    T* top() const{
    if (list.empty()) return NULL;
    return list.front();
    }
}
```

<br>

#### 特化模板

假设当前有一个主类模板

```cpp
template <typename T1, typename T2>
class MyClass {
…
};
```

那么对应的特化类模板可以这么写  
表示接收到的第二个模板参数 T2 需为 int 类型的

```cpp
template <typename T>
class MyClass<T,int> {
…
};
```

<br>

比如还有部分特化，可以参考以下代码

```cpp
template <typename T1, typename T2>
class MyClass<T1*,T2*> {
…
};
```

<br>

### Traits 技术

定义：Traits 技术是一种编程技术，它在面向对象编程中用于实现代码重用和可组合性。Traits 技术可以让程序员定义一些可复用的组件，这些组件可以被多个类或对象使用，并且可以在运行时进行组合

下面介绍使用 `traits` 技术的一个简单案例

<br>

```cpp
// 声明一个模板类 fp_traits，用于实现浮点类型的特性
template <typename numT>
struct fp_traits { };

// 模板特化，指定 float 类型的 fp_traits
template<>
struct fp_traits<float> {
    typedef float fp_type;   // 定义 typedef 为 float 类型
    enum { max_exponent = FLT_MAX_EXP };  // 定义枚举常量，表示 float 类型的最大指数
    static inline fp_type epsilon()      // 定义 inline 函数，返回 float 类型的极小值
    { return FLT_EPSILON; }
};

// 模板特化，指定 double 类型的 fp_traits
template<>
struct fp_traits<double> {
    typedef double fp_type;  // 定义 typedef 为 double 类型
    enum { max_exponent = DBL_MAX_EXP };  // 定义枚举常量，表示 double 类型的最大指数
    static inline fp_type epsilon()      // 定义 inline 函数，返回 double 类型的极小值
    { return DBL_EPSILON; }
};

// 声明模板类 matrix，使用 numT 模板参数表示矩阵中的数值类型
template <typename numT>
class matrix {
public:
    typedef numT num_type;   // 定义 typedef 为 numT 类型
    typedef fp_traits<num_type> num_type_info;  // 定义 typedef 为 fp_traits<num_type> 类型
    // 定义 inline 函数，返回 num_type_info 中的 epsilon() 函数结果
    inline num_type epsilon()
    {return num_type_info::epsilon();}
    ......
};

int main()
{
    matrix <float>  fm;     // 定义一个 float 类型的矩阵对象
    matrix <double> dm;    // 定义一个 double 类型的矩阵对象
    cout << "float  matrix: " << fm.epsilon() << endl;   // 输出 float 类型矩阵对象的 epsilon() 函数结果
    cout << "double matrix: " << dm.epsilon() << endl;   // 输出 double 类型矩阵对象的 epsilon() 函数结果
}
```

<br>

### 类型分类

定义：由于 cpp 自带的类型处理方法很少，所以使用类模板特化技术，设计专门的类模板来提供所需信息的方法称为类型分类技术

```cpp
// 类型模板，用于为指定类型 T 提供类型信息
template<typename T>
class TypeInfo {
public:
    enum { IsPtrT = 0, IsRefT = 0, IsArrayT = 0 };   // 定义三个枚举常量，表示 T 不是指针、不是引用、不是数组
    typedef T baseT;    // 定义 typedef 为 T 类型
    typedef T bottomT;  // 定义 typedef 为 T 类型
};

// 类型模板特化，用于为指针类型 T* 提供类型信息
template<typename T>
class TypeInfo<T*> {
public:
    enum { IsPtrT = 1, IsRefT = 0, IsArrayT = 0 };   // 定义三个枚举常量，表示 T* 是指针、不是引用、不是数组
    typedef T baseT;    // 定义 typedef 为 T 类型
    typedef typename TypeInfo<T>::bottomT bottomT;        // 定义 typedef 为 TypeInfo<T>::bottomT 类型
};

// 类型模板特化，用于为引用类型 T& 提供类型信息
template<typename T>
class TypeInfo<T&> {
public:
    enum { IsPtrT = 0, IsRefT = 1, IsArrayT = 0 };   // 定义三个枚举常量，表示 T& 不是指针、是引用、不是数组
    typedef T baseT;    // 定义 typedef 为 T 类型
    typedef typename TypeInfo<T>::bottomT bottomT;        // 定义 typedef 为 TypeInfo<T>::bottomT 类型
};

// 类型模板特化，用于为大小为 N 的数组类型 T[N] 提供类型信息
template<typename T, size_t N>
class TypeInfo <T[N]> {
public:
    enum { IsPtrT = 0, IsRefT = 0, IsArrayT = 1 };   // 定义三个枚举常量，表示 T[N] 不是指针、不是引用、是数组
    typedef T baseT;    // 定义 typedef 为 T 类型
    typedef typename TypeInfo<T>::bottomT bottomT;        // 定义 typedef 为 TypeInfo<T>::bottomT 类型
};
```

<br>

### 降低代码膨胀

定义：每当我们使用一个类型作为模板参数来实例化一个类模板或者函数模板时，C++编译器将生成一个和该类型对应的类或者函数，这会导致每次实例化都重复生成一次，造成代码膨胀

示例

```cpp
// 定义一个 Vector 类型的对象 VPVector，元素类型为 void*，即指向任意类型的指针
Vector<void*> VPVector;

// 定义一个模板类 Vector<T*>，其中 T 为指针类型，继承自 VPVector
template<class T>
class Vector <T*>: public VPVector{
public:
    // 重载 [] 运算符，返回类型为 T* 的指针
    T*& operator[](int i) {
        return (T*&)(VPVector::operator[](i));
    };
};

int main()
{
    // 定义一个 Vector<int*> 类型的对象 v1，用于存储 int 类型的指针
    Vector<int*>  v1;

    // 定义一个 Vector<double*> 类型的对象 v2，用于存储 double 类型的指针
    Vector<double*> v2;

    // 定义一个 int 类型的变量 i 并初始化为 3，将 i 的地址赋给 v1 中第一个元素
    int   i = 3;
    v1[0] = &i;

    // 定义一个 double 类型的变量 d 并初始化为 3.14，将 d 的地址赋给 v2 中第一个元素
    double d = 3.14;
    v2[0] = &d;
}
```

<br>

## 标准库与 QT 字符串处理

<br>

### 标准库类模板

C++使用类模板 basic_string 来存放字符串数据，并提供了一组成员函数来处理该字符串

basic_string 还具有一个明显的优势：自动内存管理功能

<br>

#### 模板参数与构造函数

类 string 及 wstring 是 basic_string 特化的结果

```cpp
typedef  basic_string<char>  string;
typedef  basic_string<wchar_t>  wstring;
```

basic_string 常见的构造函数及其使用方法

对于 `wchar_t` 类型的 basic_string，最好在字符串常量的前面加上前缀`“L”`

```cpp
#include <string>
#include <iostream>
using namespace std;
int main()
{
    basic_string<char> s1;
    basic_string<char> s2 ("hello world");
    basic_string<char> s3 ("hello world", 5);
    basic_string<char> s4 ( s2, 6, 5);
    cout << s1 << endl << s2 << endl << s3 << endl << s4 << endl;

    // 加了L前缀的字符串
    basic_string<wchar_t> ws (L"得友难失友易(A friend is easier lost than
    found)");
    wcout.imbue(locale("chs"));
    wcout << "size is: " << sizeof(ws[7]) << endl << ws << endl;
}
```

<br>

#### basic_string 的其他成员函数

`c_str()` 该函数返回一个指针，指向 basic_string 中存放的字符串数据  
对于 `string` 类型，该函数返回`char *`;  
对于 `wstring` 类型，该函数返回`wchar_t *`;

basic_string 的类模板存在一个专门存放字符串长度的成员变量，通过 length()直接获取字符串长度，可以相对于 strlen()方法获取长度节省很多时间

<br>

### QString

QString 能对 Unicode 字符串进行拼接、查找等操作

不建议使用 basic_string 中的 wstring 替换掉 QString，因为前者对于标点符号判断等其他操作支持较差，而后者封装完善便于拿来即用

<br>

#### QString 特性

利用 `fromLocal8Bit()`，将该字符串转换为 Unicode 编码的字符

QString 的成员函数 `data()` 返回一个指针，指向 QChar 序列

QString 自带的字符编码转换功能展示：

```cpp
// 定义 main 函数
int main()
{
    // 定义一个 char* 类型的字符串 humor，包含了英文和中文字符
    char * humor = "Your future depends on your dream. So go to sleep.\n"
                    "你的梦想决定你的未来，所以睡觉去吧。";

    // 将 humor 转换为 QString 类型的字符串 qs，使用 fromLocal8Bit 方法将 humor 中的字符转换为本地编码
    QString qs = QString::fromLocal8Bit(humor);

    // 将 qs 转换为 QByteArray 类型的数据 data，使用 toUtf8 方法将 qs 中的字符转换为 UTF-8 编码
    QByteArray data = qs.toUtf8();

    // 打开文件 utf8.txt，以二进制模式写入数据
    ofstream of("utf8.txt", ios::binary);

    // 将 data 中的数据写入文件，使用 data.data() 获取数据指针，data.length() 获取数据长度
    of.write(data.data(), data.length());
}
```

<br>

#### QByteArray

类 `QByteArray` 被用来存放长度可变的字节序列，其中的字节序列被存放在一个地址连续的内存块中

该类总会在字节序列的末尾添加一个额外的`“\0”`字符

<br>

## 国际化与区域文化

<br>

### 区域文化

C++标准库提供了类 `locale` 以及 `facet` ，分别用来描述某个区域的区域文化以及文化刻面

所有流对象都会共享一个全局的 locale 对象

一般的，开发者不需要知道 facet 的原理，直接使用 locale 即可实现区域文焕转换设定  
下方代码展示了使用 cout 输出区域文化为德国的文本

```cpp
#include <iostream>
#include <locale>
using namespace std;
int main( )
{
    double x = 1234567.123456;
    cout.imbue( locale( "German_germany" ) );①
    cout << fixed << x << endl;
}
```

<br>

### facet

每个 facet 子类描述某个文化刻面。C++标准库定义了 12 个常用的 facet 子类，来刻画时间、货币等表达方式

facet 对象不能够单独存在，它们必须隶属于某个 locale 对象  
通过调用函数模板 `use_facet<>`可以获得一个 locale 对象中某个 facet 对象的引用

<br>

#### numpunct

`numpunct` 是 facet 的一个子类，可以通过 `use_facet` 获取 facet 对象后对其实例化得到 numpunct 对象

```cpp
#include <locale>
#include <iostream>

// 定义 main 函数
int main()
{
    // 创建一个 locale 对象 os_locale，使用默认本地化设置
    locale os_locale("");

    // 获取 os_locale 的数字标点符号，使用 use_facet 方法获取 numpunct<char> 类型的 facet 对象
    const numpunct<char>& np = use_facet<numpunct<char>>(os_locale);

    // 输出数字标点符号的信息，包括小数点、千位分隔符、true/false 字符串等
    std::cout << "number punctuation of " << os_locale.name() << std::endl;
    std::cout << np.decimal_point() << " "
              << np.thousands_sep() << " "
              << np.falsename() << " " << np.truename() << std::endl;

    // 获取数字分组信息，输出每个分组的长度
    std::string group = np.grouping();
    for (int i = 0; i < group.length(); i++) {
        std::cout << i << "th grouping is: " << (int)group[i] << std::endl;
    }
}
```

<br>

#### time_get

类模板 `time_get` 对一个表示日期和时间的字符序列进行解析，将其中的信息存放在结构体 tm 中

类模板 time_get 的基本使用方法

```cpp
#include <iostream>
#include <sstream>
#include <locale>

// 定义 main 函数
int main()
{
    // 创建一个 locale 对象 loc，使用默认本地化设置
    locale loc;

    // 获取 loc 的 time_get facet 对象 tg，用于解析时间信息
    const time_get<char>& tg = use_facet<time_get<char>>(loc);

    // 获取日期的顺序信息，使用 date_order() 方法获取日期的顺序，返回值为整数
    int idx = tg.date_order();

    // 输出本地化设置的名称和日期的顺序信息，message 数组用于将整数转换为字符串
    char *message[] = {"no_order", "dmy", "mdy", "ymd", "ydm"};
    std::cout << loc.name() << std::endl << "date order " << message[idx] << std::endl;

    // 解析时间信息，使用 istringstream 类创建一个输入流 iss，包含了要解析的时间字符串 "10:26:00"
    std::ios::iostate state = 0;
    std::istringstream iss("10:26:00");

    // 创建一个 istreambuf_iterator 对象 itbegin，指向 iss 的开头
    std::istreambuf_iterator<char> itbegin(iss);

    // 创建一个 istreambuf_iterator 对象 itend，指向 iss 的结尾
    std::istreambuf_iterator<char> itend;

    // 创建一个 tm 结构体对象 time，用于存储解析后的时间信息
    std::tm time;

    // 使用 time_get 的 get_time 方法解析时间信息，将结果存储在 time 中
    tg.get_time(itbegin, itend, iss, state, &time);

    // 输出解析后的时间信息
    std::cout << time.tm_hour << ":" << time.tm_min << ":" << time.tm_sec << std::endl;
}
```

<br>

#### time_put

类模板 `time_put` 将存放在结构体 tm 中的日期和时间信息以某种特定的格式转化为一个字符序列

使用场景较少，不浪费过多笔墨介绍该类模板

<br>

#### codecvt

类模板 `codecvt` 将某一种编码的字符串转换为另外一种编码的字符串

此为对应模板以及三个模板参数的含义

- internT 表示一个字符串在计算机内部的编码类型
- externT 表示该字符串在外部存储设备（比如文件系统）中的编码类型
- stateT 是一个表示转换状态的类型

`template <class internT,class externT,class stateT> codecvt`

<br>

类模板 codecvt 的成员函数 in 的功能

```cpp
#include <iostream>
#include <locale>

// 定义 main 函数
int main()
{
    // 定义 codecvt 类型的别名 cvt_type
    typedef codecvt<wchar_t, char, mbstate_t> cvt_type;

    // 创建一个 locale 对象 loc，使用默认本地化设置
    locale loc;

    // 获取 loc 的 codecvt facet 对象 cvt，用于字符编码转换
    const cvt_type& cvt = use_facet<cvt_type>(loc);

    // 定义字符数组，src 为待转换的字符串，dst 为存储转换结果的字符串
    const int size = 20;
    const char* p1, src[size] = "hello world!";
    wchar_t* p2, dst[size];

    // 定义 mbstate_t 对象 state，用于存储转换过程中的状态信息
    mbstate_t state;

    // 使用 cvt.in() 方法将 src 中的字符转换为 dst 中的宽字符
    cvt_type::result result = cvt.in(state,
                                     src, src + size, p1,
                                     dst, dst + size, p2);

    // 如果转换成功，输出转换结果
    if (result == cvt_type::ok) {
        std::wcout << dst << std::endl;
    }

    return 0;
}
```

<br>

## iostream

<br>
