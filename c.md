# C

## 素数

### 构造素数表

```c
#include <stdio.h>

int main(int argc, char const *argv[])
{
    const int number = 100;
    int isPrime[number] = {0};

    for (int i = 0; i < number; ++i)
    {
        isPrime[i] = 1;
    }

    // 如果x为素数则将下标小于构造范围的i*x的值设为0;
    for (int x = 2; x < number; ++x)
    {
        if (isPrime[x])
        {
            for (int i = 2; i*x < number; ++i)
            {
                isPrime [i*x] = 0;
            }
        }
    }

    // 如果数字中所对应下标值为1则输出下标
    for (int i = 2; i < number; ++i){
        if (isPrime[i]){
            printf("%d\t",i);
        }
    }

    return 0;
}
```

### 使用已有的素数来判断一个数是否为素数

```c
#include <stdio.h>
#include <math.h>

// 声明函数原型
int isPrime(int number,int knowPrimes[],int numberOfKnowPrines);

int main(int argc, char const *argv[])
{

    const int number = 100;
    int prime[number] = {2};
    int count = 1;
    int i = 3;

    while(count < number){
        if (isPrime(i,prime,count))
        {
            prime[count++] = i;
        }
        i++;
    }

    for (int i = 0; i < number; ++i)
    {
        printf("%d\t",prime[i] );
        if ((i+1) % 5 ==0 )
        {
            printf("\n");
        }
    }

    return 0;
}

int isPrime(int number,int knowPrimes[],int numberOfKnowPrines)
{
    int ret = 1;

    for (int i = 0; i < numberOfKnowPrines; ++i)
    {
        if (number % knowPrimes[i] == 0)
        {
            ret = 0;
            break;
        }
    }
    return ret;
}
```

### 根据下标判断是否为素数

```c

#include <stdio.h>
#include <math.h>

// 声明函数原型
int isPrime(int number);

int main(int argc, char const *argv[])
{
    return 0;

}
// 返回1为素数,反之则不为素数;
int isPrime(int number)
{
    int ret = 1;
    int i;

    if ( number == 1 || (number % 2 ==0 && number != 2) ){
        ret = 0;
    }

    for (int i = 3; i < sqrt(number); i+=2 ){
        if (number % i ==0)
        {
            ret = 0;
            break;
        }
    }

    return ret;

}
```


## 存储类

* auto存储类是所有局部变量默认的存储类
* register存储类用于定义存储在寄存器中而不是RAM中的局部变量
* static存储器指示编译器在程序的生命周期内保持局部变量的存在
* extern存储类用于提供一个全局变量的引用，全局变量对所有的程序文件都是可见的

## 枚举

### 声明枚举类型

```c
enum DAY
{
    MON=1,TUE,WED,THU,FRI,SAT,SUN
};
```

### 定义枚举变量

```c
enum DAY day;

```

###声明枚举类型的同时定义枚举变量

```c
enum DAY{
    MON=1,TUE,WED,THU,FRI,SAT,SUN
} day;
```

## 指针

* 指针变量必须赋予变量的地址后使用;
* 使用*p来访问地址的值;
* 使用&p来获取地址;
* 当传入较大数据时使用指针作为参数;
* 数组变量本身就是地址,是一种const的指针;
* *p取值相当于p[0];
* const指针一旦得到了某个变量的地址不能再指向其他变量;
* const int* p1 = &i; *p1 = 26; //error 指针指向的值不能通过指针修改;
* int* const p3 = &i; //指针不能被修改;
* 可以将非const的指针传入参数为const类型的函数中,则函数中指针指向的值不能通过指针修改;
* const int array[]表示该数组不能指向别的数组且里面的值不能被修改,只能通过初始化赋值;
* p+1等于原本的内存地址加上sizeof(指针所指的数据类型)的大小;
* 不同类型指针不能相互赋值但可以强制类型转换;
* 使用void*来表示未知类型的指针;
* int* a = (int*) malloc(n*sizeof(int)); //需要分配n个int类型大小的内存空间;



### 比大小

```c
include <stdio.h>

void minmax(int array[],int len,int *max,int *min);

int main(int argc, char const *argv[])
{
    int array[] = {1,3,5,7,3,6,22,6,11,3,15};
    int min;
    int max;
    minmax(array,sizeof(array)/sizeof(array[0]),&max, &min);
    printf("min:%d max:%d\n",min,max);

    return 0;
}

void minmax(int array[],int len,int *max,int *min)
{

    *max = array[0];
    *min = array[0];
    for (int i = 0; i < len; ++i)
    {
        if (*max < array[i])
        {
            *max = array[i];
        }

        if (*min > array[i])
        {
            *min = array[i];
        }
    }
}
```

### 除法

```c
#include <stdio.h>


int divide(int a,int b,int *result);

int main(int argc, char const *argv[])
{
    int a = 5;
    int b = 2;
    int result;
    if (divide(a,b,&result))
    {
        printf("%d/%d=%d\n",a,b,result);
    }

    return 0;
}

int divide(int a,int b,int *result){
    int ret = 1;
    if (b == 0)
    {
        ret = 0;
    }else{
        *result = a/b;
    }
   return ret;
}
```

### 输出数组

```c
    int array[] = {1,2,3,-1};

    int *p =  array;

    while(*p != -1){
        printf("%d\n", *p++);
    }

    ---------------------------------------

     for (*p = array; *p != -1; p++){
        printf("%d\n",*p);
    }

```

### 动态分配内存

```c
#include <stdio.h>
#include <stdlib.h>

int main(int argc, char const *argv[])
{

    int number = NULL;
    int* a = NULL;
    int i = NULL;
    printf("%s","please enter number:");
    scanf("%d",&number);

    a = (int*) malloc(number*sizeof(int));


    for (int i = 0; i < number; ++i)
    {
        scanf("%d",&a[i]);
    }

    for (int i = number-1; i >= 0 ; i--)
    {
        printf("%d\n",a[i] );
        printf("%p\n",&a[i] );
    }
    free(a);

    return 0;
}
```

## 字符串

* const char* string = ""; //只读 如果指向相同字符的char数组则内存地址一致；
* char string[] = ""; // 本地变量会被自动回收；
* char string[10] = "";
* char* string[] = {"",} // 字符串数组

### mylen

```c
int mylen(const char* s){

    int index = 0;

    while( s[index] != '\0'){
        index++;
    }
    return index;
}
```

### mycmp

```c
int mycmp(const char* s,const char* s1)
{
    while( *s == *s1 && *s != '\0'){
        s++;
        s1++;
    }

    return *s - *s1;
}
```

### mycpy

```c
// dst和src内存地址不能重叠
// char* dst = (char*)malloc(strlen(src)+1); // 给dst分配内存空间大小
char* mycpy(char* dst,const char* src )
{
    // int index = 0;
    // while(src[index]){
    //     dst[index] = src[index];
    //     index++;
    // }
    // dst[index] = '\0';

    // return dst;

    char* ret = dst;

    while(*src){
        *dst++ = *src++;
    }

    *dst = '\0';

    return ret;
}
```

### mycat

```c
char* mycat(char* dst,const char* src)
{
    int index = strlen(dst);
    int src_index = 0;

    while(src[src_index]){
        dst[index++] = src[src_index++];
    }

    return dst;
}
```


## 结构体

* 结构的指针不是结构名，必须要用&取得；
* 整个结构可以作为参数的值传入函数，这时候是在函数内新建一个结构变量，并复制调用者的结构的值；
* 结构可作为返回类型；
* p->month // 结构变量的指针指向结构成员

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

// 声明结构类型
struct date
{
    // 结构成员
    int month = NULL;
    int day = NULL;
    int year = NULL;
};


int main(int argc, char const *argv[])
{

    // 定义结构变量
    struct date today;

    today.year = 2020;
    today.month = 04;
    today.day = 2200;

    // struct  date today1 = {2020,04,22};

    // struct  data today2 = {.month=04,.year=2020};

    // today = (struct date){04,22,2020};


    return 0;
}
```

## 类型定义

* typedef int Length; // 将Length成为int类型的别名；

## 联合

* 所有成员共享一个空间；
* 同一时间只有一个成员是有效的；
* union的大小是其最大的成员；

```c
#include <stdio.h>


typedef union {
        int i;
        char ch [sizeof(int)];
} CHI;

int main(int argc, char const *argv[])
{

    CHI chi;

    chi.i = 1234;
    for (int i = 0; i < sizeof(int); ++i)
    {
        printf("%02hhX\n",chi.ch[i] );
    }
    printf("\n");

    return 0;
}
```

## 变量

* 全局变量的值不能为变量，除非为const；
* static本地变量只会初始化一次，离开函数时不会被回收；
* static本地变量和全局变量都存储同一片内存地址中；
* 返回本地变量的地址是危险的；
* 返回全局变量或静态本地变量的地址是安全的；
* 返回在函数内malloc的内存是安全的，但是容易造成问题；
* 最好的做法是返回传入的指针；

## 宏

* 宏的值可以包含其他的宏；
* 宏可以带参数； // 参数无类型；
* 宏的整个值要括号，有参数的所有地方要有括号；
* 宏的定义不要加分号；
* 宏不会进行类型检查；

```c
#define PI 3.14159 // 定义一个名字为PI 值为3.14159的宏 类似于const；
#define cube(x) ((x)*(x)*(x)) // 定义一个带参数的宏；
```

## 头文件

* 在调用和定义函数的.c文件中都应该include；
* 全局变量也可以在多个.c文件中共享；
* extern int gall // 在头文件中声明一个全局变量使其能在多个.c文件共享；


### 避免头文件重复引用

```c
#ifndef __MAX_H__
#define __MAX_H__

...

#endif
```

## 不对外的函数

* 在函数前面加上static就使得它成为只能所在的编译单元中被使用的函数；
* 在全局变量前面加上static就使得它成为只能在所在编译单元中被使用的全局变量；

## 格式化输入输出

* 可以使用<或>做程序的重定向；
* %[flags][width][.prec]【hlL】type

### flags

| flags | 注释 | width或prec | 注释 | hlL | 注释 | type | 注释 |
|-----|-----|-----|-----|-----|-----|-----|-----|
|  -  | 左对齐 | number | 最小字符数 | hh | 单个字节 | i或d | int |
|  +  | 在前面放+或- |  *  | 下一个参数是字符数 | h | short | u | unsigned int |
| space | 正数留空 | .number | 是小数点后的位数 | l | long | o | 八进制 |
|  0  | 0填充 | .* | 下一个参数是小数点后的位数 | ll | long long | x | 十六进制 |
|     |     |     |     |     |     | X | 大写的十六进制 |
|     |     |     |     |     |     | f或F | float,6 |
|     |     |     |     |     |     | e或E | 指数 |
|     |     |     |     |     |     | g | float |
|     |     |     |     |     |     | G | float |
|     |     |     |     |     |     | a或A | 十六进制浮点 |
|     |     |     |     |     |     | c | char |
|     |     |     |     |     |     | s | 字符串 |
|     |     |     |     |     |     | p | 指针 |
|     |     |     |     |     |     | n | 读入/读出个数 |

## 文件
* fread() // 以二进制方式读取
* fwrite() // 以二进制写入
* fseek() // 在文件中定位 //SEEK_SET：从头开始 SEEK_CUR：从当前位置开始 SEEK_END：从尾开始
* ftell() //得到现在所在的位置

### 打开文件的标准代码

```c
// r：打开只读 r+：从文件头开始打开读写
// w：打开只写，如果不存在则新建，存在则清空
// w+：打开读写，如果不存在则新建，存在则清空
// a：打开追加，如果不存在则新建，存在则从文件尾开始
// ..x：只新建，如果文件已存在则不能打开
FILE* fp = fopen("file",r);
if (fp)
{
    int num;
    fscanf(fp,"%d",&num);
    fclose(fp);
}else{
    ...
}
```

## 按位运算

* & // 与 1 & 1 = 1； 1 & 0 = 0；
* | // 或 1 | 1 = 1； 1 | 0 = 1；
* ^ // 异或 1 ^ 1 = 0; 1 ^ 0 = 1;
* ~ // 取反 ~ 1 = 0; ~ 0 = 1;

// i向左边移动j个位置，右边空出来的位置填0；所有小于int的类型，移位以int的方式来做，结果是int；

* <<    // 左移 i << j // 移动n位等价于乘以2的n次方；

// i向右边移动j个位置；对于unsigned类型，左边空出来的位置填0，对于signed类型，左边填入原本的最高位保持符号不变；

* \>\>    // 右移 i >> j // 移动n位等价于除以2的n次方；

### 输出二进制

```c

#include <stdio.h>

int main(int argc, char const *argv[])
{

    int number;
    scanf("d",&number);
    unsigned int mask = 1u<<31; // 让unsigned类型的1左移31位bit；
    for (; mask ; mask >>=1 ) // mask 从左到右移动并与number进行与运算，如果为1输出1，为0则输出0；
    {
        printf("%d\n", number & mask?1:0 );
    }
    printf("\n");

    return 0;
}
```

## 可变数组

### 主函数

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>

#ifndef _ARRAY_H_
#define _ARRAY_H_

typedef struct {
    int* array;
    int size;
} Array;

Array array_create(int init_size);
void array_free (Array* a);
int array_size(const Array* a);
int* array_at(Array* a,int index);
void array_inflate(Array* a,int more_size);

#endif

const BLOCK_SIZE = 5;


int main(int argc, char const *argv[])
{

    Array a = array_create(10);

    return 0;
}
```

### 创建数组

```c
Array array_create(int init_size){
    Array a;
    a.size = init_size;
    a.array = (int*)malloc(sizeof(int)*a.size);
    return a;
}
```

### 扩容数组

```c
void array_inflate(Array* a,int more_size)
{

    int* p = (int*)malloc(sizeof(int)*(a->size + more_size));
    for (int i = 0; i < a->size; ++i)
    {
        p[i] = a->array[i];
    }
    free(a->array);
    a->array = p;
    a->size += more_size;

}
```

### 数组大小

```c
int array_size(const Array* a)
{

    return a->size;
}
```

### 寻找元素


```c
int* array_at(Array* a,int index)
{

    if (index >= a->size)
    {
        array_inflate(a,(index/BLOCK_SIZE+1)*BLOCK_SIZE - a->size);
    }
    return &(a->array[index]);
}
```

### 清空数组


```c
void array_free(Array* a)
{
    free(a->array);
    a->array = NULL;
    a->size = 0;
}
```


## 链表

### 主函数

```c
#include <stdio.h>
#include <stdlib.h>


// 相当于LinkedNode
typedef struct  _node
{
    int value;
    struct _node* next;
} Node;


// 相当于LinkedList
typedef struct _list
{
    Node* head;
    Node* tail;
} List;

// 函数声明
void add_node(List* list,int number);
void print(List* pList);
int search(List* pList,int number);
void remove(List* pList,int number);

int main(int argc, char const *argv[])
{

    int number = 0;
    List list;
    list.head = NULL;
    list.tail = NULL;

    // 输入数字使其添加到链表中，输入-1结束；
    do{
        scanf("%d",&number);
        if (number != -1){
            add_node(&list,number);
        }
    }while (number != -1);

    return 0;
}
```

### 添加结点

```c
void add_node(List* pList,int number)
{
    // 创建一个结点
    Node* p = (Node*)malloc(sizeof(Node));
    p->value = number;
    p->next = NULL;

    // 尾结点 = 头结点
    pList->tail = pList->head;
    // 如果尾结点不为空
    if (pList->tail){
        // 该节点 = 该节点的下一个结点，直到链表的最后一个结点；
        while(pList->tail->next){
            pList->tail = pList->tail->next;
        }
        // 使尾结点的下一个结点 = p；相当于将p添加到链表的最后面；
        pList->tail->next = p;
    // 如果尾链表为空，则使头节点 = p；
    }else{
        pList->head = p;
    }
}

```

### 删除结点

```c
void remove(List* pList,int number)
{

    Node* q = NULL;

    for (Node* p = pList->head; p; q = p, p = p->next)
    {
        if (p->value == number)
        {
            // 如果头节点等于number 则q为NULL；
            if (q)
            {
                q->next = p->next;

            }else{
                pList->head = p->next;
            }
            free(p);
            break;
        }
    }
}
```

### 删除链表

```c
void clear(List* pList)
{
    // Node* p = pList->head;
    // Node* q = pList->head;

    // while(p){
    //     q = p->next;
    //     free(p);
    //     p = q;
    // }

    Node* p = NULL;
    Node* q = NULL;

    for (p = pList->head; p; p=q)
    {
        q = p->next;
        free(p);
    }
}
```

### 搜索结点

```c
int search(List* pList,int number)
{
    int ret = 0;

    for (Node* p = pList->head; p; p = p->next)
    {
        ret++;
        if (p->value == number)
        {
            break;
        }
    }
    return ret;
}
```

### 打印链表

```c
void print(List* pList)
{
    Node* p = pList->head;

    do{
        printf("%d\n",p->value);
    }while(p = p->next);

    // for (Node* p = pList->head;p;p=p->next)
    // {
    //     printf("%d\n",p->value );
    // }
}
```
