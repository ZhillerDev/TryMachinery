### cerr&clog

```cpp
#include<bits/stdc++.h>
using namespace std;

void outp(){

	// cerr和clog分别输出错误信息和一般性信息
	cerr << "这是一条标准的错误信息" << endl;
	clog << "这是一条普通的信息" << endl;
}


void cp1_2(){
	// 当cin读取到一文档结束符或错误字符时就会停止输入，以此方法读取变长度内容
	// 但此方法并不实用，因为每一个数字都必须空格分开且最后按enter可能也无反应
	int sum=0,value=0;
	while(cin >>value){
		sum += value;
	}
	cout << "求和值为:" << sum << endl;
}
```

<br>

### 引用与地址

```cpp
void cp2_1(){
	bool b1 = (bool)100; //非bool值赋bool，若非0则bool值为true，反之若为0则bool值为false
}


void cp2_2(){
	// cout里面字符串字面量太长了直接换行也是OK的
	cout << "this is a \\n sign that "
			"can be change" << endl;
}


int i; // 定义于函数体外的变量不初始化也会赋予初始值
void cp2_3(){
	int ii=0; //函数体内变量必须初始化，否则它会变成未定义的
}


void cp2_4(){
	// 引用可以理解为为某变量取了别名
	// 一个引用只能指向一个目标，也就是说不能指向一个引用的引用
	// 引用不是对象，譬如c可以完美代替a使用
	int a=1,b=2;
	int &c = a;
	printf("%d",c);
}


void cp2_5(){
	// 在c++中，int叫做数据类型，而*和&叫做类型修饰符
	// 一个类型修饰符只能作用于一个变量，所以p2是int类型而非int*类型！！！
	int *p1,p2;
	int *p3,*p4;
}


void cp2_6(){

	// 说白了，对指针的引用就是对指针取了个别名而已，和普通的&作用是一样的
	int i=42;
	int *p;
	int *&r = p; //从右往左读，定义一个r引用，且其为一个指针

	r = &i; //因为r指向指针p，所以翻译过来就是p=&i，也就是让指针p指向变量i
	*r = 0; //同理，翻译过来*p=0，也就是把p指向的变量i的值变成0

}
```

<br>

### 常量与使用规范

```cpp
#include<bits/stdc++.h>
using namespace std;


// 声明常变量必须要初始化不然报错
const int a = 100;
// 由于常量默认只在一个文件中生效，而加了extern则可以在多个文件中引用
extern const int b = 100;


void cp3_1(){
	// const int &b表示对常量的引用
	// 对常量的引用应该指向常量，且非常量引用不可以指向一个常量！
	const int a = 10;
	const int &b = a;

	// 对常量的引用仍然可以指向一个非常量，但是不允许通过ii来改变i
	// 我们可以直接改变i的值从而间接改变ii指向的值，这是合法的
	int i=10;
	const int &ii=i;
}


void cp3_2(){
	// 要存放常量对象的地址就必须用指向常量的指针
	// 普通指针无法指向一个常量
	const double i=12.33;
	const double *ii = &i;

	// 事实上，指向常量的指针也可以指向非常量，语法正确
}


void cp3_3(){
	// 常量指针的作用就是无法修改他的值
	int outter=0;
	int *const k = &outter;

	// 声明一个指向常量的常量指针（指向改不了，内容也改不了）
	const int *const ll = &outter;
}


void cp3_4(){
	// constexpr关键词修饰后，改变量就变成了一个常量
	// 该关键词就是为了让编译器验证变量的值是否是一个常量表达式
	constexpr int mf = 100;
}


int main(){


	return 0;
}


/*	写在后面
	我们把指向常量的指针称为顶层const，把常量指针称为底层const
*/
```

<br>

### typedef & decltype

```cpp
#include<bits/stdc++.h>
using namespace std;


void cp4_1(){
	// cpp11引入了别名声明，也就是using那玩意，他和typedef等价替换
	typedef double DB;
	using DBB = double;

	// cpp11新增的auto可以自动推演数据类型，但别用在顶层const和底层const的命名上
	auto a = 100;

}


int foo(){return 0;}
void cp4_2(){
	// decltype可以获取并返回某函数的返回值类型，但是并不调用该函数
	// 注意这里使用了外部函数，不然本函数内无法识别
	extern int foo();
	decltype(foo()) num = 1234;


	// decltype函数还可识别一个变量亦或是一个表达式的最终数据类型
	// 如i+100得出结果类型为int
	// 而ii是一个引用类型，也就是int&，所以这一行代码必须初始化！！！
	int i=0,&ii=i;
	decltype(i+100) p1;
	decltype(ii) p2 = p1;

	// 特别的，当再用一个括号把变量括起来时，变量含义立刻转变为引用，所以这里类型为int&
	int a1=0,&a2=a1;
	decltype((a1)) a3 = a1;
}

int main(){


	return 0;
}
```

<br>
