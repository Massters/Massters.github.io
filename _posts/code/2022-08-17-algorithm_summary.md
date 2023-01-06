---
layout: blog
code: true
category: 算法
title:  "算法笔记小结"
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

## 入门模拟

- 奇偶判断:`n%2==0`为偶

- 计算总分问题:`sum=sum+score`循环

- 多组输入:`while(scanf("%d",&n)!=EOF){}`

- 采用循环的比较,可在找到时,用`break`来退出

- 日期问题:

  - 求间隔 可令较小的日期不断+1,直到与第二天相等
  - 闰年定义:`(year%4==0&&year%100!=0)||(year%400==0)`
  - YYYYMMDD型日期提取:`y=time/10000,m=time%10000/100,d=time%100`

- 进制转换:

  $P进制x \xrightarrow{①}  10进制y \xrightarrow{②} Q进制z$

  ①:x为$a_1 a_2 a_3 \cdots a_n$可转换为十进制$y=a_1\cdot p^{n-1}+a_2\cdotp^{n-2}+\cdots a_{n-1}\cdot p +a_n$

  ```c++
  int y=0,product=1;       //product不断乘P,得到p,p^2,...p^(n-1)
  while(x!=0){
      y=y+(x%10)*product;		//x%10获取x的个位数
      x=x%10;					//去掉x个位
      product=product*p;
  }
  ```

  ②:采用"除基取余法"

  ```c++
  int z[40],num=0;	//num为位数
  do{
  	z[num++]=y%Q;	//取余
  	y=y/Q;		//取下一次的商
  }while(y!=0);
  ```

  最后将数组z逆序输出即可

- OJ不能使用`gets()`,可考虑使用`scanf(),fgets(str,n.stdin),cin.get(str,n),getline(cin,str)`

## 算法初步

- 排序:简单选择,插入

  - sort(首元素地址 ,==尾元素地址的下一个地址==,比较函数(选填)); 

    需添加`<algorithm>`和`using namespace std`

  - cmp函数:`bool cmp(int a,int b){return a>b;}//由大到小`

  - 结构体

  - 字符cmp:`return strcmp (a.name,b.name)<0;`a字典序<b字典序

  - 排名法:

    - 1.已经有序情况下,将数组第一个记为1名,遍历剩余,若当前分数=上一个分数,则当前排名=上一个排名.否则,当前排名=数组下标+1.
    - 2.令int变量为1,遍历,若当前个体不是第一个个体,且不等于上一个分数,令r=数组下标+1.

- 散列:`bool hashTable[maxn]={false}`

  - 字符串hash`P109`

    ```c++
    id=id*26+(S[i]-'A');
    id=id*52+(S[i]-"A");//A-Z
    id=id*52+(S[i]-'a')+26;//a-z
    ```

- 分治,递归:

  - Fibonacci数列
  - 全排列(Full Permutation)
  - n皇后(暴力,回溯)

- 贪心:

  - 简单贪心
  - 区间贪心:总是先选择左端点最大的区间`P122`

- 二分:

  - 二分查找`P126`
  - 模板`P129`序列中是否存在满足某条件元素.
  - 拓展:求根数学问题
  - 快速幂`P135`:$当(b\%2==1或b\&1==1)时a^b=a*a^{b-1} \\ 当(b\%2==0)时a^b=a^{\frac{b}{2}}\cdot a^{\frac{b}{2}}$

- two pointers:

  - 序列合并问题

  - **归并排序**`P140`

  - **快速排序**`P143`

  - 随机函数:添加`stdlib.h`和`time.h`

    在`main`函数头生成随机种子:`srand((unsigned)time(NULL));`

    `rand()`函数范围:`[0,RAND_MAX]`

    `rand()%(b-a+1)+a`范围:`[a,b]`

    生成大范围随机数:`(int)(round(1.0*rand()/RAND_MAX*(b-a)+a))`

    (`round()`四舍五入,`(int)`类型转换,$\frac{1.0\cdot rand()}{RAND\_MAX}\cdot(b-a)+a$相当于生成的随机数是[a,b]范围内的比例位置)

- 打表:预先计算,存入数组,直接读取.先暴力计算小范围结果,找规律.

- 活用递推

- 随机选择:无序数组求第k大数,类似快速算法

## 数学问题

- 计算数学:将int型整数n的每一位存入num数组:

  ```c++
  num[i]=n%10;
  n=n/10;
  ```

- 最大公约数:`gcd(a,b)=gcd(b,a%b),gcd(a,0)=a`

- 最小公倍数:`ab/d`或`a/b*b`(防止溢出)

- 分数:用结构体表示,up,down.

  - 化简:

    - 若down为负,令up,down变为相反数
    - 若up为0,令down=1

  - 约分:求出up绝对值与down绝对值的最大公约数d,同时除d

  - 加法,减法,乘法,除法

  - 输出:

    - 先化简

    - 若`r.down==1`,省略分母输出

    - 若`r.up绝对值>r.down绝对值`,说明是假分数,以带分数形式输出.

      - 整数部分为`r.up/r.down`

      - 分子:`abs(r.up)%r.down`

      - 分母:`r.down`

    - 分子分母用`longlong`型

- 素数:1即不是素数也不是合数

  - 判断:`n<=1`特判,遍历$2,3,\cdots,\lfloor sqrt(n) \rfloor$,看是否被整除.

  - 素数表:

    - 法一:枚举1~n是否是素数,在$10^5$大小没有问题

    - 法二:埃氏筛法:

      ```c++
      int prime[maxn],pNum=0;
      bool p[maxn]={0};
      void Find_Prime(){
          for(int i=2;i<maxn;i++){
              if(p[i]==false){   //如果i是素数
                  prime[pNum++]=i;//加入表
                  for(int j=i+i;j<maxn;j+=i){
                      p[j]=true;
                  }
              }
          }
      }
      ```

- 质因子分解:对1特判

  - 定义结构体`factor`存放质因子及个数,数组开10即可

  - ①枚举`1~sqrt(n)`内所有质因子p,判断p是否是n的因子.

    - 如果是,在`fac[]`中增加p,令个数`cnt=0`,然后只要p还是n的因子,就用n不断除以p,每次操作令p的个数+1,直到p不再是n的因子为止.
    - 如果p不是n的因子,则跳过

  - ②上述步骤结束后,n仍大于1,说明n有且仅有一个大于$\sqrt{n}$的质因子(有可能是n本身),这时需要把这个质因子 加入`fac`数组,并令其个数为1.

  - 注意:

    - int范围内正整数,素数表开$10^5$大小

    - n=1时特判

    - main函数开头调用`Find_Prime()`函数

      `Prime()`函数中`i<maxn`

      处理大于$\sqrt{n}$部分的质因子

      在循环外定义变量来存储$\sqrt{n}$

  - 求正整数N的因子个数:对其质因子分解,得各质因子$p_i$个数分别为$e_1,e_2,\cdots,e_k$,则质因子个数为$(e_1+1)(e_2+1)\cdots(e_k+1)$

    N所有因子之和为:$(1+p_1+p_1^2+\cdots+p_1^{e_1})(1+p_2+p_2^2+\cdots+p_2^{e_2})\cdots (1+p_k+p_k^2+\cdots+p_k^{e_k})\\=\frac{1-p_1^{e_1+1}}{1-p_1}\cdot \frac{1-p_2^{e_2+1}}{1-p_2}\cdots \frac{1-p_k^{e_k+1}}{1-p_k}$

- 大整数运算:

  - 结构体存储:`struct bign{int d[1000];int len;};`

    整数按字符串`%s`读入,读入之后 保存在d[]时要反转一下

    逆向赋值:`a.d[i]=srt[a.len-i-1]-'0';//(i=0;i<a.len;i++)`

    比较大小:从高位向低位比较

  - 四则运算:

    - 高精度加法(两正数)：

      ```c++
      for(int i=0;i<a.len||i<b.len;i++){
          int temp=a.d[i]+b.d[i]+carry;
          c.d[c.len++]=temp%10;
          carry=temp/10;
      }
      ```

    - 高精度减法:

      ```c++
      for(int i=0;i<a.len||i<b.len;i++){
          if(a.d[i]<b.d[i]){//如果不够减
              a.d[i+1]--;	//借位
              a.d[i]+=10;
          }
          c.d[c.len++]=a.d[i]-b.[i]
      }
      while(c.len-1>=1&&c.d[c.len-1]==0){   //去高位0
          c.len--;
      }
      ```

    - 高精度与低精度乘法:

      ```c++
      for(int i=0;i<a.len;i++){
          int temp=a.d[i]*b+carry;
          c.d[c.len++]=temp%10;//个位作为该位的结果
          carry=temp/10;//高位作为新的进位
      }
      while(carry!=0){  //进位可能不止一位
          c.d[c.len++]=carry%10;
          carry/=10;
      }
      ```

    - 高精度与低精度除法:

      ```c++
      bign divide(bign a,int a,int& r){//r为余数 ,引用
          bign c;
          c.len=a.len;
          for(int i=a.len-1;i>=0;i--){//高位开始
              r=r*10+a.d[i];//和上一位遗留的余数结合
              if(r<b) c.d[i]=0;//不够除,该位为0
              else{//够除
                  c.d[i]=r/b;//商
                  r=r%b;//新余数
              }
          }
          while(c.len-1>=1 && c.d[c.len-1]==0){
              c.len--;//去除高位的0,同时至少保留一位最低位
          }
          return c;
      }
      ```

- 拓展欧几里得算法: 

  - 扩展欧几里得算法 ax+by=gcd(a,b)的求解:

    - a\*1+b\*0=gcd 此时有x=1,y=0成立.

    - 递推公式:$\begin{cases} x_1=y_2\\y_1=x_2-(a/b)y_2 \end{cases}$

      ```c++
      int exGcd(int a,int b,int& x,int& y){
          if(b==0){
              x=1;
              y=0;
              return a;
          }
          int g=exGcd(b,a%b,x,y); //递归计算
          int temp=x;
          x=y;
          y=temp-a/b*y;
          return g;//g即gcd;x,y即所求解
      }
      ```

    - 全部解:$\begin{cases} x'=x+\frac{b}{gcd}\cdot k\\y'=y-\frac{a}{gcd}\cdot k  \end{cases} (k为任意整数)$

    - x和y的所有解以$\frac{b}{gcd}$与$\frac{a}{gcd}$为周期

    - 最小非负整数解:$(x\% \frac{b}{gcd}+\frac{b}{gcd})\% \frac{b}{gcd}$

  - ax+by=c的求解(c为任意整数):

    - ax+by=c存在解的充要条件是 $c\%gcd=0$且一组解(x,y)等于$\frac{cx_0}{gcd},\frac{cy_0}{gcd}$

    - 全部解:$\begin{cases}x'=x+\frac{b}{gcd}*k=\frac{cx_0}{gcd}+\frac{b}{gcd}*k\\ y'=y-\frac{a}{gcd}*k=\frac{cy_0}{gcd}-\frac{a}{gcd}*k \end{cases}$

    - 最小非负整数解:$(x\%\frac{b}{gcd}+\frac{b}{gcd})\%\frac{b}{gcd}$,可让x取$\frac{cx_0}{gcd}$,$x_0$为$ax+by=gcd$的一个解.

  - $ax\equiv c\pmod m$的求解:

    - 若整数a,b,m满足$(a-b)\%=0$,则a与b模m同余.

    - 每一个整数都各自与[0,m)中唯一的整数同余.

    - $ax\equiv c\pmod m\implies (ax-c)\%m=0$存在y使$ax-c=my$成立,即有$ax+my=c$

      当$c\%gcd(a,m)=0$时方程有解,先通过求解$ax+my=gcd(a,m)$得$(x_0,y_0)$由$(x,y)=(\frac{cx_0)}{gcd(a,m)},\frac{cy_0)}{gcd(a,m)})$可得$\begin{cases}x'=x+\frac{m}{gcd(a,m)}*k\\y'=y-\frac{a}{gcd(a,m)}*k\end{cases}$

    - 故:设a,c,m为整数,$m\geqslant 1$则:
      - 若$c\%gcd(a,m)\not=0$则同余式方程$ax\equiv c\pmod m$无解

      - 若$c\%gcd(a,m)=0$则$ax\equiv c \pmod m$恰好有$gcd(a,m)$个模m意义下不同的解,且解的形式为:

        $x'=x+\frac{m}{gcd(a,m)}*k \space 其中(k=0,1,\cdots,gcd(a,m)-1),x是ax+my=c的一个解$

- 逆元的求解以及$(b/a)\%m$的计算:
  - a,b,m整数,m>1,有$ab\equiv 1\pmod m$成立,则a和b互为模m的逆元.记$a\equiv \frac{1}{b}\pmod m $或$b\equiv \frac{1}{a}\pmod m$

  - 计算$(b/a)\%m$:找到a模m的逆元x,则有$(b/a)\%m=(b*x)\%x$成立.

  - $ax\equiv 1\pmod m$是否有解取决于$1\%gcd(a,m)$是否为0.等价于$gcd(a,m)$是否为1:

    - 若$gcd(a,m)\not= 1$则同余式方程$ax\equiv 1\pmod m$无解,a不存在模m逆元
    - 若$gcd(a,m)=1$,则$ax\equiv 1\pmod m$在(0,m)上有唯一解,可求解$ax+my=1$得到.$ax+my=1=gcd(a,m)$,用扩展欧几里得解出x,用$(x\%m+m)\%m$得(0,m)范围内解,即逆元.

  - ```c++
    inverse(int a,int m){
        int x,y;
        int g=exGcd(a,m,x,y); //求解ax+by=1(gcd(a,m)=1)
        return (x%,+m)%m;
    }
    ```

  - 费马小定理:设m是素数,a为任意整数且$a\not \equiv 0\pmod m$,则$a^{m-1}\equiv 1\pmod m$.

    由$a^{m-1}\equiv 1\pmod m$知$a*a^{m-2}\equiv 1\pmod m$即$a^{m-2}\%m$就是a模m的逆元.

  - 当$gcd(a,m)\not=1$时,扩展欧几里得与费马小定理均失效,a逆元不存在.但$(b/a)\%m$仍有值,此时,可用$(b/a)\%m=(b\%(am))/a$来计算值(仅b是a的整数倍的情况下)

- 组合数:

  - 求n!有多少质因子p

    - n!中有$(\frac{n}{p}+\frac{n}{p^2}+\frac{n}{p^3}+\cdots)$个质因子p

      ```c++
      int cal(int n,int p){
          int ans=0;
          while(n){
              ans+=n/p;//累加n/(p^k)
              n/=p; //相当于分母多乘一个p
          }
          return ans;
      }
      ```

      上述算法可以很快的计算出n!的末尾有多少个0,末尾0的个数等于n!中因子10的个数,还等于质因子中5的个数,即代入cal(n,5)就可以得到结果.

    - n!中质因子p的个数等于1~n中p的倍数的个数$\frac{n}{p}$加$\frac{n}{p}!$中p的个数,得到递归版本的算法:

      ```c++
      int cal(int n ,int p){
          if(n<p)return 0;
          return n/p+cal(n/p,p); //放回n/p+(n/p)!中质因子p的个数
      }
      ```

  - 组合数计算:

    - 计算$C_n^m$

      - 定义计算(法一):$C_n^m=\frac{n!}{m!(n-m)!}$承受$n\leqslant20$

      - 递推计算(法二):$C_n^m=C_{n-1}^m+C_{n-1}^{m-1},C_n^0=C_n^n=1$承受$n<67,m<33$

      - 定义式变形(法三):$C_n^m=\cfrac{n!}{m! (n-m)!}=\cfrac{\cfrac{\cfrac{\cfrac{(n-m+1)}{1}*(n-m+2)}{2}*\cdots}{\cdots}*(n-m+m)}{m}$承受n<62,m<31

        故

        ```c++
        long long C(long long n,long long m){
            long long ans=1;
            for(long long i=1;i<=m;i++){
                ans=ans*(n-m+i)/i;  //注意要先乘后除
            }
            return ans;
        }
        ```

    - 计算$C_n^m\%p$

      - 递推公式(法一):假设2p不超过int型,支持$m\leqslant n\leqslant1000$,p大小素性无要求($p\leqslant 10^9$)都是可以的.

        ```c++
        int res[1010][1010]={0};
        int C(int n,int m,int p){
            if (m==0||m==n)return 1;
            if(res[n][m]!=0)return res[n][m];
            return res[n][m]=(C(n-1,m)+C(n-1,m-1))%p;
        }
        ```

      - 定义式(法二):支持$m\leqslant n\leqslant 10^6$,p大小素性无要求

        将$C_n^m$质因子分解:$C_n^m=p_1^{c_1}\cdotp_2^{c_2}\cdots p_k^{c_k}$则$C_n^m\%p=p_1^{c_1}\cdotp_2^{c_2}\cdots p_k^{c_k}\%p$

        ```c++
        int prime[maxn];  //获取素数表
        int C(int n ,int m ,int p){
            int ans=1;
            for(int i=0;prime[i]<=n;i++){
                int C=cal(n,prime[i])-cal(m,prime[i])-cal(n-m,prime[i]);//计算C(n,m)中prime[i]的指数Ci,cal(n,k)为n!中含质因子k的个数.
                ans=ans*binaryPow(prime[i],c,p)%p; //快速幂
            }
           	return ans;
        }
        ```

      - 定义式变形(法三):基于前一个问题的法三

        - m<p,p为素数:支持$m \leqslant 10^5,n,p\leqslant 10^9$

          ```c++
          int C(int n ,int m,int p){
              int ans=1;
              for(int i=1;i<=m;i++){
                  ans=ans*(n-m+i)%p;
                  ans=ans*inverse(i,p)%p;  //每次ans是已经取过模的结果,不能做除法.故用逆元做 
              }
              return ans;
          }
          ```

        - m任意,p为素数,支持$m \leqslant10^5,n,p\leqslant10^9$

          统计分子比分母多含质因子p多少个,并去除分子分母中的质因子p,其余部分正常计算.

          统计完毕,若分子中p个数大于分母中p的个数 ,返回0.否则返回计算结果.

        - m任意,p可能不是素数.支持$m\leqslant10^4,n,p\leqslant 10^9$

          - 将p质因子分解,对每一个质因子$p_i$统计组合数$C_n^m=\frac{(n-m+1)\cdots (n-m+m)}{1\cdot2\cdots m}$的分子比分母多含$p_i$多少个,同时去除分子分母中所有$p_i$,正常计算逆元并取模存于ans.所有的$p_i$统计完后,假设$p_i$还剩$c_i$个,使用快速幂计算所有$p_i^{c_i}\%p$,然后令ans乘上所有$p_i^{c_i}\%p$结果即可.
          - 将分子分母中每个数都质因子分解,然后合并结果,最后快速幂取模即可.

      - Lucas定理(法四):支持$m\leqslant n \leqslant10^{18}$,p是素数,$p\leqslant 10^5$

        如果p是素数,将m,n表示为p进制

        $m=m_kp^k+m_{k-1}p^{k-1}+\cdots+m_0\\n=n_kp^k+n_{k-1}p^{k-1}+\cdots+n_0$

        则$C_n^m\equiv C_{n_k}^{m_k}\cdot C_{n_{k-1}}^{m_{k-1}}\cdots C_{n_0}^{m_0}\pmod p$成立

        ```c++
        int Lucas(int n,int m){
            if(m==0)return i;
            return C(n%p,m%p)*Lucas(n/p,m/p)%p;
        }
        ```

## STL

- vector:变长数组 `p191`   `#include<vector>`

  - 定义：`vector<typename>name;`一维

  - `vector<typename>Arrayname[arraySize];`二维

  - 访问:下标，迭代器：`vector<typename>::iterator it;`通过`*it`访问元素

  - `begin()`首元素地址，`end()`尾元素地址的下一个地址，即左闭右开。

  - 只有`vector`和`string`中才允许用`vi.being()+3` 这种迭代器加上整数操作。

  - `push_back(x)`后面添加一个元素x

    `pop_back()`产出尾元素

    `size()`

    `clear()`

    `insert()`:`insert(it ,x)`向迭代器it处插入x

    `erase()`:

    - `erase(it)``
    - ``erase(first,last)`范围：[first,last)

  - 用途：

    - 储存数组，
    - 处理最后一个满足条件的数组后不输出额外空格
    - 用邻接表存储图。

- set:内部有序，不含重复元素 `p197` `include<set>`

  - 定义：`set<typename>name;`

  - 除了`vector`和`string`外的STL容器，都不支持`*(it+i)`方式访问，以如下方式枚举

    `for(set<int>::iterator it=st.begin();it!=st.end();it++)`

  - `insert()`

    `find(value)`返回的是迭代器

    `erase()`:

    - `erase(it)`
    - ``erase(value)`
    - `erase(first,last)`

    `size()`

    `clear()`

  - 用途：自动去重并升序排序

  - 元素不唯一用`multiset`,速度更快使用`unordered_set`

- string:`p202` `#include<string>`

  - 定义：`string str;`

  - 访问：

    - 下标：如果要读入和输出整个字符串，只能用` cin`和`cout`.若想用`printf`输出，则可以用`c_str()`将string类型转换为字符数组输出。
    - 迭代器：定义`string::iterator it;`,支持迭代器加减数字操作。

  - `operator+=:`:将两个字符串直接拼接起来，如：`str3=str1+str2`

    compare operator:比较字符串：==,!=,<,<=,>,>=

    `length()`

    `size()`

    `insert()`:

    - `insert(pos,srting)``
    - ``insert(it ,it2,it3)`it2,it3为待插字符串首尾迭代器

    `eraser()`:

    - `erase(it)`
    - `erase(first,last)`
    - `erase(pos,length)`

    `clear()`

    `substr()`:`substr(pos ,len)`放回从pos开始长len的子串

    `string::npos`:常数,本身值为-1,可作`find()`失败时返回值,`unsigned_int`类型

    `find()`:

    - `find(str)`:成功返回在str中第一次出现位置,失败返回`npos`
    - `find(str,pos)`从pos位开始匹配

    `replace()`:

    - `replace(pos, len,str2)`将str从pos开始长为len的子串替换
    - `replace(it1,it2,str2)`将str的迭代器[it1,it2)范围子串替换为str2

- map:映射`p213` `include<map>`

  - 定义:`map<typename1,typename2>mp;`typename1:键类型 ,typename2:值类型
  - 访问:
    - 下标:map中键唯一
    - 迭代器:
      - it->first访问键
      - it->second访问值
      - map以键从小到大排序

  - `find(key)`:返回键为key的迭代器,失败返回`end()`

    `erase()`:

    - `erase(it)`
    - `erase(key)`
    - `erase(first,last)`

    `size`

    `clear()`

  - 用途:

    - 建立字符与整型间映射
    - 判断大整数或其他数据是否存在,可将map当bool数组用
    - 字符串到字符串映射

  - 若一个键对应多个值,用`multimap`,速度快用`unordered_map`

- queue:队列`p218` `#include<queue>`

  - 定义:`queue<typename>name;`

  - 访问:只能用`front()`访问首元素,或`back()`访问队尾元素

  - `push()`入队

    `front()`

    `back()`

    `pop()`出队

    `empty()`

    `size()`

  - 用途:广度优先搜索时,用queue.

  - `front()`和`pop()`前,必须用`empty()`判断

  - `deque`双端队列

- priorit_queue:优先队列`p221`,队首为优先级最大的

  - 定义:`priority_queue<typename>name;`

  - 访问:只能通过`top()`访问队首元素,没有`front()`和`back()`

  - `push(x)`

    `top()`

    `pop()`

    `empty()`

    `size()`

  - 优先级设置:

    - 基本数据类型:

      - `priority_queue<int>q;`

      - `priority_queue<int,vector<int>,less<int> >q;`

        以上两种形式等价,`less<int>`表示数字大的优先级高,若想把最小元素放队首,则`priority_queue<int,vector<int>,greater<int> >q;`

    - 结构体:重载"<"

      - 结构体内:添加

        ```c++
        friend bool operator < (fruit f1,fruit f2){
            return f1.price > f2.price;
        }
        ```

        之后直接定义即可

        `priority_queue<fruit>q;`

      - 结构体外:

        ```c++
        struct cmp{
            bool operator() (fruit f1,fruit f2){
                return f1.price > f2.price;
            }
        };
        ```

        此时定义改为:`priority_queue<fruit,vector<fruit>cmp>q;`

      - 两种写法,return效果均与`sort()`中的cmp相反,以上代码效果均为数字大的在队列顶,与默认的效果(基本数据类型)一致.

    - 若数据较大(若字符串或数组)使用引用提高效率,将比较类的参数改为:`(const fruit &f1,const fruit &f2)`

  - 用途:
    - 解决贪心问题
    - 对Dijkstra算法优化
  - 使用`top()`前必须用`empty()`判空.

- stack:栈`p227` `#include<stack>`

  - 定义:`stack<typename>name;`

  - 访问:只能通过`top()`访问栈顶元素

  - `push()`

    `top()`

    `pop()`

    `empty()`

    `size()`

  - 用途:

    - 模拟实现递归
    - 防止程序对栈内存的限制而导致出错

- pair:可以看作内部有两元素的结构体`p230` `#include<utility>`

  - 定义:

    - `pair<typename1,typename2>name;`
    - `pair<string,int>p("haha",5);`定义的同时初始化
    - `pair<string,int>("haha",5)`临时定义
    - `make_pair("haha",5)`临时定义

  - 访问:pair中只有两元素`first` `second`按元素结构体访问即可.

  - 比较:== ,!= ,< ,<= ,>  ,>=规则先以`first` 大小为标准,相等则比较`second`

  - 用途:

    - 代替二元结构体及其构造函数

    - 作为`map`的键值来插入

      `mp.insert(make_pair("heihei",5));`

      `mp.insert(pair<string,int>("haha",10));`

- Algorithm下的常用函数:

  - `max(x,y)`,`min(x,y)`参数必须2个

  - `abs(x)`返回绝对值,x必须是整数,浮点数使用math下的`fabs()`

  - `swap(x,y)`交换x,y的值

  - `reverse(it,it2)`将数组指针在[it,it2)之间的元素或容器迭代器在[it,it2)范围内的元素反转

  - `next_permutation()`给出一个序列在全排列中的下一个序列,循环要使用`do...while`,达到最后一个排列时返回`false`

  - `fill()`将数组或容器中的某一段区间赋为某个相同的值`fill(a,a+5,23)`

  - `sort()`排序:

    - cmp函数:

      - 基本数据类型数组排序:`bool cmp(int a,int b){return a>b;}`

      - 结构体数组的排序:`bool cmp(node a,node b){return a.x<b.x;}`

      - 容器排序:

        只有`vector`,`string`,`deque`可以使用`sort()`

        `bool cmp(int a,int b){return a>b;}`

        `sort(vi.begin(),vi.end(),cmp();`

    - 用法:sort(首元素地址,尾元素地址的下一个地址,比较函数(选填))

  - `lower_bound()/upper_bound()`:需用在有序容器中

    - `lower_bound(first,last,val)`寻找[first,last)内第一个值大于等于val的元素位置
    - `upper_bound(first,last,val)`寻找[first,last)内第一个值大于val的元素位置
    - 成功返回数组指针或容器迭代器,失败返回可以插入改元素的位置指针或迭代器

## 数据结构专题(1)

- 栈的应用:

  - 数组实现:栈顶指针int型变量TOP,栈空TOP=-1
  - 链表实现:栈顶指针`int*`型指针
  - `pop()`,`top()`前必须用`empty()`判空
  - 清空栈:`while(!st.empty()){st.pop();}`或定义可以新栈实现变相清空.
  - 简单计算器:中缀->后缀->计算`p248`
    - 除法可能导致浮点数,故操作数类型设为`double`
    - 用`string`的`erase()`方法消空格
    - 数字可能不是个位数
    - 为了读输入中的空格,需要用`getline(cin,str)`

- 队列的应用:

  - 队首指针`front()`指队首元素的==前一个位置==,队尾指针`rear`指队尾元素

  - 初始状态`front=-1,rear=-1`

  - `front+1`为队首元素位置

  - `pop()`,`get_front()`,`get_rear()`前必须用`empty()`判空

  - 清空:`while(!q.emtpy()){q.pop()}`

    - 若单独用函数清空,则参数需引用:

      ```c++
      void emtpy(stack<int> &q){
      	while(!q.empty()){
      		q.pop();
      	}
      }
      ```

    - 更常见的方法是重新定义一个队列以实现队列的清空.

- 链表处理:`p253`

  - ```c++
    struct node{
    	typename data;
        node* next;
    };
    ```

  - 分配内存:

    - `typename* p=(typename*)malloc(sizeof(typename));`

      例:`int* p=(int*)malloc(sizeof(int));`

    - `typename* p=new typename;`

      例:`int* p=new int;`

  - 释放内存:

    - `free(p);`:对应malloc
    - `delete(p);`:对应new

  - 创建:

    - `node* node1=new node;`

    - 使用for循环创建:

      ```c++
      node* create (int Array[]){
          node *p,*pre,*head;
          head=new node;    //head
          head->next=NULL;  //head
          pre=head;         //pre=head
          for(int i=0;i<5;i++){
              p=new node;         //p
              p->data=Array[i];   //p
              p->next=NULL;       //p
              pre->next=p;        //pre
              pre=p;              //pre
          }
          return head;
      }
      ```

  - 查找:从第一个结点开始,不断判断当前结点数据域是否等于x,若等于,`count`+1,直到链表结尾,最后返回计数器`count`

  - 插入:

    - 插入位置:在第3个位置插入4的意思是:插入完成后第3个位置是4
    - 过程:![](https://cdn.jsdelivr.net/gh/Massters/images/images/images链表插入过程.png)

  - 删除:

    ![](https://cdn.jsdelivr.net/gh/Massters/images/images/images链表删除过程.png)

  - 静态链表:

    - 原理:hash,令数组下标直接表示结点地址,不需要头结点

    - 结构体类型名和结构体变量名尽量不同

    - 模板:

      1. 定义静态链表

         ```c++
         struct Node{
             int address;//结点地址
             typename data;//数据域
             int next;//指针域
             xxx;//结点的性质,不同的题目会有不同的设置
         }node[100010];
         ```

      2. 初始化

         ```c++
         for(int i=0;i<maxn;i++){
             node[i].xxx=0;
         }
         ```

      3. 遍历链表,对性质xxx标记,对有效结点数统计

      4. 数组排序以把有效点移至左端,这样就可以用步骤3的计数来访问它们

         ```c++
         bool cmp(Node a ,Node b ){
             if (a.flag==false||b.flag==false)return a.flag>b.flag;  //只要a,b中有一个无效,就把它放到后面.
             else return a.data<b.data;//如果都是有效结点,则按要求排序
         }
         ```

      5. 由题意操作

## 搜索专题

- DFS:

  - 栈,递归,剪枝

  - 函数{递归边界;分支;}
  - 常见问题:
    - 最大价值
    - 给定序列枚举序列的子序列
    - 从N个数中选k个数的所有方案

- BFS:

  - 队列,递归

  - ```c++
    void BFS(int s){
        queue<int> q;
        q.push(s);
        while(!q.empty()){
            取出队首元素top;
            访问队首元素top;
            将队首元素出队;
            将top下一层结点中未曾入队的结点全部入队,标记层号+1,并设为已入队;
        }
    }
    ```

  - 增量数组:

    ```c++
    X[4]={0,0,1,-1}
    Y[4]={1,-1,0,0}
    ```

  - 常见问题:

    - 矩阵的块个数
    - 矩阵迷宫及最小步数

  - 注意:

    - inq数组含义是判断结点是否入过队,而不是结点是否已被访问
    - 元素入队的push操作只是制造了元素的一个副本入队,与原数据互不干扰,故当需要对队列中的元素进行修改而不仅是访问时,队列中存放的元素最好不要是元素本身,而是它们的编号

## 数据结构专题(2)

- 树:

  - 二叉树:插入时,根结点指针root要使用引用

  - 一般来说,函数中需新建结点时,即对结构做修改需引用

  - 务必令新结点左右指针域为NULL

  - 通过数组存完全二叉树,数组大小为$2^k$,k为完全二叉树最大高度,1号位必须存放根结点

  - 数组中元素存放的顺序恰好为该完全二叉树的层序遍历

  - 判断叶结点:该结点(记为root)的左子结点(即root*2)>总结点数n

  - 判断空结点:root>n

  - 遍历:

    - 先序,中序,后序:DFS
    - 层序:BFS

  - 左子树一定先于右子树遍历

  - 必须知道中序遍历才能唯一确定一棵树(+先,后,层序中任意一个)

  - 层序遍历思路:

    1. 根结点入队q

    2. 取队首结点,访问它

    3. 如果该结点有左孩子,左孩子入队

    4. 如果该结点有右孩子,右孩子入队

    5. 返回步骤2,直到队列空

       - 求所在层次,则在结构体中加入layer项

       - 队列中的元素为`node*`型,队列中保存的只是原元素的副本

  - 给定先序,中序,重建二叉树

  - 静态二叉链表

  - 树建议静态写法:

    ```c++
    struct node{
        typename data;
        vector child;
    }Node[maxn];
    ```

  - 对所有合法的DFS求解过程,都可以画成树的形式,且这棵树的DFS遍历过程为先根,BFS为层序遍历

- 二叉查找树(BST)

  - 插入:查找成功时,说明结点存在,查找失败,则失败处为插入的地方,注意root需引用
  - 删除:找到比删除结点小的最大结点(即前驱)(或比删除结点大的最小结点(即后继)),覆盖欲删除的结点,删除原结点
    - 前驱为该结点左子树中最右结点,后继为该结点右子树中最左结点
    - 基本思路:
      - 若root为空,返回
      - 若`root==x`,说明找到了欲删除结点,进入删除环节
        - 若root无孩子,直接删除
        - 若root有左孩子,使pre覆盖root,删除pre
        - 若root有右孩子,使next覆盖root,删除next
      - 若`root>x`,左子树递归
      - 若`root<x`,右子树递归
    - 若总优先删pre(或next),会导致树不平衡,可以
      - 交替删除
      - 记录高度,优先删除高的
  - 性质:BST中序遍历时,结果有序

- 平衡二叉树(AVL)`p320`

  - AVL仍然是一棵BST

  - 插入:

    - 左旋:root指向A,temp指向B

      ![](https://cdn.jsdelivr.net/gh/Massters/images/images/images左旋_修改.png)

      1. B的左子树△成为A的右子树

      2. A成为B的左子树

         更新root和temp高度

      3. 根结点设为B

    - 只要把最靠近插入结点的失衡结点调整正常,路径上所有的结点都会平衡

    - A左孩子平衡因子为1时,为LL型,-1时为LR型

      - LL型:

        ![](https://cdn.jsdelivr.net/gh/Massters/images/images/imagesLL.png)

        将以C为根结点的子树看作一个整体,然后以结点A为root右旋即可

      - LR型:

        ![](https://cdn.jsdelivr.net/gh/Massters/images/images/imagesLR.png)

        先忽略结点,以C为root左旋,可将情况转为LL型,再按LL做法进行一次右旋即可

    - A右孩子平衡因子为-1时,为RR型,1时为RL型

      - RR型:

        ![](https://cdn.jsdelivr.net/gh/Massters/images/images/imagesRR.png)

        将以C为根结点的子树看作一个整体,然后以结点A为root进行左旋即可

      - RL型:

        ![](https://cdn.jsdelivr.net/gh/Massters/images/images/imagesRl.png)

        先忽略结点A,以C为root右旋,可将情况转为RR型,按RR型做法进行一次左旋即可

    - 在每个`insert`函数后,更新当前子树高度.

  - AVL树的建立:依次插入n个结点即可

    ```c++
    node* Create(int data[],int n){
        node* root=NULL;
        fot(int i=0;i<n;i++){
            insert(root,data[i]);
        }
        return root;
    }
    ```

- 并查集:

  - 操作:

    - 合并
    - 查找

  - 同一集合只存在一个根结点

  - 基本操作:

    - 初始化:令所有`father[i]=i;`

    - 查找:递归或递推寻找父结点

    - 合并:先判断两元素是否同属一个集合,合并时为:将其中一个集合的根结点的父亲指向另一个集合根结点

      ```C++
      void Union(int a,int b){
          int faA=findFather(a);
          int faB=findFather(b);
          if(faA!=faB)father[faA]=faB;
      }
      ```

    - 并查集产生的每一个集合都是一棵树

  - 路径压缩:把当前查询结点的路径上所有结点的父亲都指向根结点

    ```c++
    int findFather(int x){
        int a=x;
        while(x!=father[x]){
            x=father[x];
        }
        while(a!=father[a]){
            int z=a;
            a=father[a];
            father[z]=x;
        }
        return x;
    }
    ```

- 堆:完全三叉树

  - 优先队列默认大顶堆

  - 向下调整(建堆):总是将当前结点V与其左右孩子比较(如果有)假如孩子中存在权值比V大的,将权值最大的孩子与V比较;交换完毕后,继续让V和孩子比较,直到V孩子权值都比V小或V没有孩子

  - 向上调整(添加):把欲调整的结点V与父结点比较:如果V权值比父结点大,则交换,反复比较,直到堆顶或父结点权值较大为止

  - 堆排序:取堆顶元素,将堆最后一个元素替换至堆顶,再进行一次向下调整,重复直至堆中只有一个元素为止

    ```c++
    void heapSort(){
        createHeap();
        for(int i=n;i>1;i--){
            swap(heap[i],heap[1]);
            downAdjust(1,i-1);
        }
    }
    ```

- 哈夫曼树:

  - 哈夫曼树中不存在度为1的结点,权值越高的结点,相对越接近根结点.
  - 构建思想:反复选择两个最小元素,合并,直到只剩下一个元素.
  - 哈夫曼编码:所有左分支都标记为0,所有右分支都标记为1
    - 把字符出现次数作为各自叶子结点的权值,则字符串编码成01串后的长度实际上就是这棵树的带权路径长度.

## 图算法专题

- 遍历

  - 累加某条边的边权后将这条边删除(即将反向边边权设为0),以避免走回头路,重复计算边权.

- 最短路径:

  - 常用算法:Dijkstra,Bellman-Ford,SPFA,Floyd

  - Dijkstra:

    - 基本思想:对图G(V,E)设置集合S,存放已被访问的顶点,然后每次从集合V-S中选择与起点s的最短距离最小的一个顶点(记为u),访问并放入集合S.之后,令顶点u为中介点,==优化起点s与所有从u能到达的顶点v之间的最短距离==.这种操作执行n次,(n为顶点数),直到S已包含所有顶点.

    - 集合S可用bool数组vis[]实现,int数组d[]表示起点s到vi的最短距离.初始时,除了s的d[s]赋0,其余赋最大数,$10^9$或`0x3fffffff`

    - 如果边权出现负数,那么Dijkstra很可能出错,最好使用SPFA.

    - 拓展第二标尺:(求路径,新增pre[]即可):

      - 新增边权:新增cost\[u][v]和c[].
      - 新增点权:新增weight[u]和w[]
      - 求路径条数:新增num[].

    - 模板化方式:Dijkstra+DFS:

      先在Dijkstra中记录下所有最短路径(只考虑距离),后从最短路径中选出一条第二标尺最优路径.

      1. 在Dijkstra中记录最短路径.pre数组:`vector<int>pre[maxn]`且一开始不用赋初值.

         如果d[u]+G[u\][v]<d[v],清空pre[v]添加u.

         如果d[u]+G[u]\[v]==d[v],直接添加u

      2. 遍历所有最短路径,找出第二标尺最优路径.

         递归遍历路径DFS

         作为全局变量的第二标尺最优值`optValue`

         记录最优路径数组`path`(vector)

         临时记录DFS遍历到叶子结点时路径`tempPath`(vector)

         `tempPath`中路径结点是逆序的.

  - Bellman-Ford:

    - 主要思路:对图中边进行V-1轮操作,每轮遍历所有边.

      对每条边u->v,如果以u为中介点,可以使d[v]更小,就用d[u]+length[u->v]更新d[v]

      再对所有边判断是否某边u->v仍满足d[u]+length[u->v]<d[v],如果有,说明图中有源点可达负环,返回`false`

    - 使用邻接表比较方便

    - 扩展第二标尺需注意统计路径条数:

      若按Dijkstra中num[]写法,会反复累计已计算过的顶点.

      需设置前驱数组`set<int>pre[maxn]`当遇到一条和已有最短路径长度相同的路径时,必须重新计算条数.

  - SPFA:在Bellman-Ford优化而来

    - 原理:只有当某个顶点u的d[u]值被改变时,从它出发的邻接点v的d[v]值才有可能改变.
    - 建立一个队列,每次将队首顶点u取出,然后对所有从u出发的边u->v判断d[u]+length[u->v]<d[v]是否成立,若成立,用d[u]+length[u->v]覆盖d[v],此时若v不在队列中,把v加入队列,直到队列为空(说明无源点可达负环)或某顶点入队次数超过V-1(说明有源点可达负环).
    - 如果负环从源点不可达,需添加一辅助顶点C,并添加一条从源点到C的有向边以及V-1条从C到除源点外各顶点的有向边,才能判断负环是否存在.

  - Floyd:全源最短路径,邻接矩阵.

    - 思路:枚举顶点k$\in$[1,n]

      以k为中介点,枚举所有顶点对i和j($i\in[1,n],j\in[1,n]$).

      如果dis[i]\[k]+dis[k]\[j]<dis[i]\[j]

      赋值dis[i]\[j]==dis[i]\[k]+dis[k]\[j]

- 最小生成树:给定无向图G(V,E)中求一树T,T拥有G中所有顶点,所有边都是来自图G中的边,且满足T的边权之和最小.

  - prim:

    对图G(V,E)设置集合S,存放已被访问的顶点,然后每次从集合V-S中选择与集合S的最短距离最小的一个顶点(记为u),访问并加入集合S,之后,令u为中介点,==优化所有从u能到达的顶点v与集合S之间的距离==,这样的操作执行n次,直到S包含所有顶点.

    思想与Dijkstra几乎相同,只是涉及最短距离时使用集合S代替Dijkstra中的起点s.

  - kruskal:边贪心

    - 思想:初始状态时隐去图中所有边,之后执行:

      1. 对所有边按边权大小排序

      2. 按边权从小到大测试所有边,如果当前边所连接的两个顶点不在同一个连通块中,则把这条边加入当前最小生成树中,否则舍弃.

      3. 执行步骤2,直到最小生成树的边数等于总顶点数减1,或测试完所有边时结束.

         当结束时,如果最小生成树的边数小于总顶点数减1,说明图不连通.

      4. 使用并查集判断结点是否同属于一个集合以及合并操作,注意并查集初始化范围与顶点编号范围一致.

  - 稠密图(边多):prim,稀疏图(边少):kruskal

- 拓扑排序:有向无环图G的所有顶点排成一线性序列,使G中任意两个顶点u,v,如果存在边u->v,那么序列中u一定在v前,序列称拓扑序列.

  - 求拓扑序列:
    1. 定义队列Q，将所有入度为0的结点入队。
    2. 取队首结点，删去从它出发的所有边，令这些边到达的顶点入度-1，如果某顶点入度减为0，加入队列。
    3. 重复2.直到队列为空,如果队列为空时,入过队的结点数恰好为N,说明拓扑排序成功.G为有向无环图,否则失败,图中有环.

- 关键路径:

  - AOV网:顶点表活动,边集表活动优先关系,图中不能有环

  - AOE网:带权边集表活动,顶点表事件.事件仅代表一中介状态.

  - 如果给定AOV网中各顶点活动所需时间,那么可将AOV网转为AOE网,

    方法:将AOV网中每个顶点都拆成两个顶点,分别表示活动的起点和终点,两顶点间用有向边连接,该有向边表示原顶点的活动,边权给定,原AOV网中边全部视为空活动,边权为0.

  - AOE网中最长路径被称为关键路径.

  - 最长路径:对一个没有正环的图,如求最长路径长度,可将所有边权乘-1,使用Bellman-Ford或SPFA算法求最短路径长度.

  - 关键路径:求有向无环图(DAG)中最长路径

    e[r]:活动$a_r$的最早开始时间

    l[r]:活动$a_r$的最迟开始时间

    ==e[r]=l[r]时,$a_r$为关键活动==

    $事件V_i \xrightarrow[]{活动a_r}事件V_j$

    ve[i]:事件$v_i$的最早发生时间

    vl[i]:事件$v_i$的最迟发生时间

    e[r]=ve[i]

    l[r]=vl[j]-length[r]

    ve[j]=ve[i1]+length[r1]~ve[ik]+length[rk]中的最大值(拓扑)

    vl[i]=vl[j1]-length[r1]~vl[jk]-length[rk]中的最小值(逆拓扑)

    思路:先求点,再夹边.

    1. 按拓扑序和逆拓扑序分别计算给顶点(事件)的最早发生时间和最迟发生时间:

       最早(拓扑序):$ve[j]=\underset{i,边i\to j存在}{\max}\{ve[i]+\mathrm{length}[i\to j]\}$

       最迟(逆拓扑序):$vl[i]=\underset{j,边i\to j存在}{\min} \{ vl[j]- \mathrm{length} [i \to j] \}$

    2. 用上面的结果计算各边(活动)的最早开始时间和最迟开始时间:

       最早:e[i->j]=ve[i]

       最迟:l[i->j]=vl[j]-length[i->j]

    3. e[i->j]=l[i->j]即为关键活动.

  - 如事先不知道汇点编号,取ve数组最大值为汇点.(因为ve数组的含义就是事件的最早开始时间)

## 动态规划专题

- 一个问题必须拥有重叠子问题,才能使用动态规划解决.

- 递推写法:自底向上,即从边界开始,不断向上解决问题,直到解决目标问题

  递归写法:自顶向下,即从目标问题开始,将它分解成子问题的组合,直到分解至边界.

- 一个问题必须拥有最优子结构,才能用动态规划解决

- 分治与动态规划:分治解决的子问题是不重叠的,解决的问题不一定是最优化问题

- 对能划分阶段的问题来说,都可以尝试把阶段作为状态的一维.如果当前设计的状态不满足无后效性,可将状态升维,即增加一维或若干维表示相应的信息,这样可能满足无后效性.

- 常见问题

  - 最大连续子序列和:d[i]表示以A[i]为末尾的连续序列最大和.
  - 最长不下降子序列(LIS):d[i]表示以A[i]结尾的最长不下降子序列长度.
  - 最长公共子序列(LCS):dp[i]\[j]表字符串A的i位和串B的j号位之前的LCS长度.
  - 最长回文子串:dp[i]\[j]表S[i]至S[j]所表示的子串是否是回文子串.
  - 数塔DP:dp[i]\[j]表从i行第j个数出发的到最底层的所有路径上所能得到最大和
  - DAG最长路:dp[i]表从i号顶点出发得的最长路径长度
  - 01背包:dp[i]\[v]表示前i件物品恰好装入容量v的背包中能得到的最大价值.
  - 完全背包:dp[i]\[v]表前i件物品恰好放入容量v的背包中能得的最大价值.

- 子序列可能不连续,子串必须连续

## 字符串专题

- 字符串hash:

  - $H[i]=(H[i-1]\times p+\mathrm{index}(\mathrm{str}[i]))\% \mathrm{mod}$

    进制数p为$10^7$级别的素数(如$10000019$),mod为$10^9$级别素数(如$1000000007$),str[i]为字符串i号位.

  - $H[i\cdots j]=((H[j]-H[i-1] \times p^{j-i+1})\%\mathrm{mod}+\mathrm{mod})\%\mathrm{mod}$

  - 最长公共子串长:分别求每个子串的hash(同时记录长度)然后找出两堆子串对应的hash值中相等的那些.

  - 最长回文子串:字符串hash+二分法

    先求其hash数组H1,再将str反转,求hash数组H2,分奇偶讨论:

    - 奇长度回文串:枚举回文中心点i,二分子串半径k,判断str的[i-k,i]与rstr的[len-1-(i+k),len-1-i]
    - 偶长度回文串:枚举回文中心点i,二分子串半径k,判断str[i-k+1,i]与rstr[len-1-(i+k),len-1-(i+1)]
    - 即奇:H1[i-k....i]与H2[len-1-(i+k)....len-1-i]；偶:H1[i+k+1....i]与H2[len-1-(i+k)....len-1-(i+1)]

- KMP:判断字符串pattern是否是字符串text的子串

  - next数组:next[i]即求子串s[0......i]最长相等前后缀中==前缀最后一位==的下标.

    每次求出next[i]时,总是让j指向next[i]

  - 求解过程(求解next数组就是pattern自我匹配的过程)

    1. 初始化next数组,令j=next[0]=-1
    2. 让i在1~len-1的范围遍历,对每个i,执行3.4.,以解next[i]
    3. 不断令j=next[j],直到j回退到-1或s[i]=s[j+1]成立
    4. 如果s[i]=s[j+1],则next[i]=j+1,否则next[i]=j.

  - next数组含义:当j+1位失配时,j应回退的位置.

  - KMP算法思路:

    1. 初始化j=-1,表示pattern当前已被匹配的最后位.
    2. 让i遍历文本串text,对每个i,执行3.4.来试图匹配text[i]和pattern[j+1]
    3. 不断令j=next[j],直到j回退为-1或text[i]=pattern[j+1]
    4. 如果text[i]=pattern[j+1],则令j++,如果j到达m-1,说明pattern是text的子串,返回true

  - nextval数组:nextval[i]含义为pattern的i+1位失配时,i应当回退的最佳位置.

  - > [KMP算法专题]({% post_url /basics/2022-07-29-KMP %})

## 专题拓展

- 分块思想

  - 对一个有N个元素的有序序列来说,除最后一块外,其余每块中元素的个数都应为$\lfloor \sqrt{N} \rfloor$,块数为$\lceil \sqrt{N} \rceil$.

    这样把有序序列分为$\lceil \sqrt{N} \rceil$块,每块元素个数不超过$\lfloor \sqrt{N} \rfloor$.

  - 设一hash数组,`table[100001]`,其中`table[i]`表整数x的当前存在个数,设统计数组`block[317]`,其中`block[i]`表第i块中存在的元素个数.

- 树状数组(BIT):

  - lowbit运算:`lowbit(x)=x&(-x)`,即取x的二进制最右边的1和它右边所有的0,也可以理解为能整除x的最大2的幂次.

  - 解决前x个整数之和问题:给定序列A,元素个数N,查询k次

    - 设一数组`C[i]`来记录和,表存放在i号位之前(含i)lowbit(i)个整数之和.则`C[i]`的覆盖长度为`lowbit(i)`(也可以理解为管辖范围),且下标必须从1开始.

    - 记SUM(1,x)=A[1]+...+A[x] ,又因为C[x]=A[x-lowbit(x)+1]+...+A[x] (即末端坐标x-长度lowbit(x)+1) 故:SUM(1,x)=SUM(1,x-lowbit(x))+C[x],这样就把SUM(1,x)转化为SUM(1,x-lowbit(x))

    - 若求下标在[x,y]内的数的和,即A[x]+A[x+1]+...+A[y],则可用`getSum(y)-getSum(x-1)`求解.

    - ```c++
      //getSum返回前x个整数和
      int getSum(int x){
          int sum=0; //记录和
          for(int i=x;i>0;i-=lowbit(i)){
              sum+=c[i];
          }
          return sum;
      }
      ```

  - 设计函数`update(x,v)`实现第x个数加上v的功能

    - 让x不断加上lowbit(x),并让每步的C[x]都加上v,直到x超出范围.

    - ```c++
      //update将第x个整数加上v
      void update(int x, int v){
          for(int i=x;i<=N;i+=lowbit(i)){  //i必须能取到N
              c[i]+=v; //让c[i]加上v,然后让c[i+lowbit(i)]加上v
          }
      }
      ```

  - 给定一个有N个正整数的序列A,对序列中的每个数,求它左边比它小的数的个数:

    - hash[x]记录整数x当前出现次数
    - 序列中在A[i]左边比A[i]小的个数等于hash[1]+hash[2]+...+hash[A[i]-1],可以通过`update(A[i],1)`,`getSum(A[i]-1)`求得.

  - 离散化:求{520,999999999,18,666,88888}若只考虑大小关系,其与{2,5,1,3,4}等价,即可将A[i]与1~N对应起来,方法与排名问题一致.离散化只适用于离线查询.

  - 求序列第K大:对一个序列来说,如果用hash数组记录每个元素出现的个数,那么序列第K大就是在求最小的i,使得hash[1]+...+hash[i]$\geqslant$K成立,其等价于寻找第一个满足条件"getSum(i)$\geqslant$K"的i.

    ```c++
    int findKthElement(int k){
        int l=1, r=MAXN, mid;//初始区间为[1,MAXN]
        while(l<r){
            mid=(l+r)/2;
            if(getSum(mid)>=K) r=mid; //所求位置不超过mid
            else l=mid+1;
        }
        return l; //返回二分法夹出的元素
    }
    ```

  - 若求A[1]\[1]~A[x]\[y]这个子矩阵中所有元素之和,及A[x]\[y]加上整数v.则将`update`和`getSum`中的`for`循环改成双循环即可.

    求A[a]\[b]~A[x]\[y]这个子矩阵的元素之和,只需计算`getSum(x,y)-getSum(a-1,y)-getSum(x,b-1)+getSum(a-1,b-1)`

  - 以上为单点更新,区间查询.若要区间更新,单点查询,C[i]不在表示区间元素和,而表示区间内每个数当前被加了多少,原先的`update`作为`getSum`,原先的`getSum`作为`update.`如果让A[x]~A[y]每个数加上v,则先后执行`update(y,v)` `update(x-1,-v)`即可. 

<p align="center">---END---</p>

写完这篇算法笔记的总结已经是2021年的8月31日了,虽然仅有39页B5纸.却承载了我很多记忆,从中也学到了很多,也算是我正式算法的启蒙书籍了.距离我这次重新翻开算法笔记 把它写到博客上也已经是过去了1年多了,再回首,昔日书中的点点滴滴也依稀能记得起来,看来书还是要常看,习还是要多学.

<p align="right">2022-9-9</p>