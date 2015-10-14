title: 浅析struct与union
tags:
  - C语言
  - struct
  - union
id: 145
categories:
  - C/C++
date: 2013-10-12 08:13:58
---

Struct与union作为C语言的关键字，定义与用法这里就不说了。下面主要针对几点进行说明。

### 1.Struct与内存对齐

先看一下下面三个结构体，在不实验的情况下能确定的说出他们的大小？
<pre>[cce_cpp]
struct test1
{
    char a;
    char c;
    short b;
};

struct test2
{
    char a;
    short b;
    char c;
};

struct test3
{
    char a;
    int b;
    char c;
};
[/cce_cpp]</pre>
编译器会自动进行成员变量的对齐，以提高运算效率。缺省情况下，编译器为结构体的每个成员按其自然对界（natural alignment）条件分配空间。各个成员按照它们被声明的顺序在内存中顺序存储，第一个成员的地址和整个结构的地址相同。
<!--more-->则在32位机器上，我们可以知道上述三个结构体的大小分别为4，6，12。为什么要进行内存对齐呢，原因在于，为了访问未对齐的内存，处理器需要作两次内存访问；然而，对齐的内存访问仅需要一次访问。所以为了内存的有效利用，我们有必要在定义结构体前考虑下成员的顺序。字，双字，和四字在自然边界上不需要在内存中对齐。（对字，双字，和四字来说，自然边界分别是偶数地址，可以被4 整除的地址，和可以被8 整除的地址。）
<pre>除此之外我们还可以利用#pragma pack（）来改变编译器的默认对齐方式。使用伪指令#pragma pack (n)，编译器将按照n个字节对齐；使用伪指令#pragma pack ()，取消自定义字节对齐方式。注意：如果
#pragma pack (n)中指定的n大于结构体中最大成员的size，则其不起作用，结构体仍然按照size最大的成员进行对界。其对齐的规则是,每个成员按其类型的对齐参数(通常是这个类型的大小)和指定对齐参数
(这里是n 字节)中较小的一个对齐，即：min( n, sizeof( item ))</pre>

### 2\. struct内部可以定义函数吗

答案是对于C++, struct内部可以定义函数，而对于C，struct内部只能定义函数指针。
<pre>[cce_cpp]
//c
struct test2
{
    char a;
    short b;
    char c;
    int (*fun)();
};

//c++
struct test2
{
    char a;
    short b;
    char c;
    void fun()
    {
        cout &lt;&lt; "hello world" &lt;&lt; endl;
    }
};

[/cce_cpp]</pre>

### 3.struct与class的区别

在C++中，struct与class的区别有两点，一是struct中成员变量和函数的默认访问权限为public，而class的为private。二是由于C++中struct兼容了C中的struct的所有特性，可以定义的时候直接
以{ }对其成员变量赋初值，而class则不能。

### 4.union与存储模式

概括一下union：union 维护足够的空间来置放多个数据成员中的“一种”，而不是为每一个数据成员配置空间，在union 中所有的数据成员共用一个空间，同一时间只能储存其中一个数据成员，所有的数据成员具有相同的起始地址。union大小为一个数据成员的最大长度。
所谓存储模式也就是我们常说的大小端模式。大端模式（Big_endian）：字数据的高字节存储在低地址中，而字数据的低字节则存放在高地址中。小端模式（Little_endian）：字数据的高字节存储在高地址中，而字数据的低字节则存放在低地址中。
而union与存储模式有什么关系呢，其实也没什么关系，只是我们可以通过union来编写程序确定当前系统的存储模式。
<pre>[cce_cpp]
int endian()
{
    union biglittle
    {
        int i;
        char ch;
    }c;
    c.i = 1;
    return c.ch==1;
}
//函数返回1则表示c.i写入低字节，也就是小端模式，否则为大端模式。
[/cce_cpp]</pre>
&nbsp;