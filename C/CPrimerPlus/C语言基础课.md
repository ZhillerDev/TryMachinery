### 前言

此笔记依据 CPrimerPlus 总结得来

<br>

### 基础 C 文件

本节介绍了 C 语言基本文件的组成结构及其对应语句段的实际意义

关注：预处理器、主入口点 main、局部变量定义、格式化输出 printf

```c
#include <stdio.h>
// 这是预处理器，stdio.h是C编译器软件包的标准部分，它提供键盘输入和屏幕输出的支持
// 文件带有后缀.h的叫做头文件

int main(void)
// main()是该C程序的入口点，且int表示main()函数返回的是一个整数
{
   printf("万年helloworld \n");
   // getchar(); 这个叫做等待用户输入某键，添加他后可以使exe文件不会一闪而过
   // \n也就是换行符

   printf("/* 这里面的内容不作为注释 */");
   // 注意，当注释符号放在了一个字符串里面，他就失去了注释的作用

   int nmb;
   nmb = 1;
   // 这里采用了先定义再赋值的方式，这里赋值是整数型的！

   printf("the number is %d \n",nmb);
   // %d是指调用变量，该变量就是printf中写的nmb，而nmb我们之前已经定义为1了，所有显示1

   return 0;
   // 目前，可暂时把该行看作是结束main()函数的要求

}
```

<br>

### 函数原型

函数原型!=函数定义

C 语言编译是按照顺序执行的，即你不可以在函数初始化之前就调用它，除非你定义了一个函数原型

函数原型需要包括函数名、形参类型（可以省略形参名，直接写类型即可）、返回值类型

```c
#include <stdio.h>

void butif(void);
/* 在最开始的这个叫做函数原型（或函数变量），C90新加入，旧的编译器可能导致无效
    该函数原型还指明了函数的返回值类型，这里是void
    函数原型声明表示告诉系统正在使用此函数
*/

int main(void)
{
    printf("下面会执行我新建的一个子函数： \n");
    butif();
    // 任何C程序最先开始执行的永远是main()，函数定义放在main前面还是后面都无任何关系
    // 定义的函数如何出现仅与它是否被调用有关！

    return 0;
}

void butif(void){
    printf("早上好啊孩子们！ \n");
    // 这是函数定义，一般的圆括号内返回值类型应该写上，但老式编辑器无法识别，去掉就好了
}
```

<br>

### scanf

基础 scanf 获取输入

```c
#include <stdio.h>

int main(void)
{
    float weight;
    float value;
    printf("write down your weight:\n");
    scanf("%f",&weight);
    // %f说明scanf函数读取的是一个浮点数值的输入值，%f本义指的就是一个存储浮点值的变量
    // &weight指将获取的输入值赋值给weight变量

    value = 1700.0 * weight * 14.5833;
    printf("god,your weight is value: $%.2f.\n",value);
    // %.2f指存储的浮点值变量精确到小数点后2位


    getchar();
    return 0;
}
```

<br>

### 数据类型

使用 printf 格式化输出时对应的符号

常用的数据类型

```c
#include <stdio.h>

int main(void)
{
    int a = 1, b = 3;
    int c, d = 5;
    // 十分不建议上一行的命名方法，千万不要声明一个而赋值另一个，会让人误解的（即便可以运行）！

    printf("this is %d and %d \n",b);
    // 设置了俩%d但是后者没有指定变量，则会随即输出一个内存里的数据，不会报错

    int x = 10;
    printf("dec=%d; octal=%o; hex=%x \n",x,x,x);
    // %d表示用十进制表示数，%o则是八进制，%x则是十六进制，但并不显示各自进制的前缀
    printf("dec=%d; octal=%#o; hex=%#x \n",x,x,x);
    // 这样子就可以显示各个进制的数字的前缀了！注意对于八进制和十六进制，前面要加#如%#o

    short int si = 123;
    // 该类型占用的存储空间比int小
    long int li = 1000;
    // 就是加长了一点存储内容
    long long int lli = 1000000;
    // 这个就是适用于极大数字的存储了
    unsigned int ui = 11234;
    // 只可表示非负数，且范围是0-65535
    unsigned long long int ulli = 12344;
    // unsigned无符号

    /*  程序会根据Int数值的大小来决定是否为数值的数据类型“升级”
        比如int类型中你填入的数字太大，编译器识别后自动升级为long int
        若数字大的离谱，则有可能使用unsigned long long int
        事实上，上面所有的声明里面都可以去掉Int,都是成立的！
    */

    short asd = 200;
    printf("%hd + %d",asd,asd);
    // %hd表示把int类型的数转换成short类型，但由于数字比较小，并不会出现被截断的现象

    char chara = 'A';
    // 单引号表示字符常量，双引号表示字符串，无引号表示一个变量，不要搞错了
    char chara2 = 65;
    // 由于使用ASCII编码，这么写也可以表示A，但是是一种不好的编码风格

    return 0;
}
```

<br>

科学计数法的表示，以及 double 和 float 那个要加后缀 F 的问题；  
不太常用，但是可以看看

```c
#include <stdio.h>

int main(void)
{
    char ch;
    ch = 'a';
    printf("translate this %c into %d \n",ch,ch);
    // %c表示打印该字符，而%d则会打印出该字符对应的整数值（ASCII码）！

    signed char sc;
    unsigned char uc;
    // 有符号char范围-127~128,无符号char范围0~255

    printf("asd""%""d""\n",ch);
    // 字符串可以直接像上面一样进行连接哦！

    float fl;
    fl = 1.0e9;
    // 以上是c中的科学计数法的表示，1.0e9表示1.0乘以10的九次方
    float fl2 = 1.0e-19;
    // 这表示1.0乘以10的负19次方！

    double db = 1.2E+10;
    long double ldb = .8E+11;
    float fl3 = 4.E-11;
    float fl4 = 2E3;
    float fl5 = 123.22;
    /*  正号可以省略的，如fl4
        可以没有整数部分或者小数部分，但是二者不可以同时省略如ldb,fl3
        浮点型赋的数值里面不可以有任何空格！
        可以没有小数点或者指数部分但不可以同时没有，如fl5与fl4
    */

   float asd = 9.123L;
   // 若为float类型，后面加F或f，但如上加了L那么就强制转换数值类型为long double

   float about = 32000.0;
   double abet = 2.14e11;
   long double addy = 5.32e-4;
   printf("%f can be changed into %e \n",about,about);
   printf("%Lf can be changed into %Le \n",addy,addy);
   // %f指用十进制表述浮点，%e表示用指数形式表示浮点
   //%Lf和%Le作用和上面俩是一样的，但这是专门针对Long double类型使用的

    float toobig = 3.0E38 * 100.0f;
    printf("oh fuck, the number is too big %e \n",toobig);
    // toobig被赋予了一个超过了float可以容纳的范围，输出后C会给予一个INF值，这叫上溢

    float cc1,cc2;
    cc1 = 2.0e20 + 1.0;
    cc2 = cc1 - 2.0e20;
    printf("%f \n",cc2);
    // 由于2.0e20有21位数字，加的1恰好就在21位上，但是float只能存储前6位数，加了一会错误
    // 既然cc1已经错误，则cc2必然会被赋予一个错误的值，所以输出一个错误的结果

    printf("type int has a size of %zd bytes. \n",sizeof(int));
    printf("type double has a size of %zd bytes. \n",
        sizeof(double));
    // sizeof函数可以获取括号内的数据类型的大小
    // %zd是专门给sizeof用返回值的，早期版本编辑器可以使用%u替代
    // 只要不是从单词或者引号之间断开，都可以像最后这条命令一样换行写，但输出结果不会换行

    return 0;
}
```

<br>

变量命名规范以及一些细枝末节的问题

```c
#include <stdio.h>

int main(void)
{

    int cost = 12.99;
    // 可别这么写，会导致数据丢失的！

    int i_asd = 100;
    unsigned long long int ulli_asd = 123;
    // 比如上面这样，int类型的变量名用i_前缀，这是一个好习惯

    printf("\aasd \n");
    // 有些C会把\a解释为警报，但是某些版本会把他显示为一个颠倒的问号

    printf("asd");
    printf("aaadddfff \n");
    // 系统一般是把print内容先放入缓冲区，等缓冲区满了后再发送到屏幕上的
    // 强制缓冲区刷新的方法是用换行符，或者scanf函数

    return 0;
}
```

<br>

### 逻辑与循环

if 判断与 switch

```c
#include <stdio.h>
// 对象的声明和使用
// switch语句的使用

int main(void)
{
    void intPlus(int,int);
    intPlus(543,23);
    // 使用一个对象需要两个步骤，先声明，后使用，一定要这样！！！

    void switchTest(int);
    switchTest(44);
    return 0;
}

void intPlus(int x,int y){
    printf("%d\n",x+y);
    // 特别注意！printf函数不直接接受整数，需要用%d来代替
}

void switchTest(int k){
    switch(k){
        case 5:printf("%d\n",k);
            break;
        case 8:printf("%d\n",k);
            break;
        case 10:printf("%d\n",k);
            break;
        case 44:printf("%d\n",k);
            break;
            // case一旦判定成功可以选择写上break退出判断，或者不写break从而继续判断
        default:printf("cannot find the number");
        // default用于所有case都判断完后才最后到他执行
    }
}
```

<br>

for 循环

```c
#include <stdio.h>
// for语句相关使用注意事项
// for语句和while的替换

int main(void)
{
    void for1();
    for1();

    void for2();
    for2();

    void for3();
    for3();
    return 0;
}

void for1(void){
    for(int i=1;i<=10;i++){
        printf("%d\n",i);
    }
    // for语句内执行顺序：先赋值int i=1后判断i<=10，判断成功后执行语句块内容，执行完后才执行i++

    int k=10;
    for(;k>=5;k--){
        printf("%d\n",k);
    }
    // for可以省略第一个参数，但是必须要在该for前面为即将使用的变量赋值！

}

void for2(void){
    for(int i=1;i<=4;i++){
        if(i==4){
            printf("%d\n",i);
            break;
        }
    }

    int w=1;
    while(w<=4){
        if(w==4){
            printf("%d\n",w);
            break;
        }
        w++;
    }
}

void for3(){
    for(int i=1;i<100;i++){
        if(i==12){
            printf("%d\n",i);
            continue;
            // continue的作用就是立即结束该循环并跳入下一个循环
        }
    }

    for(int w=4;w<6;w++){
        printf("%d\n",w);
    }
    // 注意同一个方法里面for进行变量赋初值时，变量名不可以一样！
}

```

<br>

### 数组

C99 新增的可变长数组特性

```c
#include <stdio.h>

int main(void)
{
    void arry1();
    void arry2();
    void arry3();
    arry1();
    arry2();
    arry3();
    return 0;
}

void arry1(void){
    int a[10];
    // 声明一维数组，含有十个元素，索引0-9不是1-10！！！，没有a[10]这个值
    // 声明不可以包含变量，比如int a[n]是错误的！

    int ar1[10]={5,2,7,2};
    // 可以仅对数组的部分元素赋值

    int ar2[]={3,4,5};
    // 我们可以不用再方括号里指定元素个数，直接赋值个数就是元素个数了！！！

}

void arry2(int n){
    int k[2*n];
    // 这就是一个可变长的数组，按照用户输入的值来确定数组元素的多少
    // 但若数组是静态的就不可以这么做，譬如static int k[2*n]就是错误的
}

void arry3(){
    int ar3[1][3];
    // 这样子定义二维数组，以后还有n维数组......

    int ar4[3][2]={{5},{},{2,3}};
    // 花括号嵌套赋值，注意这是二维数组
    // 有些地方我们可以空出来不写，这也是正确的
    // 没有被赋值的元素直接给0（因为这是整数型数组）
}
```

<br>

字符数组，puts、gets 使用（重点关注，输入输出题提高解题速度可以使用！）

```c
#include <stdio.h>
// 字符数组的使用
// puts和gets函数的使用

int main(void)
{
    void char2();
    void char1();
    char2();
    return 0;
}

void char1(){
    char c1[10];
    c1[0]='K';
    // 这个叫字符数组，可以存储单个字符

    int c2[3];
    c2[0]='l';
    // 由于字符存储都是以ASCII码（数字型）为准，所以也可以用整数型数组存储

    char c3[5]={'C','H','I',' ','A'};
    // 这是字符数组创建方法，空格也算一个字符，

    char c4[10]={"china"};
    // 字符数组也可以接收字符串，并将它转换成字符
    // 这里元素设置了10个，但我们只用了5个，第六个元素用"\0"填充
    // 且每个字符数组的末尾都以\0结尾
}

void char2(){
    char c5[10]={"china"};
    printf("%s\n",c5);
    // 这是打印字符数组的方法，不要直接printf(c5)，这样是错误的
    printf("%s\n",&c5[0]);
    // 因为c5[0]表示数组第一个指针，取其地址返回给%s也可以一连串把字符统统输出完毕

    char str1={"english"};
    puts(str1);
    // puts和printf用法是一样的
    // puts后面括号内的字符数组数目不可以超过一个！所以puts(str1,str2)是错的

    //还有gets();函数，它的功能和scanf也是一样的

    // scanf(%f,str);注意，若变量str是一个字符数组，那么前面就不加&，否则错误
}
```

<br>

### string

平平无奇的字符串操作~

```c
#include <stdio.h>
#include <string.h>
// 基本字符串函数的操作
// 以下的所有字符操作都需要导入string.h这个东西

int main(void)
{
    void str1();
    void str2();
    str2();
    str1();
    return 0;
}

void str1(){
    char s1[10];
    char s11[10]="england";
    char s2[]="china";
    // 字符数组若赋值字符串则可以不加花括号，但是加了比较标准
    strcat(s11,s2);
    // 该函数把s2内容接到s1后面，所以s1必须要有足够大的空间否则会溢出！
    printf("%s\n",s11);

    strcpy(s1,s2);
    // 把s2内容复制到s1里面去，规则和上面一个函数一样（说是替换，实际上是直接覆盖）
    printf("%s\n",s1);

    char s3[]="qq";
    strncpy(s11,s3,2);
    // 把s3前面两个字符替换了s11前面两个字符，不包括\0字符
    printf("%s\n",s11);
}

void str2(){

}
```

<br>

### 变量作用域

```c
#include <stdio.h>
// 变量的作用区域
// 静态变量和动态变量

int main(void)
{

    void name3();
    void kit1();
    void thank();
    thank();
    name3();
    kit1();
    return 0;
}

int name(int x){
    int b,c;
}
int name2(int x){
    int b,c;
}
// 形参和函数内的变量都属于局部变量
// 上面两个不同函数即便里面的局部变量一模一样也互不影响

void name3(){
    int a = 1;
    printf("%d\n",a);
    {
        int a = 100;
        printf("%d\n",a);
    }
    // 上面是一个单独的代码块，里面的局部变量和代码块外的变量互不干扰
}

int Kit = 100;
// 这是全局变量的声明，一般的，会把全局变量名称开头大写
// 所有的全局变量都是静态变量！！！
// 规定：全局变量和所有的静态变量都有自动初始化功能

void kit1(){
    int Kit = 10;
    printf("%d\n",Kit);
    // 在函数内的局部变量可以屏蔽掉全局变量！
}


int f(int x){
    auto int k1 = 1;
    static int k2 = 2;
    register int k3 = 3;
    // auto即自动变量，一般的只要不声明auto，系统也会自己认定其为auto
    // static即静态变量，声明局部变量后无论该函数被调用几次，该变量值永远不会被自动重置
    // register即寄存器变量，可以加快提取速度，很鸡肋基本没用

}

static int cannot = 1;
// 用static修饰的全局变量无法被外部文件调用，只能在本文件里面使用

int kkk = 1;
// 这是一个外部变量，或者说是全局变量
void thank(){
    extern int kkk;
    // 把全局变量导入进来供局部函数使用
    printf("%d\n",kkk);
}

```

<br>

### getchar 猜数游戏

请牢记常见的输入方式，如 `gets getchar getline scanf` 等等

```c
#include <stdio.h>
// 一个简单的猜数游戏

int main(void)
{

    int guess = 1;
    char response;

    printf("please press y or n to determine whether the result of guess is true or false\n");
    while ((response = getchar()) != 'y')
    {
        if(response == 'n')
            printf("is it %d\n",++guess);
        else
            printf("sorry I just learn y or n");
        while (getchar() != 'n')
        {
            continue;
        }

    }

    printf("gods! I have done it fabolous!");


    return 0;
}
```

<br>

### 数组进阶

数组和指针相互结合会碰撞出什么火花呢？

```c
#include <stdio.h>


# define SIZE 4

void num1(void){

    // C99可以指定初始化器，这样子就可以对指定数组元素赋值而不需要从头赋值
    int arr[6] = {[4] = 1};


    // 优秀的程序员会用符号常量来表示指定数组大小，从而方便修改
    int arr2[SIZE];


    // C99后，我们可以指定一个“可以变长的数组”
    int n;
    int arr3[n];

}


void num2(void){

    // arr1与&arr1[0]等价，意味着数组名称直接表示数组第一个元素的地址
    int arr1[2] = {0,3};
    printf("%d\n",arr1);
    printf("%d\n",&arr1[0]);


    // arr1+1 == &arr1[1] 直接对数组进行加法，则可以表示地址指向数组第二个元素
    // *(arr1+1) == arr1[1] 加了*表示转换地址为指向的元素内容

}


void num4(void){

    int arr1[3] = {9,5,3};
    int total = 0;

    // 定义一个指针变量指向数组arr1
    int *start = arr1;

    // 事实上，start+3后会溢出，他将指向数组最末尾元素的后一个元素，但是系统可以自动纠正
    // 这种格式最后一个循环虽然溢出，但是他的地址指向数组最后一个元素
    while (start<start+3)
    {
        total += *start;
        *start++;
    }

}


void num5(void){

    // 指针必须要初始化才可以进行解引用，解引用就是*ar啦
    int arr1[3] = {112,43,12};
    int *ar = arr1;
    *ar = 9.9;

}


int main(void)
{

    // 特别注意，函数num3形参里面有引用和数组，所以在声明函数原型的时候不可以漏掉符号*和[]
    void num3(int *,int []);

    num1();
    num2();


    return 0;
}

void num3(int *arr1, int arr2[]){}
```

<br>

### 指针光速入门

```c
#include <stdio.h>


void const1(const int a[]){
    // 形参中的变量被const修饰后就没办法在函数体里面对他进行修改
    // 也就是我调用它给予的实参赋予后，函数体里面就改不了他的值


    // 无法令一个指针变量指向一个被const修饰的数组，比如double *p = len是错的
    const double len[4] = {0.3,5.4,212.8,95.44};


    // 15定义了一个不能指向别处的指针，注意const的位置，这里指向&rate[0]
    // 按照这个原理，在16行后面写入p1 = &rate[1]就是错误的！
    double rate[10] = {5.0,492.9};
    double * const p1 = rate;


    // 定义一个既不能指向别处也不能指向地址上的值的变量，这里面含有俩const
    // 所以*p2 = 19.0和p2=&rate[3]都是错的！
    const double * const p2 = rate;
}


void array(){

    // 套娃双重间接定理
    // a指向地址为：&a[0]，他包含了两个int值：a[0][0]和a[0][1]
    // a[0]指向的地址为&a[0][0]，它仅包含一个值a[0][0]
    // 所以*a=*&a[0]=&a[0][0]  **a=**&a[0]=*&a[0][0]=a[0][0]
    // **a通过解两次地址从而成功的取到值a[0][0]，这就是双重间接
    // a+1的地址就变成了&a[1]
    int a[3][2];

}


void array2(){

    // 因为[]的结合级别高于*，所以必须要加一个括号，下面是两个例子
    // 下面表示先创建一个数组p并包含两元素，其中每个元素都是一个int类型的指针
    int *p[2];
    // 下面表示创建一个指向数组的指针，该指针包含两个元素
    int (*pp)[2];


    // 指针数组p3创建后，他默认指向zipper[0]和zipper[1]
    // 一旦指针数组指向数组，那他就可以向数组一样调用数组内容，如p3[1][0]==zipper[1][0]
    // 理解完*(*(p3+1)+1)你就出师了
    // 如果zipper[2][3]那就无法将其赋予给指针p3，因为p3仅容得下2元素，这里有3个元素
    // p3容纳的元素数目是以被赋予的数组的最低维度的内容数目为主，比如上面的以[3]而不是[2]为主
    int zipper[2][2] = {{1,2},{5,8}};
    int (*p3)[2] = zipper;
    printf("%p %p %p\n",p3,p3+1,zipper[0],zipper[1]);
    printf("%d %d\n",p3[1][0],zipper[1][0]);
    printf("%d\n",*(*(p3+1)+1));

}


void array3(){

    int un[5] = {100,200,300,400,500};
    int *p1,*p2,*p3;

    p1 = un;        // 赋予p1一个指针，该指针指向un[0]
    p2 = &un[2];    // 同理也给p2一个指针

    // &p1指该指针自己的地址，*p1表示该指针指向的数值，p1表示该指针指向数值的原地址（即un[0]的地址）
    printf("p1=%p, *p1=%d, &p1=%p\n",p1,*p1,&p1);

    // 直接对指针做加法，如图p1+4表示指向un[5]的地址，不能超出数组最大长度
    printf("%d\n",(*(p1+4)));

}


void array4(){

    // p1与p2不可以互相指定，因为他们指向的指针的数据类型不一样
    int *p1; double *p2;

}


void array5(int ar1[][4]){

    // 形参里面的参数表示一个包含有四个元素的指针数组
    // 和形参等价的代码：int (*ar2)[4] 和 int ar1[4][4] (系统自动忽略ar1后面第一个[]里面的4)
    // int ar1[][4]左边第一个空方括号可以看成是指针标识符，c只能识别第一个[]如int ar1[][][3]则错误


    // rows表示行数，cols表示列数（可变长的数组仅C99+才支持）
    // 下面是可变长的数组示例，可变长不代表可以任意改变维度，而是表示可以用变量来初始化数组
    // 一旦变量被变量初始化后，他的维度和数目等就无法二次改变！
     int rows=4,cols=5;
     double sale[rows][cols];
}


// 声明一个含有可变长数组的形参！该数组的行数和列数直接使用形参里面的rows和cols
void array6(int rows,int cols,int ar[rows][cols]){}


void array7(){

    // 下面就是一个复合字面量的使用
    // (int[2][4])表示字面量类型，这样子类似于可以创建一个匿名的数组！
    // 因为复合字面量是匿名的，所以必须要指定一个变量并将复合字面量赋予给他！
    int (*p1)[4] = (int[2][4]){{1,2,3,4},{5,6,7,8}};

}

int main(void)
{

    array3();
    array2();
    array4();


    // 先定义一包含4元素的数组arr4，再声明一包含3个arr4元素的数组arr3x4
    // void后面表示声明函数，事实上arr3x4=int ar1[][4]=int ar1[3][4]
    typedef int arr4[4];
    typedef arr4 arr3x4[3];
    void array5(arr3x4 ar);


    // 函数声明，可以省略形参名字，但是可变长的数组里面的行数和列数必须要用*来代替
    void array6(int,int,int [*][*]);

    return 0;
}
```

<br>

### 字符串函数

```c
#include <stdio.h>
#include<stdlib.h>


void S1(){

    // 被双引号括起来的内容叫做字符串字面量，也叫做字符串常量
    // 被双引号括起来的内容被视为指向该字符串存储位置的指针
    // %p打印的是一个地址，也就是hello的首字母h的地址而不是整个字符串的地址
    // 对字符串解引用*"think"即表示字符串首字母的值，也就是t，故%c输出t
    printf("%s %p %c\n","hey","hello",*"think");


    // char c1[]声明的是一个常量，而const char *c2声明的是一个变量！
    // 仅只有变量类型的才可以使用递增简写式，比如 c2++，所以绝对不可以说c1++
    // 我们可以用方法修改c2的内容但是无法修改c1的内容！！！
    char c1[] = "I love it";
    const char *c2 = "I love it";
    while (*(c2) != '\0')
    {
        putchar(*(c2++));
    }

}


void S2(){

    // 下面是创建一个指向字符串指针的数组，注意最后面也就是花括号那里有个分号！！！
    const char *c1[] = {
        "hello",
        "welcome",
    };

    // 这是创建一个char类型数组的数组
    char c2[2][50] = {
        "lily",
        "tom"
    };


    char name[100];
    // 下面是fgets()和fputs()函数的使用，他们一般用来操作文件，到那时也可以用于输出
    // fgets(name,100,stdin) name表示存储变量，100指输入字符串长度，stdin表示专门用于用户输入
    // fputs(name,stdout) name表示输出的变量，stdout表示专门用于输出字符串
    // fgets()会存储换行符，但是gets()不会
    // fputs()不会在输出的字符串末尾添加换行符，但是puts()会自动加上


    // gets_s(name,100)表示将字符串内容赋值到name里面并且字符串长度不超过100
    // gets_s()函数是在C99+才有的，注意输入的字符串不可以超出限制，不然会导致错误！

}


void S3(){

    // 直接在puts函数里面输入一个地址就可以把里面的值原封不动的输出出来了
    // puts所接受的内容必须是一个字符串，如果整个字符串最后没有\0的话，puts就会错误
    const char *sb[] = {
        "\nthanks",
        "\nlet me look it",
        "\nget out quickly!"
    };
    puts(sb[0]);

    // 这样子使用则会计算sb[1]的字符数目然后赋值给num
    int num = puts(sb[1]);

}


void S4(const char *str){

    // 通过这种办法我们创建了一个自定义输出函数
    // 以下代码表示先从首字符开始检测，若不是\0则输出并str地址下移一位，继续执行直到碰到\0
    // 注意*str++的++位置，因为从右到左运算，所以我们需要对str自增而不是对str指向的字符自增
    // *str!='\0'可以改写为*str，因为str指向空字符时值为0，C程序员特别喜欢这么做
    while (*str!='\0')
    {
        putchar(*str++);
    }

}


void S5(){

    // 使用以下的函数必须导入库stdlib.h
    // atoi可以把字符串类型的数值转换成整数型数值int，但如果字符串不是数字，则返回0
    // atof转换成double，而atol转换成long类型的
    char *str1 = "123";
    printf("\n%d %d",atoi(str1),atoi("asd"));


    // 使用strtol不仅可以将字符串数值转换成Long类型，而且可以指定转换的进制
    printf("%d",strtol("a","b",1));

}

int main(void)
{
    S1();
    S2();
    S3();
    S4("I love my dog");
    S5();
    return 0;
}
```

<br>

### malloc 内存分配

```c
#include <stdio.h>
#include<stdlib.h>			// 使用malloc函数必须要写入这个头文件
#include<stdatomic.h>


void testmalloc(){

    // malloc的返回值为一void，所以到最后我们需要强制转换类型，比如(double *)
    // 下面的代码为30个double类型的值申请内存空间
    // 这样可以代替可变长的数组使用！而且在低版本C也可以使用
    int n=5;
    double * ptd;
    ptd = (double *) malloc(n*sizeof(double));


    for(int i=0;i<5;i++){
        ptd[i]=i;
    }
    for(int ii=0;ii<5;ii++)
        printf("%7.2lf",ptd[ii]);


    // 必须要在最后把malloc分配的内存给释放掉，用free函数
    free(ptd);

}


void testcalloc(){

    // calloc和malloc用处完全一样，但是calloc可以提供俩形参
    // 第一个参数写存储单元数量，后面写存储单元的字节大小，sizeof的填入可以增加程序可移植性
    int m=5;
    long * l;
    l = (long *) calloc(100, sizeof(long));
    free(l);

}


void limitS(){

    // 该限定符表示限定的变量是一个可以通过代理而改变的变量
    volatile int a;

    // 当然可以const和volatile一起用，顺序不重要，b无法改变但是其代理可以变化
    const volatile int b;


    // restrict修饰一个指针，表示该指针是访问数据对象的唯一且初始的方式
    int * restrict re = (int *) malloc(10*sizeof(int));


    // 这声明一个原子类型变量，其他线程无法访问该变量k（太高端了你目前也没用）
    _Atomic int k;

}

int main(void)
{

    testmalloc();
    testcalloc();
    limitS();

    return 0;
    // return 0和exit(0)作用一致，但return 0只有在主函数内使用才可以退出程序，但exit任何位置都有效
}
```

<br>

### 文件处理

```c
#include <stdio.h>
#include<stdlib.h>
#include<string.h>


void fileone(){

    int ch;         // 用来保存获取文件内的字符
    FILE *fp;       // 存储文件名的指针，必须要有！！！

    // fopen表示打开文件，如果成功打开就会获取一个文件指针，打不开就返回一个NULL
    // fopen第一个参数是文件位置，文件位置最前面的点表示在当前文件目录下寻找，第二个参是读写选项
    // exit表示直接退出程序，不论在任何函数内都是如此
    if((fp=fopen("./file/writeone.txt","r"))==NULL){
        printf("the file cannot be opened");
        exit(EXIT_FAILURE);
    }

    // getc的参数是一个文件指针，获取第一个字符并赋值给ch后，若第一个字符不存在就会返回EOF
    // putc的作用就是不断地输出字符，直到文件末尾遇到空字符才停下，stdout表示输出，还有stdin,stderr
    while((ch=getc(fp)) != EOF){
        putc(ch,stdout);
    }

}


void filetwo(){

    // 可以同时处理俩文件，一个附则输入，一个则负责接收数据
    FILE *in,*out;
    int ch;

    // w表示写入功能，一旦以这种方式打开文件，该文件的内容直接全部清除，除非将w换成a使用
    in = fopen("./file/writeone.txt","r");
    out = fopen("./file/writeone_copy.txt","w");

    // 如果没有检测到结尾，就一直复制in中的文本到out中去，putc函数直观可见
    while((ch=getc(in))!=EOF){
        putc(ch,out);
    }

    // 用完文件需要手动关掉，非常麻烦下面的这个函数就是为了检测文件是否成功关掉
    if(fclose(in)!=0 || fclose(out)!=0){
        printf("文件关闭失败，请及时重试");
    }

}


void filethree(){

    FILE *fp;
    char words[40];

    // a+表示以附加的方式读写文件
    fp = fopen("./file/instring.txt","a+");

    // %40s表示允许输入40位字符串，stdin指输入，并将输入的值存入words数组里面
    // fprintf一行表示将字符数组words里面的字符逐个写入文件指针fp里面去，并且写一个换行一下
    while((fscanf(stdin,"%40s",words))==1){
        fprintf(fp,"%s\n",words);
    }

    // rewind表示返回到文件开始处，因为上面打印文字已经把光标移动到最后面了
    // fscanf则表示获取该文件内的所有内容并赋予给数组words，之后采用puts全部打印出字符
    rewind(fp);
    while(fscanf(fp,"%s",words)==1){
        puts(words);
    }

}


void filefour(){

    FILE *in = fopen("./file/nameless.txt","r");
    FILE *out = fopen("./file/nameless.txt","a+");
    char inc[10];

    // fgets一行表示将文件指针in内的前十个字符赋予给字符数组inc（in指针为r形式）
    fgets(inc,10,in);
    printf("\n%s",inc);

    // fputs一行表示将字符数组inc内的字符以附加形式补充道文件指针out最后面（out指针为a+形式）
    fputs(inc,out);

    fclose(in);
    fclose(out);

}


void filefive(){

    FILE *fp = fopen("./file/writeone.txt","r");

    // fseek所定位的位置必须是Long类型的数据！
    fseek(fp,0L,SEEK_END);  // 定位到结尾
    fseek(fp,3L,SEEK_SET);  // 定位到开头后三位
    fseek(fp,2L,SEEK_CUR);  // 于本位置向前移两位
    fseek(fp,-2L,SEEK_END); // 定位到结尾回退两位

    printf("\n%d",ftell(fp)); // ftell获取目前位置距离文件开头的字符数目

}


int main()
{

    fileone();
    filetwo();
    // filethree();
    filefour();
    filefive();


    return 0;
}
```

<br>

### 基本结构体

```c
#include <stdio.h>

#define MAXLEN 100
#define MINLEN 10


// 定义一个结构布局，但是并没有创建它，test叫结构标记不叫结构名字
struct test{
    char title[MAXLEN];
    int a;
};
// 我们可以省略结构标记，直接用末尾的一个结构名字来代替之
struct{
    double k;
    char c[MINLEN];
} tt;
// 定义一个嵌套变量，后面会有介绍并且使用它
struct text{
    struct test t;
    float k;
};


void structT(){

    // 可以直接初始化结构变量，里面的参数分别对应结构变量定义的内部变量类型！！！注意逗号和分号
    struct test TT = {
        "hello c++",
        12343
    };

    // 结构变量初始化可以通过指定关键字来不按顺序定义变量，类似于python的关键词形参
	// 但因为编译器不一致原因，故不建议使用本方法
    struct test TTT = {
        .a = 10086,
        .title = "love it"
    };

    // 创建一个结构变量，该结构变量名字叫T（现在test相当于可以视为一个数据类型）
    struct test T;

    // 结构变量相当于一个超级数组，这里通过下标调用了结构变量TT里面的子变量title
    printf("%s\n",TT.title);

}


void structArray(){

    // 这里创建一结构数组，ARRAY是名字，后面是调用方法，注意[0]的书写位置是在ARRAY后面的！！！
    struct test ARRAY[10];
    ARRAY[0].a=10;
    printf("%d\n",ARRAY[0].a);

}


void structIN(){

    // 嵌套结构，outter里面套了一个inner，紧接着就是初始化套娃结构变量的方法
    struct inner {
        int a;
        double b;
    };
    struct outter {
        struct inner handle;
        char k[MAXLEN];
    };
    // 初始化套娃结构变量
    struct outter let = {
        {12,54.0},
        "no thanks"
    };

    // 调用outter内的inner内的a，需要连续用点运算符取出才可以
    printf("%d\n",let.handle.a);

}


void structPT(){

    struct text tx = {
        {"hello",12},
        22.543
    };

    // 这样子就声明了一个指向text的指针结构变量，该变量名字叫txt
    struct text * txt;
    // 声明的指针变量可以指向一个现有的结构变量，方法用下面表示
    txt = &tx;
    // txt = &xxx[0] 这样子可以将指针结构变量指向一个结构数组

    // 用->符号来获取指针指向的内容，txt->t.title = tx.t.title
    printf("%s\n",txt->t.title);


    // 可以把结构变量内部的变量作为实参传递进一个函数里面

}


int main(void)
{
    structT();
    structArray();
    structIN();
    structPT();
    return 0;
}


/*
结构变量注意事项：
1. 声明结构变量时代码块里面的每一行后面用分号
2. 初始化结构变量时代码块里面的每一行后面用的是逗号，而且最后一行后面什么符号都没有
*/
```

<br>

### 指针结构体

```c
#include <stdio.h>
#include <string.h>

 struct str1 {
     double a;
     int c;
     char b[100];
 };

// ------------------------------------------
// 向一个函数直接传递结构（特别注意形参里面的结构类型写的是你需要传递进去的类型，这里是str1）
double num1(struct str1 sk1){
    return(sk1.a);
}
// 向一个函数传递结构地址，const限定了无法改变，上几节有讲过，所以const可加可不加
double num2(const struct str1 * sk2){
    return(sk2->a);
}
// ------------------------------------------


// ------------------------------------------
void num3(){
    struct in1 {
        int k;
    };

    // 当一个结构变量初始化的时候可以直接使用另一个初始化完成的结构变量赋值进去
    struct in1 i1 = {1};
    struct in1 i2 = i1;

}
// ------------------------------------------


// ------------------------------------------
// 这里使用了结构的双向传递模式
// 这里先通过获取结构变量s1的指针，并提取s1中变量c的值然后对s1中变量a的值进行更改
void mutualTalk(struct str1 * sk3){
    sk3->a = sk3->c + 100;
}
// ------------------------------------------


// 我们可以使用字符指针放在结构变量里面，然后让他接受一个字符串
// 但是该结构变量中字符指针只存储地址，所以在结构变量里面不会开辟一个新空间存储字符串
void charc(){
    struct ch1 {
        char * c1;
    };
    struct ch1 c = {"nothing can we do"};
    printf("%s\n",c.c1);
}


 // 复合字面量以及其结构
struct compound {
    char name[20];
    int age;
};
int compound_litera_1(struct compound * cpdd){
    return cpdd->age;
}
void compound_literal(){
    // 我们先声明一个结构，然后用下面圆括号括起结构类型的方法初始化一个复合字面量
    struct compound cpd;
    cpd = (struct compound) {"tom",12};

    // 我们可以给一个指针结构变量传递一个结构字面量
    int get_age = compound_litera_1(&(struct compound){"jack",19});
    printf("%d\n",get_age);
}



int main(void)
{

    struct str1 s1 = {
        14.556,
        88,
        "china"
    };

    double num1_out = num1(s1);     // 直接传递结构名字
    double num2_out = num2(&s1);    // 需要传递结构的地址，用取地址运算符&

    // 结构变量双向传递示例
    printf("%lf\n",s1.a);
    mutualTalk(&s1);
    printf("%lf\n",s1.a);

    // 使用字符指针来存储字符串，但要注意最好不要这么用因为会导致某些你找不到的bug
    charc();

    // 复合字面量在不同函数里面的应用
    compound_literal();

    return 0;
}
```

<br>

### 匿名结构体及其它特性

```c
#include <stdio.h>
#include <stdlib.h>


// 伸缩性数组成员
struct longevity{
    int k;
    double scores[];    // 声明一个可伸缩的数组成员
};
void longz(){
    struct longevity * lg;

    // 因为结构变量中的可伸缩的数组不占用空间，这里的malloc专门为其申请五个位置，才可以使用
    lg = malloc(sizeof(struct longevity)+ 5*sizeof(double));

    // 这里通过指针赋值并接着使用了该可变长的数组
    lg->scores[0]=12.4;
    printf("%lf\n",lg->scores[0]);
}


// 匿名结构
void anonymous_struct(){
    // 这里在一个结构里面直接定义了一个结构，该结构就是匿名结构
    // 调用匿名结构里面的变量不需要嵌套式的调用，可以直接调用！
    struct anony{
        struct {char k[10];double c;};
    };
    struct anony ay = {
        "china",
        12.3923
    };
    printf("%lf\n",ay.c);
}


// 把结构内容保存到文件之中



int main(void)
{
    longz();
    return 0;
}
```

<br>

### union 与 enum

```c
#include <stdio.h>


void union_1(){

    union un1{
        int a;
        double b;
        char c[100];
    };

    union un1 u1[10]; // 声明含有10个联合变量的数组

    // 声明一个联合变量后，该联合变量仅可以存储一种类型的内容，这里选择int类型的变量a进行赋值
    // 我们可以随意选择任何一个联合变量里面存着的数据类型进行赋值
    union un1 u2;
    u2.a=10;


    u2.a=100;   // 用新的100的值覆盖掉原来的10这个值
    u2.b=12983.3289;     // 这个时候联合变量u2就保存b这一double类型的值，原int类型的值被覆盖

}


// 联合的从属关系
void union_2(){

    // 首先在联合里面写入两个已经定义好的结构变量，之后再一个新的结构变量里声明该union
    struct str1{
        char a[100];
    };
    struct str2{
        char b[100];
    };
    union data{
        struct str1 s1;
        struct str2 s2;
    };
    struct str3{
        int status;
        union data info;
    };

    // 若使s3.status=0时，联合变量使用s3.info.s1.a的值
    // 若使s3.status=1时，联合变量使用s3.info.s2.b的值
    struct str3 s3;

}


// 枚举enum的使用
void enumtext(){

    // 下面声明一个枚举，之后声明一个变量c让他等于枚举类型color
    // c=blue表示设置c指定的枚举内容为枚举类里面的blue
    // 默认的枚举索引值从0开始，所以red=0,blue=1...（red这些叫枚举符，他们都是整数类型）
    enum color {red,blue,yellow,white};
    enum color c;
    c=blue;
    if(c==blue) printf("%d",c);


    // tom=0，但是在之后我们主动设置了枚举符的数值，那么后边也跟着，所以lily=101,sam=102
    // 把这些枚举类的名称用作switch判据也是完全可以的，而且极其方便！
    enum name {tom,jack=100,lily,sam};
}


// 名称作用域详解
void zone(){

    // 分属不同标签下的同名变量都算正确（但是在C++，此行为是被禁止的，运行会报错）
    struct str{int a;};
    int str;

}



// typedef自定义数据类型名字使用方法
// typedef和#define有些情况可以互换，如下面的两个例子，但是事实上define只是把SMALL赋予值short
typedef unsigned long long int LARGE;
#define SMALL short
LARGE nlli = 1000000000;
SMALL st = 1;

// 当声明指针时就是能用typedef，因为#define声明可能会导致只使一个变量获得了指针类型，譬如：
typedef char * STRING;
#define STR char *
STR name,age; // 实际上是 char *name,age
STRING first,second; // 结果是正确的 char *first, *second

// typedef当然可以用来表示结构，下面展示了一个自定义名称的复数类型
typedef struct complex{
    float real;
    float imag;
} COMPLEX;


int main(void)
{

    enumtext();
    return 0;
}
```

<br>

### 复制声明分析

```c
#include <stdio.h>


// * [] () 三大复杂声明解析
int * arr1[10];     // 含10元素数组，每个元素都是指向int的指针
int (* arr2)[10];   // 一个指向数组的指针，但数组元素都是int值而非指针
int (* arr3[2])[5]; // 二维数组，第一维度是指针，有俩个；第二维度是int，有五个


// 由于()优先级比*高，故下面代码表示先将函数pf定义为一指针，它里面才包含一形参char*
void (*pt)(char *){}
// 因*包围的范围很大，直接涵盖了形参，所以表示为返回字符指针的函数
void *pf(char *){}




int main(void)
{

    return 0;
}
```

<br>

### 宏入门 define

```c
#include <stdio.h>



// 可以使用反斜杠对定义进行换行处理，但是换行后多余的空格也会计算在内
#define OJ "this is a namelessness thing\
    which make him so exhausted?"

// 可以在一个宏里面使用另一个宏，但是在预编译是仅仅是定义，他不进行计算！！！
#define ONE 4
#define TWO ONE*ONE

// 简单粗暴的直接定义一行代码作为宏
#define PT printf("nameless\n");

void pr(){
    printf("%s\n",OJ);
    printf("%d\n",TWO);
    PT;
}


// 第一种写法解释为字符型字符串，第二种写法解释为记号型字符串
// 事实上一下俩写法意义一致，但有些编译器必须第二种写才可以正确识别，不过我们基本上可以忽略二者差别
#define K 1*2
#define KK 1 * 2

// 以下是类函数宏
// 调用该宏是传入一个参数x，然后后面才做计算并输出结果
#define PLUS(x) x*x

// 因为预编译器不计算，若调用p1(2+3)就会变成2+3*2+3=11而不是5*5=25
#define p1(x) x*x
// 圆括号限制可以解决此问题，输入p2(3+3)变成(3+3)*(3+3)=36为正确答案
#define p2(x) (x)*(x)

// #x用于替换p3(x)中的x，下面使用了字符串的连接方法，也就是中间没有任何符号的方法
#define p3(x) printf("this is " #x " the %d\n",(x)+(x))

// 两个##表示粘连字符，如3 ## 4结果为34
// 注意数据类型的匹配，这里两个整数相连结果仍为整数而不是字符串！
#define p4(n) 3 ## n


void PP(){
    printf("%d\n",PLUS(2));
    printf("%d\n",p1(2+3));
    printf("%d\n",p2(3+3));
    p3(4);
    printf("%d\n",p4(6));
}


// 三元运算符结合类函数宏实现取绝对值
#define ABS(x) ((x)>0 ? (x) : -(x))

// undef即撤销宏定义，但是无法检测宏之前是否被定义
#define LIMIT 123
#undef LIMIT


int main(void)
{
    pr();
    PP();
    return 0;
}
```

<br>

### 宏判断

在我们编写 C51 单片机头文件时，会频繁用到以下的判断语句，特别是 `#ifndef` `#else`

```c
#include <stdio.h>


// #ifdef判断后面的宏是否被定义，如果被定义则执行下面的代码
#ifdef NAME
    #define NAMELESS 123
#else
    #define NAME 456
#endif

// 如果ONE没有被定义则执行下面的代码
// 无论#ifdef或者#ifndef，最后都必须要有#endif
#ifndef ONE
    #define ONE 123
#endif

// #if后面跟整数表达式，若表达式非零则结果为真
#if SYS==1
    #define SYS1 123
#endif

// 较高版本的编译器可以使用以下代码代替#ifdef NA，即检测一个宏是否存在
#if defined (NA)
    #define NAA 123
#endif

void P1(){
    printf("%d\n",ONE);
}

int main(void)
{

    P1();
    return 0;
}
```

<br>

### 预定义宏

```c
#include <stdio.h>


void preInit(){
    printf("the file is %s\n",__FILE__);    // 写出源代码文件名字符串字面量
    printf("the line is %d\n",__LINE__);    // 写出源代码行数
    printf("the time is %s\n",__TIME__);    // 翻译代码的时间
    printf("the date is %s\n",__DATE__);    // 预处理的日期
}


// #error可以认为输出一段错误的信息
#ifndef NAMELESS
    #error nameless is invaild
#endif


int main(void)
{

    preInit();
    return 0;
}
```

<br>

### 泛型函数

```c
#include <stdio.h>


// 使用_Generic整个泛型函数
// 泛型表达式判断输入值的数据类型，符合的就输出一个值，譬如下面
// 若输入x=1，泛型判断为整数，而int:"int"此时就会返回一个int值
// default对应的就是当输入的数据不符合任何一个数据类型的时候，返回的值
#define MYTYPE(x) _Generic((x),\
    int:"int",\
    float:"float",\
    double:"double",\
    default:"other"\
    )

void typeOF(){
    printf("%s\n",MYTYPE(1));
    printf("%s\n",MYTYPE(3.22));
    printf("%s\n",MYTYPE(5.44f));
    printf("%s\n",MYTYPE("asdas"));
}


int main(void)
{

    typeOF();
    return 0;
}
```

<br>
