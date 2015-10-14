title: CSAPP实验之数据实验
tags:
  - CSAPP，数据实验，右移，算术，逻辑
id: 67
categories:
  - 算法设计
date: 2013-02-28 10:18:54
---

CSAPP是以前在IBMTC大家对Computer Systems： A Programmer's Perspective的简称，中文名字是《深入理解计算机系统》。这本书我就不介绍了。最近拿出来温习一下，顺便把这本书上的实验再做一下。

实验主要有：数据实验，二进制炸弹实验，缓冲区溢出实验，体系结构实验，性能实验，shell实验，malloc实验，代理实验。争取都做了吧！

数据实验具体如下：函数srl使用算术（由值xsta给出）来执行逻辑右移，紧跟着的是其他不包括右移或者除尘的运算。函数sta使用逻辑(值由xstrl给出)来执行算术右移，紧跟着的是其他不包括右移或者除法的运算。可以假设int是32位长的，移位量k的取值范围是0~31.

一般而言，机器支持两种形式的右移：逻辑右移和算法右移。逻辑右移在左端补k个0，算术右移是在左端补k个最高有效位的拷贝。所以我们只要设计一个掩码就好，还是比较简单的。

`
unsigned srl(unsigned x, int k)
{
    /*Perform shift arithmetically */
    unsigned xsra = (int) x >> k;
    /* Make mask of low order 32-k bits*/
    unsigned mask = k ? ((1<<(32-k))-1) : ~0;

    return xsra && mask;
}
`

`
int sra(int x, int k)
{
    /*Perform shift logically*/
    int xsrl = (unsigned) x >> k;
    /*Make mask of high order k bits*/
    unsigned mask = k ? ~((1<<(32-k))-1) : 0;

    return (x<0) ? mask | xsrl : xsrl;
}
`

关于移位运算还有一点要注意是优先级问题：1<<5-1等价于1<<(5-1)而不是(1<<5)-1.