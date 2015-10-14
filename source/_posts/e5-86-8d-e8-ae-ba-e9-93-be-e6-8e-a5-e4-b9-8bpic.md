title: 再论链接之PIC
tags:
  - PIC
id: 213
categories:
  - C/C++
date: 2013-11-08 17:16:05
---

动态链接使用共享库的一个主要目的就是允许多个正在运行的进程共享存储器中相同的库代码，因而节约资源。那么，多个进程是如何共享一个程序的一个拷贝的呢？一种比较好的方法就是编译库代码，使得不需要链接器修改库代码，就可以在任何地址加载和执行这些代码。这样的代码就叫做位置无关的代码（position-independent code, PIC）。

PIC的特点是，它被加载到任意地址空间都可以正确的执行，这点是显著区别于可重定向文件的。其原理是PIC对常量和函数入口地址的操作都是基于PC+偏移量的寻址方式。即使程序被移动，但是PC也变化了，而偏移量是不变的，所以程序仍然可以找到正确的入口地址或者常量。那些引用了绝对内存地址的指令（比如绝对跳转指令）必须被替换为PC相对寻址指令。这些间接处理过程可能导致PIC的运行效率下降，但是目前大多数处理器对PIC都有很好的支持，使得这效率上的这一点点下降基本可以忽略。下面具体说明。

<!--more-->

1.PIC数据引用

由于无论我们在存储器中的何处加载一个目标模块，数据段总是分配为紧随在代码段后面。因此，代码段中任何指令和数据段中的任何变量之间的距离都是一个运行时常量，与代码段和数据段的绝对存储位置是无关的。基于上面的事实，编译器在数据段开始的地方创建了一个全局偏移量表（global offset table, GOT）。GOT包含每个被这个目标模块引用的全局数据目标的表目。编译器还为GOT中每个表目生成一个重定位记录。在加载时，动态链接器会重定位GOT中的每个表目，使得它包含正确的绝对地址。引用全局变量的代码形式可以表示如下
<pre>[cce_cpp tab_size="4"]
    call L1
L1: popl %ebx;            #ebx contains the current PC
    addl $VAROFF, %ebx    #ebx points to GOT
    movl (%ebx), %eax     #reference indirect through GOT
    movl (%eax), %eax
[/cce_cpp]</pre>
2.PIC函数调用

对函数调用当然也可以用上面的方法。但是，ELF编译系统使用了一种有趣的技术，叫做延迟绑定（lazy binding），将过程地址的绑定推迟到第一次调用该过程时。第一次调用过程的运行时开销很大，但是其后的每次调用都只会花费一条指令和一个间接的存储器引用。延迟绑定是通过GOT和PLT（procedure linkage table, 过程链接表）交互实现。交互比较复杂，下次有空再说。

Happy Weekend!

参考：《深入理解计算机系统》

Loading and overlays  [http://www.iecc.com/linker/linker08.html](http://www.iecc.com/linker/linker08.html)