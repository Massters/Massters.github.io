---
layout: blog
code: true
category: 算法
title:  算法笔记小结
date:   2022-08-17 22:10
tags:
- 算法
- Code
---

<p align="center"><font size="30"><b>算法笔记归纳</b> </font></p>

## C/C++ 快速入门

- 数据变量类型`P7`:

  绝对值在$10^9$内整数可以定义为`int`型 32bit ;$10^{18}$内需要定义为`longlong`型 64bit

  longlong型赋大于$2^{31}-1$的值时,在其后加`LL` 

  单精`float`:32bit 1bit符号位 8bit指数位 23位尾数位 有效精度6-7

  双精`double`:64bit 1bit符号位 11bit指数位 52bit尾数位 有效精度15-16位

  浮点型数据都应该用`double`

  字符型:

  - 字符常量单引号
  - 字符串常量双引号

  - 小写字母比大写字母ASCⅡ值大32

- 宏:

  `#define 标识符 常量`:在能加括号的地方加上括号

  `const数据类型 变量名=常量;`

- `typedef`为复杂的数据类型起别名`P24`  例:`typedef long long LL;`

- math:

  - `fabs(double x)`
  - `floor(double x)`
  - `ceil(double x)`
  - `pow(double r,double p)`
  - `sqrt(double x)`
  - `log(double x)`
  - `sin cos  tan asin acos atan`
  - `round(double x)`返回类型为`double`需取整

- `if`:表达式为`!=0`则可以省略

- `switch`:case下属无需大括号,注意`break`

- `while`:只要条件成立,反复执行主体

- `for(A;B;C){}`:循环开始前,执行A,判断B,若成立则执行主体,否则退出.主体完成后,执行C,之后判断B...

- 数组:若大小较大(约$10^6$)则需要定义在主函数外部

  `memset(数组名,值,sizeof(数组名));`:需添加`string.h`建议只赋0或-1.按字节赋值.若赋其他数,则用`fill`函数.

- `%c`能识别空格与换行符,将其输入.而 `%s`用空格与换行识别字符串结束

- 一维数组,二维的第二维末尾有空字符`\0`,以表示字符串的结尾.

  字符数组长度要比实际存储字符串长度至少多1  	`P50`

- `strlen()`:字符数组中第一个`\0`前的字符数

- `strcmp()`比较,前<后,返回负整数

- `strcpy_s()`复制,数组2复制给1,包括结束符`\0`

- `strcat_s()`将数组2接到数组1后面.

- `sscanf(str,"%d",&n)`:将`str`中的内容以`"%d"`的格式写到`n`,支持正则表达式.

- `sprintf(str,"%d",n)`把`n`以`"%d"`的格式写到`str`中.

- 数组作参数时,第一维不需要写长度,二维数组第二维需要写长度.

  在函数中对数组元素的修改就等同于对原数组元素的修改,数组不能作返回类型.

- 在变量前加`&`表示变量的地址.`P61`

  指针是一个`unsigned`类型的整数

  给指针变量赋值方式一般是:将变量的地址取出来,然后赋给对应类型的指针变量.

- `int* p=&a;`  `int*`是指针变量的类型,`p`为变量名,用来存储地址.故地址`&a`是赋给`p`而不是`*p`

  `p`保存的是地址,`*p`是地址中存放的元素.`p+1`是p指向的`int`型变量的下一个`int`型变量的地址

  对指针变量,把其存储的地址的类型称为基类型,基类型必须和指针变量存储的地址类型相同.

- `a+i`等同于`&a[i]`   `*(a+i)`等同于`a[i]`

  两个`int`型指针相减,等同于求两指针间相差了几个`int`

- 引用:不产生副本,对引用的变量操作就是对原变量的操作

  在函数的参数类型后加`&`,常量不可引用

  `void swap(int*&p1,int &x){}`

- 访问结构体变量:普通变量`.`,指针变量`->`   `P71`

  `stu.id` `(*p).id` `p->id`

  结构体初始化:构造函数,可定义任意多个构造函数

  eg:`point(int _x,int _y):x(_x),y(_y){}`

- `cout`控制精度:

  `#include<iomanip>`

  `cout<<setiosflags(ios::fixed)<<setprecision(2)<<123.4567<<endl;`

- 浮点数的比较:`eps`,`pi` `p77`

  ```
  const double eps=1e-8;
  const double pi=acos(-1.0);
  fab((a)-(b))<(eps)等于
  ((a)-(b))>(eps)大于
  ((a)-(b))<(-eps)小于
  ((a)-(b))>(-eps)大于等于
  ((a)-(b))<(eps)小于等于
  ```

- 一般的OJ系统,一秒能承受运算次数大概为$10^7 \sim 10^8$

- 多点测试:

  - 输入方式:`while...EOF``while...break``while(T--)`
  - 输出类型:正常；每次输出后额外加一空行；两组内有空行,最后无.
  - 多点测试中,每一次循环要重置一下变量和数组,重置一般用`memset`或`fill`

  