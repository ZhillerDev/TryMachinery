### 一元二元三元运算符

```cpp
#include<bits/stdc++.h>
using namespace std;


void cp10_1(){
	bool b = true; // bool值的true表示1
	bool b2 = -b;  // -b即-1，但是bool不接受负数，故转换为1，所以导致b2仍然为true

	int c1 = 21/6; // 结果为3，因为俩整数相除且结果为整数，直接掐掉小数部分且不四舍五入
	int c2 = 10%1; // 取余当且仅当左右两边都是整数类型才可以使用

	cout << -10/2 << endl; // 除法的结果符号依据正常运算标准，所以这里-10除以2结果为-5
	cout << 10%(-3) << endl; // 取余结果符号依据第一个数字的符号，所以10对-3取余结果为1
}


void cp10_2(){
	int a=10;
	if(a) cout<<"hey"<<endl; // 直接写变量作为判据，当a==0时为假，不等于0是就是真

	int v1,v2;
	v1=v2=0; // 赋值运算符满足右运算律，所以不用怕没有初始化导致错误，这里v1=v2=0
}


void cp10_3(){
	int a=100;
	cout << a++ << " " << ++a << endl;
	/* a++运行机理：
	   先把原来的a值复制一个副本并保存下来
	   之后对a进行自增操作
	   然后返回保存的副本（也就是没有+1的原始值）

	   ++a运行机理：
	   直接a自增后返回自增后的a值
	*/
}


void cp10_4(){
	// 循环输出向量内容的另一种方法
	vector<int> v = {1,2,3,4,5,6,7,8};
	auto num = v.begin();
	while(num!=v.end() && *num>=0){
		cout << *num++ << endl;
	}

	/* 对*num++的解释：
	   因为自增运算符的优先级比解引用符号高，所以可以看出*num++和*(num++)是等价的
	   之后根据之前讲的，a先把原值存成副本再自增后返回副本
	   所以解引用符合解的是num指向的内容而不是num++指向的内容
	   （此方法在简化代码方面及其常用）
	*/
}


void cp10_5(){
	// (*p).size()和p->size()含义一致
	// 如果写成*p.size()就会先对指针取出值，但因为指针不属于值，所以会报错
	string s1 = "not in here", *p=&s1;
	auto u = s1.size();
	u = (*p).size();
	u = p->size();
}


void cp10_6(){
	// 三元运算符也可以嵌套，下面计算了分数值并根据分数的大小从而输出不同的值
	int grade=70;
	string result = (grade>90) ? "excellent" : (grade>60) ? "pass" : "fail";
	cout << result << endl;
}


// 使用static_cast代替（int）进行强转类型可以避免编译器警告，但依然会损失精度
int k = 0;
double c = static_cast<double>(k);

```

<br>

### if 与 for 规范

```cpp
#include<bits/stdc++.h>
using namespace std;

void cp11_1(){
	// 空语句独占一行，表示什么都不做
	for(int i=0;i<10;i++){
		;
	}
}


void cp11_2(){
	// 当if语句下都仅只有一行且不用花括号时就会导致可能存在的“悬垂else”
	// 悬垂else就如下面的else一样可能匹配第一个if或第二个if，但C编译器会按照就近原则，即选第二个if
	// 把所有if语句下加上花括号即可
	int a=10,b=20;
	if(a<b)
		if(a+5>b);
	else
		cout<<"error"<<endl;
}


void cp11_3(){
	// switch可以一连串匹配多个case，除此之外他们还可以写在一行内
	char c='a';
	int num=0;
	switch(c){
		case 'a': case 'b': case 'c':
		num++;break;
		default:
			break;
	}


	// 在某个case里面定义一个语句块，就会把里面的变量作用域限制在该case中，其他case无法使用这些变量
	// 如果不加语句块，在语句判断时会导致跳转到另一case使用该变量但变量未初始化从而导致错误
	bool b=true;
	switch(b){
	case true:
		{
			int k=0;
			k++;
		}
		break;
	case false:
		break;
	}
}


void cp11_4(){
	// goto语句强烈建议不要使用，虽然他也可以作为类似循环来使用，但还是要注意变量初始化问题
	int c=0;
	loop:
		if(++c<10)goto loop;
	cout<<c<<endl;
}

```

<br>

### 错误抛出与异常

```cpp
#include<bits/stdc++.h>
using namespace std;


void cp12_1(){
	// 在try代码块里写入判断条件，并使用throw函数抛出指定异常
	// 每个异常都定义于某头文件内，并且throw该异常是需要传递字符串参数，表示抛出异常的文本
	// catch后面的参数就是接收抛出异常的名字，且必须为该异常指定别名，为接下来的处理做准备
	// err.what()中的what存在于每个异常类型中，他表示需要传入一个字符串用作输出
	int a1=100,a2=200;
	try{
		if(a1!=a2)
			throw runtime_error("a1 is equals a2!!!\n");
	}catch(runtime_error err){
		cout<<err.what()<<"please change the value of them"<<endl;
	}
}


void cp12_2(){
	// 常用的标准异常类定义于stdexcept头文件里面
	// new头文件定义了bad_alloc异常类型
	// type_info头文件定义了bad_cast异常类型

	// 特别注意exception也是一个异常类，但他不允许和其他异常类一样用字符串初始化，仅能默认初始化
}

```

<br>
