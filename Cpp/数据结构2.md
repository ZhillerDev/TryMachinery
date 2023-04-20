### 字符串操作与 auto 修饰符

```cpp
#include<bits/stdc++.h>
using namespace std;


// string初始化术语
string s1;
string s2(s1);		// s2是s1的一个副本
string s3("hello"); // s3是字符串字面量hello的副本，他的含义和下面一样
string s4="hello";


void cp5_1(){
	string s1="helloworld"; // 拷贝初始化
	string s2("helloworld");// 直接初始化
	string s3(10,'c'); 		// 直接初始化，内容为十个c，也就是cccccccccc
}


void cp5_2(){
	string s1;
	cin >> s1; // 读取字符串，忽略开头所有空格并在读入字符后遇到的第一个空白结束
}


void cp5_3(){
	string s1;
	while(getline(cin,s1)){	// getline读取一行字符，cin表示输入，s1为存储字符的变量
		cout << s1 << endl;
	}

	if(s1.empty()) cout << "null"; // empty检查是否为空
	if(s1.size()!=0) cout << s1;   // size返回长度

	size_t st = s1.size(); // size返回一个size_t的无符号整数，故如此存储
}


void cp5_4(){
	string s1="hello";
	string s2="helloworld";
	string s3="hey";

	if(s2>s1) cout << "yes!" << endl; // 若俩string开头一样且一个较短，则较短的一方偏小

	if(s3>s2) cout << "not" << endl; // 此时比较的是第一个不相等字符的ASCII码谁的更大
}


void cp5_5(){
	string s1="hey",s2="tom";
	string s3=s1+"john"; // 俩子串相加时，字符串字面量左右至少有一个变量
	// 譬如"hello"+"tom"错误，因为两者左右都没有一个string变量

	string s4=(s1+"not")+"think"; // 完美解决问题
}


// cctype头文件函数简析（需要提前导入，这里用了万能头文件）
void cp5_6(){
	cout << isalnum('a') << endl; // 是否为数字或字母
	cout << isalpha('1') << endl; // 是否为字母
	cout << isdigit('3') << endl; // 是否为数字
	cout << isspace(' ') << endl; // 是否空白
	cout << islower('a') << isupper('B') << endl; // 判断大小写字母
	cout << ispunct('.') << endl; // 是否为标点符号
}


void cp5_7(){
	// cpp11新出炉foreach方法，遍历str内的每一个字符并赋给c，并输出c
	string str("helloworld");
	for(auto c:str){
		cout << c << "\t";
	}
	cout << endl;

	// 遍历改变字符串内容，需要使用引用！
	for(auto &c:str){
		c=toupper(c);
		cout << c << "\t";
	}
	cout << endl;
}
```

<br>

### vector

```cpp
#include<bits/stdc++.h>
#include<vector> //使用vector必备的头文件
using namespace std;


// vector也成为容器，它存储对象，但vector本身不是一个类型，且不存在vector的引用
vector<double> cp1;		//定义一个vector，其中存储的元素都是double类型的
vector<vector<int> > cp2; //vector进行类型嵌套时注意最后尖括号的空格，老编译器若无空格会报错


vector<int> cp3_1;
vector<int> cp3_2 = cp3_1; //俩类型相同的vector可以直接进行副本拷贝


vector<string> s1{"abc","hello","boy"}; //列表初始化，用的花括号且无等号
vector<string> s2(10,"hey"); //初始化10个具有相同hey值的string对象


vector<double> d1(10); //若vector使用内置类型，则允许不初始化而直接给定对象数目
vector<double> d2{10}; //上面表示创建10对象，而这里是创建一个值为10的对象，注意区分！


void cp6_1(){
	// 向一个string类型的vector中底部不断插入元素（类似于在栈顶插入）
	string word;
	vector<string> str;
	while(cin >> word){
		str.push_back(word);
	}
}


void cp6_2(){
	vector<unsigned> num(11,0); //将十一个无符号整数对象都赋予初始值0
	cout << num[1] << endl;		//可以直接通过下标访问vector内存储对象的值

	//请勿试图用num[1]=10来赋予元素，这是错的，只能用push_back()函数
}
```

<br>

### 迭代器

```cpp
#include<bits/stdc++.h>
using namespace std;


void cp7_1(){
	// 使用begin来获取字符串指向的第一个元素的迭代器
	// 因为迭代器类型未知，直接auto让系统自动判断迭代器类型
	// 解引用迭代器可以获取其指向的值
	string s("helloworl");
	auto it = s.begin();
	cout << *it <<endl;
}


void cp7_2(){
	// s1.end()获取最后一个字符的后一位的迭代器，所以*(s1-1)=y
	// 当字符串是空的，则begin和end返回同一迭代器
	string s1("some body");
	if(s1.begin()!=s1.end()){	//确保s1非空
		auto it = s1.begin();	//将it指向迭代器的第一个字符
		*it = toupper(*it);		//将当前字符改写成大写的形式
	}
	cout << *(s1.end()-1) <<endl;
}


// 迭代器类型
void cp7_3(){
	// 对于一个非常量，他的迭代器类型格式：数据类型::iterator
	// 该迭代器::iterator可以对内容读写
	string s1("hello");
	string::iterator si1 = s1.begin();

	// 仅可对常量使用const_iterator类型，此迭代器仅可读不可写
	// 注意向量不是一个类型，所以要写vector<string>
	vector<string> s2(10,"str");
	vector<string>::const_iterator sci1 = s2.begin();
}


void cp7_4(){
	vector<int> v1{1,39,221};
	auto v2 = v1.cbegin();	// cbegin()和cend()函数取出的迭代器都是const_iterator类型的
	vector<int>::const_iterator v3 = v1.cend();
}


void cp7_5(){
	// (*s2).empty()检测迭代器指向的内容是否为空，注意括号包住解引用
	// 可以使用箭头表达式来代替指针，如(s2+1)->empty()
	vector<string> s1{"hey"};
	vector<string>::iterator s2 = s1.begin();
	if(!(*s2).empty() && !(s2+1)->empty()){
		cout << "got it!" <<endl;
	}
}


void cp7_6(){
	// 对迭代器使用加法表示返回值前移（但以我们视角事实上是指向迭代器后移）
	// 注意s2+1表示指向hey，而不是在迭代器中的一个对象进行操作！！！
	vector<string> s1{"helloworld","hey","thanks","tombs"};
	auto s2 = s1.begin();
	cout << *s2 << *(s2+1) <<endl;

	// 俩迭代器相减返回顺序差，类型为difference_type，这里用auto判断
	auto s3 = (s2+3)-s2;
}


// 迭代器实现二分查找！！！
void important(){
	vector<int> text(10,0);

	auto beg = text.begin(), end = text.end();
	auto mid = text.begin() + (end - beg)/2; // 初始状态下中间点
}
```

<br>

### 数组迭代器与引用

```cpp
#include<iostream>
#include<cstddef>
using namespace std;


void cp8_1(){
	int c0[10];
	int *c1[10];	// 数组指针，含10个整形指针
	int (*c2)[10]=&c0;	// 指向一个含有10个整数的数组的指针
	int (&c3)[10]=c0;	// 引用一个含有10个整数的数组
}


void cp8_2(){
	// 使用范围遍历的方法取出多维数组所有元素（按照顺序），这里不需要我们手动控制范围
	int a[3][2]={{3,2},{0,9},{34,213}};
	for(auto i:a){
		cout<<a<<" ";
	}
	cout << endl;
}


void cp8_3(){
	// 指针数组可也可以使用迭代器操作
	// 让指针指向一不存在的索引（也就是最后一个元素的后一位），获取数组c1的地址用于初始化
	// 循环解释：让指针b指向数组c1,每次循环指向后移一位，移动到c2（不存在的索引）位置后停下
	// 这种方法十分不安全，建议使用cp8_4内的begin和end做法
	int c1[4]={1,2,3,4};
	int *c2 = &c1[4];
	for(int *b=c1;b!=c2;++b){
		cout << *b << endl; // 借b之手输出c1元素
	}
}


void cp8_4(){
	int c1[4]={1,2,3,4};
	int *c2 = begin(c1);	// 指向数组c1首元素的指针
	int *c3 = end(c1);		// 指向数组c1最后一个元素的下一位的指针
}


void cp8_5(){
	// size_t和ptrdiff_t类型都存在头文件cstddet.c内
	constexpr size_t sz = 5;	// 数组中元素个数类型的size_t
	constexpr ptrdiff_t pt = 4; // 俩数组指针相减结果为ptrdiff_t类型
}


void cp8_6(){
	// 又一种遍历数组元素的方法，c指向数组最后一元素下一位，用作与b一起的判据
	double a[3]={3.44,2.239,9.541};
	double *b=a,*c=a+3;
	while(b<c){
		cout << *b++ << " " ;
	}
	cout<<endl;
}


void cp8_7(){
	// 指针还可以这么玩，p指向a[3]也就是第三个元素，故p[-2]就指向了a的第一个元素
	int a[5]={1,2,3,4,5};
	int *p = &a[2];
	int k = p[-2];	// 此刻的p[-2]取出的是一个值而不是指针，故k不可以使用指针形式
}
```

<br>

### 其余字符串杂项

```cpp
void cp9_1(){
	// 两个string类型可以进行大小比较，但是两个字符数组无法进行比较，因为他们比较的是指针
	string s1="helloworld";
	string s2="hey";
	if(s1>s2) cout<<"yes"<<endl;

	// 这是C风格的字符串
	char c[]={'a','b','\0'}; // 逐个初始化字符数组必须要在最后加一个\0
}


void cp9_2(){
	string s1="nothing";
	const char *str=s1.c_str(); // c_str把string类型转换成一个指针，此时才可用string初始化char*

	// 使用数组的开头和结尾初始化一个int向量（类似于拷贝操作）
	int num[]={1,2,3,4,5,6};
	vector<int> num2(begin(num),end(num));
}
```

<br>
