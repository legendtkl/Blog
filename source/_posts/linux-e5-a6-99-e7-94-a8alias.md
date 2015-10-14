title: linux妙用alias
tags:
  - alias
id: 440
categories:
  - linux
date: 2015-05-06 17:12:55
---

写篇短文回去吃饭:)

使用linux的过程中会发现很多奇淫技巧，比如alias。alias是用来设置命令的别名。比如ll命令其实就是ls -al的别名。打开.bashrc可以发现一些例子。比如ubuntu 14.04的.bashrc中程序片段。

[![alias](http://www.legendtkl.com/wp-content/uploads/2015/05/alias.jpg)](http://www.legendtkl.com/wp-content/uploads/2015/05/alias.jpg)

<!--more-->

哇，使用alias可以让我们少巧几个字符～（笑）

我大概搜了一下中文的博客对于alias的介绍大概就到上面，所以我才写了这篇文章。考虑两个最简单的命令mkdir和cd，怎样将她们组合起来呢？

[cce_cpp]mkdir mydir cd mydir[/cce_cpp]

问题转换一下就是如何给alias传参数。其实csh是可以的，但是bash不可以。我们可以定义一个函数，然后再使用alias。
[cce_cpp]
myfoo() {
    mkdir $1
    cd $1
}

alias mkcd=myfoo
[/cce_cpp]
也可以这样。
[cce]alias mkcd='_(){ mkdir $1; cd $1; }; _'[/cce]
对了，再我们编译的使用，如果有外部库的情况下，命令会很长。比如编译下面的C语言片段。
[cce_cpp]
#include &lt;math.h&gt;
#include &lt;stdio.h&gt;

int main(){
    double root = sqrt(9.0);
    printf("root of 9 is %lf\n", root);
    return 0;
}
[/cce_cpp]
由于sqrt函数不在默认程序库里，需要用外部链接库来编译。
[cce_cpp]
gcc foo.c -o foo /usr/lib/libm.a
或者
gcc foo.c -o foo -lm
[/cce_cpp]
同样，我们可以使用alias来搞定，具体怎么做已经很明显了。吃饭去。

&nbsp;