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
