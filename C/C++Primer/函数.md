### main 形参

```cpp
#include<bits/stdc++.h>
using namespace std;



int main(int argc, char **argv){

	for(int i=0;i<=argc;i++){
		cout<<argv[i]<<endl;
	}

	return 0;
}

/* main内形参详解
   假定我们来到源代码（代码已编译成exe）文件夹下并输入代码CP13 -c -d name
   CP13必须有，表示我们要执行的exe文件的名字，后面几个是附加参数，有3个
   argc表示传入的参数总数，可见这里有4个参数（包括文件名）
   argv则是存参数的数组，argv[0]存文件名"CP13"，以此类推argv[1]存"-c"......
   但是argv会在最后一个元素自动加0表示结尾，故argv[5]=0，所以我们上面的for特地用了i<=argc

*/
```

<br>

### 可变长参数

```cpp
#include<bits/stdc++.h>
#include<initializer_list>
using namespace std;


// initializer_list<string> list表示接收一个可变长的参数，参数类型都是string
// initializer_list可以类似vector一样使用迭代器的方式输出里面的成员
// 下面的代码就用了此方法输出所有成员
// 注意main里面的调用代码，必须有一花括号把传入参数括起来，里面写多少个参数都无所谓，因为可变长
// 事实上可以再加入任意一个形参，此时调用该函数时就需要按照顺序来输入实参了
void cp14_1(initializer_list<string> list){
	for(auto beg=list.begin();beg!=list.end();++beg){
		cout<<*beg<<" ";
	}
	cout<<endl;
}


// void函数里面仍然可以有return，但是后面不能接除了void类型的任何返回值
// 我们可以在任意一个判断或者循环条件中使用return达到退出函数的效果
// 注意，对于有返回值的函数，仅在for里面写return是不够的，必须要在函数最末尾加上return!!!
void cp14_2(){
	for(int i=0;i<10;i++){
		if(i==4) return;
	}
	return;
}


void cp14_3(){

}


int main(){
	cp14_1({"hello","my","dear","cpp"});
	return 0;
}
```

<br>

### 形参引用与指针

```cpp
#include<bits/stdc++.h>
using namespace std;


void cp15_1(const int a){
	// 当形参存在顶层const时，编译器会自动忽略掉形参的const
	// 意思就是我们可以为形参传递一个const int或者int类型的实参进去而不报错
	// 所以我们没法在写一个void cp15_1(int a)来重载，因为系统会自动去掉顶层const，导致俩函数形参一致
}


// 我们可以使用标记确定数组的长度
// 下面的判断表示遇到空字符后停止输出，这样就可判别字符串中有多少个字符
void cp15_2(const char* c){
	if(c)
		while(*c)
			cout<<*c++<<endl;
}
// 传递数组的首指针和尾后元素指针来确定数组长度
void cp15_3(const int* a,const int*b){
	while(a!=b){
		cout<<*a++<<endl;
	}
}


// 定义一返回值为引用的函数
// 这样就避免了对string对象的任何拷贝，这里全程仅传递并使用引用来处理所有事情
const string &cp15_4(const string &s1,const string &s2){
	return s1.size()>=s2.size() ? s1 : s2;
}

```

<br>

### 尾置返回类型

```cpp
#include<bits/stdc++.h>
using namespace std;


void cp16_1(){
	// 定义一个数组指针parr，他指向一个含有10个元素的数组
	int arr[10];
	int (*parr)[10] = &arr;
}


// cpp11尾置返回类型，用来声明复杂函数（这里使用返回值为数组指针的函数作为示范）
// 小箭头后面写返回值类型，前面写函数名字和形参，最前面的auto是为了适配小箭头后面的数组指针的
// 第一行和第二行（传统表达法）含义完全一致，但是第二行真的太难理解了
auto cp16_2(int i) -> int(*)[10];
int (*cp16_2(int i))[10];


// 若已经确定函数要返回那个数组指针，则可以使用decltype来确定返回值
// 因为decltype(odd)表示：含义5个元素的int类型数组，故后面还要加*表示这是数组指针
int odd[]={1,2,3,4,5};
int badly[]={6,7,8,9,0};
decltype(odd) *cp16_3(int i){
	return (i%2) ? &odd : &badly;
}

```

<br>

### 函数重载

```cpp
#include<bits/stdc++.h>
using namespace std;


// 重载函数必须要形参数目或者类型不一致
// 但返回类型不同但形参完全一致是没办法重载的！
void cp17_1(double a){}
void cp17_1(int a){}


// 前情提要：一个声明顶层const和一个没声明顶层const是无法区分的，因为系统编译时自动去除const
// 所以我们不能再写void cp17_2(int i){}
void cp17_2(const int i){}


// 但给形参加了指针或引用后，再加const就变成底层的了，这时完全可以重载
// 我们可以给一被const修饰的形参传入非常量，他会自动转换类型
void cp17_3(const int& a){}
void cp17_3(int &a){}

```

<br>

### 形参默认值与 constexpr

```cpp
#include<bits/stdc++.h>
using namespace std;

// 使用：类型 变量名=值 来设置默认值
// 不能仅对前面赋予默认值而不对后面的值赋予默认值，所以去掉第一行只保留第二行就报错
// 因最后一个参已被第一行赋初始值，所以第二行赋初始值绝对不可以对他重复定义
void cp18_1(int,double,char c='a');
void cp18_1(int a=1,double b=1.43,char);


/*  constexpr函数很特殊
	1. 当他修饰一个函数时，函数体内有且仅有一个return语句
	2. 被修饰的函数的形参、返回值、return内的所有内容都必须是字面量形式
	3. constexpr函数隐式定义为内联函数
*/
constexpr size_t cnt(size_t st){
	return st*100;
}
constexpr size_t s = cnt(16);



int main(){

	// 直接使用函数的所有默认值
	cp18_1();
	// 如果我们需要修改最后一个形参的默认值，必须要把前面的所有参数都填上不能留空！
	cp18_1(1,4.2,'k');

	return 0;
}

void cp18_1(int a,double b,char c){}
```

<br>
