### 基本类组成

```cpp
#include<bits/stdc++.h>
using namespace std;


/*	讲解const和this的作用
	1.name()是成员函数声明
	2.this可以简单地理解为“这个类”,在一个类中不可以出现任何名字为this的变量或函数
	3.this原意是指向类类型非常量类型的常量指针，他的类型：cp19_1 *const
	4.name()后加const则表示一个 常量成员函数
	5.常量成员函数表示不能改变调用它的对象的内容
*/
class cp19_1{
	string name() const {return this->getName;}
	string getName;
	void setName();
};

// 编译器初始化顺序：先编译成员声明然后才编译成员函数体
// 所以成员函数体可以任意使用类内其他成员（成员函数体就是void name(){}花括号里面的玩意）
```

<br>

### 构造函数与友元

```cpp
#include<bits/stdc++.h>
using namespace std;


/*	构造函数深入理解
	1.cp20_1()=default表示让编译器生成一个默认构造函数
	2.第二行冒号后面的内容表示为成员变量赋予形参的值,bookName(s)表示将该变量值设置为传入的实参s
	3.构造函数后面冒号的内容叫做 构造函数初始值列表
	4.如果像第三行中bookName()参数置空的话，表示用默认方法初始化bookName的值
*/
class cp20_1{
	cp20_1()=default;
	cp20_1(const string &s, unsigned b):bookName(s),age(b){}
	cp20_1(unsigned c):bookName(),age(c){}

	string bookName;
	unsigned age;
};



// struct也可以替换类，但是它里面的所有成员都是public类型的
// class一般用来定义有private类型的成员



/*	友元
	1.在一个类内对其他类或者函数使用friend声明友元函数
	2.声明friend后，这些其他类和函数就可以使用本类中的private成员或其他成员
	3.friend声明仅定义访问权限，如要使用外部类或函数，则需要再次普通声明一次
	4.友元不能套娃，
*/
class cp20_2{
	friend cp20_2 add(const std::string &s);
};
// 定义一个cp20_2接口的非成员组成部分声明
cp20_2 add(const std::string &s);



// 在cp20_3可以声明友元类cp20_4，表示对类cp20_3开放自己的所有内容
// 也可以只对专门的函数进行友元声明，那么操作权限仅对该类中的该函数有效
class cp20_4{
public:
	void getOut();
};
void cp20_4::getOut(){}

class cp20_3{
	friend class cp20_4;
	friend void cp20_4::getOut();
};


/*	构造函数特性
	1.构造函数可以有多个，也就是所谓重载
	2.构造函数不能被const修饰
	3.构造函数没有返回值！
*/
```

<br>

### 可变成员与作用域

```cpp
#include<bits/stdc++.h>
using namespace std;


// mutable关键词声明的成员叫可变数据成员
// 该成员可以被一const修饰的成员函数调用，但是他自己不可以被声明为const
class cp21_1{
public:
	void getName() const;
private:
	mutable size_t acc;

};
void cp21_1::getName() const{
	acc++;
}


// 名字可以在不同作用域内覆盖，但是类型名不可以，所以不能在类里面再次using Money=xxx
// 类初始化是先声明成员函数然后再到函数体，所以最终return bal中的bal使用Money类型
using Money = double;
class cp21_2{
public:
	Money balance(){return bal;}
private:
	Money bal;
};

```

<br>

### 委托构造函数

```cpp
#include<bits/stdc++.h>
using namespace std;


// C++11新特性：委托构造函数
// 第二个构造函数叫委托构造函数，他委托给默认构造函数，调用默认构造函数执行赋值操作
// 运行构造函数时，先执行被委托对象，执行完后返回执行本构造函数的函数体
// 第三个构造函数先委托第二个，第二个再委托第一个，第一个执行完返回第三个构造函数函数体
class cp22_1{
public:
	cp22_1(string name, int age, double health):name(name),age(age),health(health){}
	cp22_1():cp22_1("",0,0.0){}
	cp22_1(istream &is):cp22_1(){}

private:
	string name;
	int age;
	double health;
};


// 类类型修饰，第一个加括号表示定义函数，第二个不加括号表示定义对象
cp22_1 cp22_1_1();
cp22_1 cp22_1_2;

```

<br>

### 转换构造函数

```cpp
#include<bits/stdc++.h>
using namespace std;

//---------------------------------------------------------------------
// 转换构造函数
// 当一个类构造函数仅一个形参时，可以使用类类型隐式转换机制，该构造函数叫转换构造函数
class cp22_2{
public:
	cp22_2(string name){}
	explicit cp22_2(int num){}
	void getName(){}
};

// 使用直接初始化的方式初始化对象，传入string值，该值成为类构造函数的实参并实例化该对象
// 转换是必须是一步，不可以直接写cp22_2 ex1("abc")，因为系统执行两步转换操作，这是错的！
string name = "python";
cp22_2 ex1(name);

// 加了explicit修饰的构造函数则无法隐式实例化对应对象
// 所以该代码是错的：cp22_2 ex2(int(123))
// 但是显式使用构造函数则可以使用static_cast<...>来初始化
cp22_2 ex2(static_cast<cp22_2>(int(123)));

//---------------------------------------------------------------------
```

<br>

### 聚合类&字面值常量&类内静态成员

```cpp
#include<bits/stdc++.h>
using namespace std;


//---------------------------------------------------------------------
// 聚合类
// 形成条件：全部成员均public，无任何构造函数，无类内初始值，无基类
// 下面定义了一个简单的聚合类
class cp24_1{
public:
	int a;
	string b;
};
// 使用花括号对聚合类进行初始化，括号内值顺序必须和类中声明顺序一致
// 若对某些属性缺省，则自动初始化为默认值
cp24_1 ex1 = {1, "happiness"};
//---------------------------------------------------------------------



//---------------------------------------------------------------------
// 字面值常量类
// 1.情况一：数据类型都是字面值类型的聚合类可以视为字面值常量类
// 2.情况二：大多数情况下，把必须有一constexpr构造函数的，成员全是字面值常量的类叫做字面值常量类
// 3.在第二种情况下，构造函数初始化列表必须全部对数据成员初始化！
// 4.第二种情况下，constexpr构造函数不可以有函数体（初始化列表里面的变量也必须是字面值变量）
class cp24_2{
public:
	constexpr cp24_2(bool a, bool b) : name(a),age(b){}
private:
	bool name;
	bool age;
};
//---------------------------------------------------------------------



//---------------------------------------------------------------------
// 类内静态成员
// 可以在类外部正常写入一个方法，在类内声明该方法时使用static把它变成静态的（不可在外部直接static）
// 类内静态成员不属于该类，所以类在初始化时不会对静态成员赋予初始值
// 实例化对象后用点运算符访问静态成员，或者直接用区域运算符并结合类名直接调用静态成员！！！
class cp24_3{
public:
	static int stic_mth();
};
int cp24_3::stic_mth(){return 0;}

cp24_3 c24;
int a = c24.stic_mth();
int b = cp24_3::stic_mth();


// 静态成员可以作为类内函数的参数默认值出现，但是普通成员绝对不可以
// 静态成员的类型可以直接指定为本类的类类型，但是普通成员只能指定为本类的引用或者指针
// 当且仅当静态成员为字面值常量constexpr类型的，才可以在类内用一const整形数值初始化（就这一种情况！）
class cp24_4{
public:
	cp24_4(char a=bg){}
private:
	static const char bg;
	static cp24_4 cp24;

	static constexpr int period = 40;
};
//---------------------------------------------------------------------


int main(){

	// 输出获取到的内容
	cout<<a<<" "<<b<<endl;
	return 0;
}
```

<br>
