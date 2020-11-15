

vim ：%y*  ：%y+  这两个一个是全部复制到vim的剪切板，一个是全部复制到系统剪切板



```c
#include <stdio.h>

int main(){
    char str[100] = {0};
    while(scanf("%[^\n]s", str) != EOF){
        getchar();
        printf(" have %d char!\n",printf("%s", str));

    }
    return 0;
}
```

```c
#include <stdio.h>
#define swap(a, b){ \
    __typeof(a) __temp = a; \
    a = b; b = __temp; \
}

int main(){
    int n;
    scanf("%d", &n); //stdin
    printf("%d\n", n); //stdout  stderr
    char str[1000] = {0}, buffer[100] = {0}, *p = str, *q = buffer;
    sprintf(str, "%d.%d.%d.%d", 192,168,0,1);
    printf("str = %s\n", str);
    if(n & 1){       
        sprintf(q, "(%s)", p);
        swap(q, p);
    }
    if(n & 2){
        sprintf(q, "(%s)", p);
        swap(q, p);
    }
    if(n & 4){
        sprintf(q, "[%s]", p);
        swap(q, p);
    }
    printf("%s\n", p);
    FILE *fout = fopen("output", "w");
    fprintf(stdout, "stdout = %s\n", p);
    fprintf(stdout, "stderr = %s\n", p);
    return 0;
}

```



# c语言的数学函数库

头文件：math.h

**常用函数：**

pow(a,n);指数函数  

sqrt(n);开平方函数 

ceil(n);上取整函数  floor(n);下取整函数  

abs(n);(stdlib.h)整数去绝对值函数 fabs(n);实数取绝对值函数 

log(n);以e为底绝对值函数 log(10);以10为底对数函数 

acos(n);（arccos（x））

inttypees.h  固定宽度整数类型，增加系统的可移植性

```c++
#include <stdio.h>
#include <inttypes.h>

int main () {
	int32_t a = 70000;
    printf("%s\n", PRId32);
    printf("%s\n", PRId64);
    printf("%s\n", PRId16);
    printf("%s\n", PRId8);
    printf("%" PRId32 "\n", a);
    printf("INIT8_MIN =" "%" PRId8 "INT8_MAX =" "%" PRId8 "\n", INIT8_MIN, INIT8_MAX);
    ptintf("INIT16_MIN =" "%" PRId16 "INIT16_MAX =" "%" PRId16 "\n", INIT16_MIN, INIT16_MAX);
    printf("INIT32_MIN = " "%" PRId32 "INIT32_MAX =" "%" PRId32 "\n", INIT32_MIN, INIT32_MAX);
    printf("INIT64_MIN = " "%" PRId64 "INIT64_MAX = " "%" PRId64 "\n", INIT64_MIN, INIT64_MAX);
    return 0;
}
```

回文整数

```c++
bool isPalindrome(int x){
	if(__builtin_expect(!!(x < 0), 0)) return false;
    int y = 0, z = x;
    while(x){
        y = x % 10 + y * 10;
        x /= 10;
    }
    return z == y;
}
```

`#define likely(x)  __builtin__expect (!!(x), 1);`

`#define unlinkely(x) __builtin__expect(!!(x), 0);`  

//likely 代表x经常成立，加载条件分支的内部代码

//unlikely 代表x不经常成立，加载条件分支的外部代码

cpu的任务机制是多路处理，在做分支结构的时候会进行分支预测，预测错误会浪费大量的时间。   

__builtin_ffs(x): 返回x中最后一个为1的位是从后向前的第几位

__builtin_popcount(x):x中1的个数

__builtin_ctz(x): x末尾0的个数 x=0时结果未定义

__builtin_clz(x): x前导0的个数 x=0时结果未定义

__builtin_prefetch(const void *addr) : 对手工预取的方法

__builtin_types_compatible_p(type1,type2):判断type1和type2是否是相同的数据类型

__builtin_expext(long exp, long c): 用来引导gcc进行条件分支预测

__built_constant_p(exp):判断exp是否在编译时就看i一确定其为常量

__builtin_parity(x):x中的奇偶性

__builtin_return_address(n):当前函数的第n级调用者的地址

质数判断循环for（int i = 2; i * i <= n; ++i）

```c

#include <stdio.h>
#include <stdlib.h>
#include <time.h>

int main() {
    int a, b;
    srand(time(0));//随机种子
    scanf("%d%d", &a, &b);
    if(a - b) {
        printf("no equal!\n");
    } else {
        printf("equal!\n");
    }
    a = 0, b = 0;
    if ((a++) && (b++)) {
        printf("true\n");
    } else {
        printf("false\n");
    }
    printf("a = %d, b = %d\n", a, b);
    if ((a++) || (b++)) {
        printf("true\n");
    } else {
        printf("false\n");
    }
    printf("a = %d, b = %d\n", a, b);
    for(int i = 0; i < 10; i++) {
        i == 0 || printf(" ");
        printf("%d", i);
    }
    printf("\n");
    int cnt = 0;
    for (int i = 0; i < 5; i++) {
        int val = rand() % 100;//随机取值的使用
        i && printf(" ");
        printf("%d", val);
        cnt += (val & 1);
    }
    printf("\n");
    printf("odd : %d\n", cnt);
    return 0;
}
```

```c
#include <stdio.h>
//是否为回文数
int is_reverse(int n, int base) {
    if (n < 0) return 0;
    int sum = 0, x = n;
    while (x) {
        sum = sum * base + x % 10;
        x /= base;
    }
    return sum == n;
}
//是否为质数
int is_prime(int n) {
    for (int i = 2; i <= n; i++) {
        if(n % i == 0) return 0;
    }
    return 1;
}

int main() {
    int n, x, dight = 0;
    scanf("%d", &n);
    x = n;
    do {
        dight++;
        x /= 10;
    }while(x);
    printf("%d has %d dights!\n", n, dight);
    printf("%s\n", is_reverse(n, 10) ? "yes" : "no");
    printf("%s is prime!\n", is_prime(n) ? "yes" : "no");
    return 0;
}
```

**函数指针**

```c++
int g(int (*f1)(int), int (*f2)(int), int (*f3)(int), int x) {
	if (x < 0) {
		return f1(x); 
	}
    if (x < 100) {
        return f2(x);
    }
    return f3(x);
}
```

算法是聪明人解决问题的方法

# 辗转相除法

又名欧几里得法   迄今为止最古老的算法

用于快速计算两个数的最大公约数，快速求a*x+b*y=1的一组整数解

![image-20200929212238458](../linux系统编程笔记/image/image-20200929212238458.png)

![image-20200929212247157](../linux系统编程笔记/image/image-20200929212247157.png)



**辗转相除法  又名欧几里德法**

用于快速计算两个数的最大公约数的值

快速求解a*x + b*y = 1方程的一组整数解

```c++
#include <stdio.h>

int gcd(int a, int b){
	return (b ? gcd(b, a % b) : a);//
}

int main() {
    int a, b;
    while(~scanf("%d%d", &a, &b)) {
        printf("%d\n", gcd(a, b));
    }
    return 0;
}
```

**扩展欧几里得**

a*x + b*y =0;    a或b为0   

```c
#include <stdio.h>

int ex_gcd(int a, int b, int *x, int *y) {
    if (!b) {
        *x = 1, *y = 0;
        return a;
    }
    int xx, yy , ret = ex_gcd(b, a % b, &xx, &yy);
    *x = yy;
    *y = xx - a / b *yy;
    return ret;
}

int main() {
    int a, b , x, y;
    while (~scanf("%d%d", &a, &b)) {
        printf("gcd(%d, %d) = %d\n", a, b, ex_gcd(a, b, &x, &y));
        printf("%d * %d + %d * %d = %d\n", a, b, x, y, a * x + b * y);
    }
    return 0;
}

```



# **变参函数  va家族**

实现可变参数max_int ,从若干个传入的参数中返回最大值

int max_int (int a, ...);

![image-20200923201931384](/image/image-20200923201931384.png)

```c++
#include <stdio.h>
#include <stdarg.h>
int max_int(int n, ...){
	if(n < 0) return 0;
    int ans = 0;
    va_list arg;
    va_start(arg, n);
    while(n--){
        int temp = va_arg(arg, int);
        if(temp > ans) ans = temp;
    }
    va_end;
    return ans;
}
int main() {
    printf("%d\n", max_int(3, 1, 2 5));
    printf("%d\n", max_int(2, 1, 7, 17));
    printf("%d\n", max_int(3, 3, 6, 18));
    return 0;
}
```

```c
#include <stdio.h>
#include <stdarg.h>
#include <inttypes.h>


int reverse_num(int num, int *temp) {
    int dight = 0;
    do {
        *temp = *temp * 10 + num % 10;
        num /= 10;
        dight++;
    }while(num);
    return dight;
}

int output_num(int num, int dight) {
    int cnt = 0;
    while (dight--) {
        putchar(num % 10 + 48);
        num /= 10;
        cnt++;
    }
    return cnt;
}

int my_printf(const char *frm, ...) {
    va_list arg;
    va_start(arg, frm);
    int cnt = 0;
    #define PUTC(a) putchar(a),++cnt;
    for (int i = 0; frm[i]; i++) {
        switch(frm[i]) {
            case '%': {
                switch(frm[++i]) {
                    case '%': {
                        PUTC(frm[i]);
                    }break;
                    case 'd': {
                        int xx = va_arg(arg, int);
                        uint32_t x;
                        if (xx < 0) {PUTC('-'); x = -xx;}
                        else x = xx;
                        int num1 = x / 100000, num2 = x / 100000;
                        int temp1 = 0, temp2 = 0;
                        int dight1 = reverse_num(num1, &temp1);
                        int dight2 = reverse_num(num2, &temp2);
                        if (num1) dight2 = 5;
                        else dight2 = 0;
                        cnt += output_num(temp1, dight1);
                        cnt += output_num(temp2, dight2);
                    }break;
                    case 's': {
                        const char *str = va_arg(arg, const char *);
                        for (int i = 0; str[i]; i++) {
                            PUTC(str[i]);
                        }
                    }break;
                }
            }break;
            default:PUTC(frm[i]);break;
        }
    }
    #undef PUTC
    va_end(arg);
    return cnt;
}

int main() {
    int a = 123;
    printf("hello world!\n");
    my_printf("hello world!\n");
    printf("int a = %d\n", a);
    my_printf("int a = %d\n", a);
    printf("int a = %d\n", 1000);
    my_printf("int a = %d\n", 1000);
    printf("int a = %d\n", 0);
    my_printf("int a = %d\n", 0);
    return 0;
}
```



# **素数筛算法**



![image-20201011204529858](../linux系统编程笔记/image/image-20201011204529858.png)

线性筛的过程就是用m来枚举标记合数，p为最小的素因子，m为除n外最大的因子

```c
//素数筛
#include <stdio.h>
#define MAX_N 100

int prime[MAX_N + 5];

void init() {
    for (int i = 2; i <= MAX_N; i++) {
        if(prime[i]) continue;
        prime[++prime[0]] = i;
        for (int j = i * i; j <= MAX_N; j += i) {
            prime[j] = 1;
        }
    }
    return ;
}

int main() {
    init();
    for (int i = 1; i <= prime[0]; i++) {
        printf("%d\n", prime[i]);
    }
    return 0;
}

```

```c

#include <stdio.h>
#define MAX_N 100

int prime[MAX_N + 5];

void init() {
    for (int i = 2; i <= MAX_N; i++) {
        if(prime[i]) continue;
        for (int j = i; j <= MAX_N; j += i) {
            if(prime[j]) continue;
            prime[j] = i;
        }
    }
}


int main() {
    init();
    for (int i = 2; i <= MAX_N; i++) {
        printf("min_factor(%d) = %d\n", i, prime[i]);
    }
    return 0;
}

```



```c

#include <stdio.h>
#define MAX_N 100

int prime[MAX_N + 5];

void init() {
    for (int i = 2; i <= MAX_N; i++) {
        if (!prime[i]) prime[++prime[0]] = i;
        for (int j = 1; j <= prime[0]; j++) {
            if(prime[j] * i > MAX_N) break;
            prime[prime[j] * i] = 1;
            if(i % prime[j] == 0) break;
        }
    }
}

int main() {
    init();
    for (int i = 1; i <= prime[0]; i++) {
        printf("%d\n", prime[i]);
    }
    return 0;
}

```







源文件-> 预处理->编译->对象文件（xx.o）->链接->可执行文件

数组是展开的函数，函数是压缩的数组

牛顿迭代

# **预处理命令-宏定义**（代码替换）

#include  

#define

定义符号常量   #define PI 3.114115926

定义傻瓜表达式   #define s(a，b)  a * b

定义代码段   #define  P（a） {   \

​	printf（“%d\n”, a）;   \

}

**预处理命令-预定义的宏**

```
__DATE__ 日期：Mmmddyy 程序编译的时间
__TIME__  时间：hh：mm：ss
__LINE__  行号
__FILE__  文件名
__func__  函数名（非标准）
__FUNC__  函数名 （非标准）
__PRETTY_FUNCTION__  更详细的函数信息 （非标准）
```

**条件式编译**

```
代码的剪裁
#ifdef DEBUG  是否定义了DEBUG宏
#ifndef DEBUG	是否定义了DEBUG宏
#if MAX_N == 5	宏MAX_N是否等于5
#elif MAX_N == 4 否则宏MAX_N是否为4
#else
#endif
```

![image-20201012200323366](../linux系统编程笔记/image/image-20201012200323366.png)

注意：在宏定义时 注意空格的使用

# **字符串**

字符串相关操作：

![image-20201012211009104](../linux系统编程笔记/image/image-20201012211009104.png)

字典序排序

memset是把每个字节设置成c

![image-20201012212246700](../linux系统编程笔记/image/image-20201012212246700.png)



结构体

->间接引用   . 直接引用

宏 强行改变对齐的大小

共用体  union —— {}

大端  数据的低位对应地址的高位

小端 数据的高位对应地址的低位



函数指针

int (*add) (int, int)

typedef int (*add) (int , int) 定义类型

![image-20201013083726576](../linux系统编程笔记/image/image-20201013083726576.png)





![image-20201013084441386](../linux系统编程笔记/image/image-20201013084441386.png)



预处理命令 

#ifndef  常用的代码风格  `工程名_路径名_文件名_H_`

#define  

#endif

用于避免头文件重复引入

# gdb的基本使用

要充分的利用 gdb 帮助我们找到程序编写时候的问题，我们需要在编译时使用`-g`作为一个编译参数（否则你将看不见函数名、变量名，而只能看到运行时的内存地址），例如：

`gcc -o program -g main.c`

我们在通过 gdb ./program

- 如果你输入`l`（list 的首字母），gdb 会列出带着行号的 1010 行程序，你可以根据这个行号进行进一步的调试。`l`之后可以加行号作为参数，列出这一行开始的 1010 行（例如希望列出第 11 到 1010 行时，我们可以写`l 1`）。
- 如果你输入`b`（breakpoint 的首字母），则表示设置程序运行的断点，程序运行到断点时就会暂停运行。`b`之后既可以加函数名作为参数（例如当有一个函数名为 func 时，我们可以写`b func`），使程序在调用某一函数时暂停；也可以加行号作为参数（例如`b 5`表示在第五行设置一个断点），使程序在运行某一行时暂停。
- 如果你输入r（run 的首字母），程序则会开始运行，并且暂停在我们设置了断点的位置。
  - 运行暂停时，我们如果输入`p 表达式`（print 的首字母） 则表示我们希望在当前断点运行这个表达式并查看它的值。例如`p ++age[0]`表示我们希望让`age[0]`自增 11 并查看自增后的值。注意：这个表达式会对之后的程序继续运行造成影响。
  - 运行暂停时，如果我们输入`n`（next 的首字母）程序会执行暂停位置后的下一条语句并再次暂停。
  - 运行暂停时，如果我们输入`c`（continue 的首字母）程序会继续执行到下一关断点再暂停（如果没有断点就会执行直到结束）。

makefile 

目标文件：依赖文件

【TAB】命令



