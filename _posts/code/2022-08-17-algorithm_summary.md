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

