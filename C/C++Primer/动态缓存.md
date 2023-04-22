### 智能指针 shared_ptr

```cpp
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
#include<set>
#include<string>
#include<stack>
#include<queue>

#include<memory>
using namespace std;

// shared_ptr智能指针，它类似vector，尖括号内为模板，它存在于头文件memory里面


// 创建一个智能指针shared_ptr，他指向一个string对象
// s1检测指针是否为空，而s1->empty()检测指针所指对象是否为空，使用解引用来赋值，因为是指针！
// 下面if内表示：若指针非空且包含一元素，那么将该元素赋值为helloworld
void sp1() {
    shared_ptr<string> s1;

    if (s1 && s1->empty()) *s1 = "helloworld";
}



// make_shared根据指定类型初始化并返回一个shared_ptr，它常用来初始化shared_ptr
// 注意到make_shared后面尖括号和shared_ptr是一致的！
// 常用auto代替复杂的类型指定
void sp2() {
    shared_ptr<int> si = make_shared<int>(123);

    auto p6 = make_shared<vector<string>>();
}



// shared_ptr对象可以被多次引用，每次被引用该对象自身计数器+1
// 当shared_ptr对象是局部变量且函数结束后内存会在计数器==1时自动释放（因为>1表示还有东西引用它）
// 只要计数器不等于0，智能指针就不会自动释放内存并销毁
void sp3() {

}
```

<br>

### new 内存分配

```cpp
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
#include<set>
#include<string>
#include<stack>
#include<queue>

#include<new>
#include<memory>
using namespace std;

// 直接管理内存


// new出的是匿名对象，你可以不传参从而使用默认初始化，比如第一行
// 必须要用指针，来指向匿名对象所分配好的内存
// 对于vector这种类型的，也可以直接使用列表初始化的方法
void n1(){
	int *i1 = new int;
	int *i2 = new int(10);

	vector<int> *vec = new vector<int>{1,2,3,4,5};
}


// 用new分配const对象是合法的，但必须要进行初始化！
void n2(){
	const int *cn = new const int(123);
}


// bad_alloc和nothrow都定义在头文件new里面
// 电脑内存不足时就会抛出bad_alloc
// 使用定位new，如下方的(nothrow)就表示不抛出bad_alloc错误，而是指定其为空指针，不分配内存
// 如果内存充足，就不触发nothrow，依然开辟正常内存存储
void n3(){
	int *nn = new (nothrow) int;
}


// reset让一个指针重新指向新的对象，仅当指针为shared_ptr类型时才可以使用
// 使用reset后会更新引用计数，必要情况下还会释放指针所指对象
void n4(){
	shared_ptr<int> p(new int(20));
	p.reset(new int(100));
}



// delete释放指针
// 当new后，delete前发生异常，内存无法释放，因为除了函数n5外没有任何东西指向这个内存
void n5(){
	int *p = new int(100);
	delete p;
}
```

<br>

### unique_ptr

```cpp
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
#include<set>
#include<string>
#include<stack>
#include<queue>

#include<memory>
using namespace std;

// unique_ptr解析


// unique_ptr依然在头文件memory里面
// unique_ptr只允许指向一个对象
// 它不支持直接赋值，也不允许直接用圆括号进行拷贝，需要特殊的方法
// 第二行为固定格式，将p1所有权转让给p2，p1.release()表示释放指针，本代码等同拷贝效果
// reset释放引用，第四行将引用指向了p2
void up1(){
	unique_ptr<int> p1(new int(10));

	unique_ptr<int> p2(p1.release());

	unique_ptr<int> p3;
	p3.reset(p2.release());
}
```

<br>

### 动态数组

```cpp
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
#include<set>
#include<string>
#include<stack>
#include<queue>

#include<new>
#include<memory>
using namespace std;


// 动态数组


// 使用方括号定义一个数组，但没有初始化，并且此方法都会返回一个指针
// 方括号后加一对圆括号进行默认初始化，第二行为数组中每一个元素初始化为0
// 方括号内必须是整数且非常量
// 第三行：可以new一个[0]数组，但是无法解引用
void da1(){
	int *pia = new int[10];

	int *npia = new int[10]();

	char *ch = new char[0];
}


// delete [] name 释放数组指针（不要把方括号丢了）
// unique_ptr也可以初始化动态数组，且用release释放它（代替delete）
// unique_ptr指向动态数组时可以使用下标运算符来取出元素
void da2(){
	int *it = new int[10];

	delete [] it;

	unique_ptr<int[]> ui(new int[10]);
	ui.release();

	for(int i=0;i!=10;i++) ui[i]=1;
}


// shared_ptr也可使用动态数组，但是需要lambda函数定义一个自己的删除器
void da3(){
	shared_ptr<int> sp(new int[10], [](int *p) {delete[] p;});
	sp.reset();
}
```

<br>

### allocator

```cpp
#include<iostream>
#include<cstdio>
#include<algorithm>
#include<cstring>
#include<cmath>
#include<set>
#include<string>
#include<stack>
#include<queue>

#include<memory>
using namespace std;

// allocator类 -> 定义于头文件memory中


// allocator也是一个模板，他接受一个类型参数
// 使用allocate函数分配给定数量对象的内存，下面分配5个string内存
void ac1(){
	allocator<string> str;

	auto const ss = str.allocate(10);
}
```

<br>
