title: 关于C++中static以及virtual的理解
tags:
  - static
  - virtual
  - 构造函数
  - 析构函数
id: 397
categories:
  - C/C++
  - 编程语言
date: 2014-09-30 14:52:02
---

<span style="font-size: medium;">首先声明一下，这篇不是入门教程，前提是你了解了static的作用以及C++中的virtual函数的实现机制。:)</span>

<span style="font-size: medium;">先来几个问题，大家思考一下，类的析构函数可以为virtual吗？类的构造函数可以为virtual吗？类的构造函数可以为static吗？类的析构函数可以为static吗？virtual函数可以为static吗？</span>

<span style="font-size: medium;">static关键字的作用可以简单概括为：应用于全局变量时候，应用于局部变量的时候，应用于函数的时候以及在C++中应用于类的时候。前面三种写的都比较多，我今天主要写下static应用在C++类的时候，以及virtual关键字。</span>

<span style="font-size: medium;">static在类中的运用简单来说两种：静态数据成员和静态成员函数。</span>

<!--more-->

<span style="font-size: medium;">静态数据成员被编译器提出与class之后，并被视为一个global变量，但只在class生命范围内可见。每个静态数据成员只有一个实体，存放在程序的数据段之中。可以用static修饰局部变量来对比理解。访问虽然可以通过'.'运算符来调用，但是并不是必须的，也就是说可以通过作用域操作符"::"来调用，比如class A有一个静态数据成员z可以A::z调用。值得注意的一点是，类的静态数据成员要在全局下进行定义，才能使用。否则编译器会报错：undefined reference。也就是说，静态成员在class中声明，外部是看不见的。</span>

<span style="font-size: medium;">在说静态成员函数之前，要说下成员函数的调用方式。先说非静态成员函数吧。为了保证非静态成员函数至少和一般的外部函数有相同的效率，编译器会对非静态成员函数进行改写为一般的函数。主要有以下转化步骤：</span>

1.  <span style="font-size: medium;">改写函数原型，安插一个额外的参数this指针，用以提供一个存取管道，使类对象得以调用该函数。</span>
2.  <span style="font-size: medium;">对函数体中类的非静态数据成员的存取操作，改为经由this指针来存取。</span>
3.  <span style="font-size: medium;">将成员函数重新写成一个外部函数，对函数名称进行“mangling”处理，避免符号冲突。</span>
<span style="font-size: medium;">举个例子如下：</span>
<pre><span style="font-size: medium;">[cce_cpp tab_size="4"]
class Legend{
private:
    int x;
    int y;
    int z;
public:
    legend(){}
    ~legend(){}
    int sum() const{
        int ret = x+y+z;
        return ret;
    }
};
//内部转化
void sum_3legendFv(const legend *const this, int &amp;__result){
    int ret = this-&gt;x+this-&gt;y+this-&gt;z;

    __result = ret;

    return;
}
[/cce_cpp]</span></pre>
<span style="font-size: medium;">感受一下思想，其中引入了NRV（named returned value），先不要太在意，知道有这个东西就好了。</span>

<span style="font-size: medium;">虚成员函数是通过虚函数表来访问的。虚函数的实现机制简单来说就是，类为所有的虚函数建一个虚函数表vtbl，每个类只有一个。每个对象有一个虚函数表指针，vptr，通过指针来访问。因为访问说到底就是找到函数地址。之所以多态无法在编译器间确定，而要在运行时才能确定就是因为在编译器无法确定指针或者引用指向的真正的类型，从而无法确定偏移找到函数地址。</span>
<pre><span style="font-size: medium;">[cce_cpp tab_size="4"]
prt-&gt;normal()
//内部转化
(*ptr-&gt;vptr[1])(ptr)
[/cce_cpp]</span></pre>
<span style="font-size: medium;">其中vptr表示由编译器产生的指针，指向virtual table。1是virtual table slot的索引值，关联到normal函数，第二个ptr是this指针。</span>

<span style="font-size: medium;">静态成员函数并不会关联到this指针，因此差不多等同于外部函数。主要有下面三个特性</span>

1.  <span style="font-size: medium;">不能直接存取其class内的非静态成员</span>
2.  <span style="font-size: medium;">不能够直接声明为const</span>
3.  <span style="font-size: medium;">不需要通过对象来调用。</span>
<span style="font-size: medium;">下面我们来看上面几个问题</span>

<span style="font-size: medium;">1.类的析构函数可以为virtual吗？毋庸置疑，对于可能作为基类的类的析构函数要求就是virtual的。因为如果不是virtual的，派生类析构的时候调用的是基类的析构函数，而基类的析构函数只要对基类部分进行析构，从而可能导致派生类部分出现内存泄漏问题。</span>

<span style="font-size: medium;">2.类的构造函数可以为virtual吗？答案也是不能的，通过上面虚函数的调用方式我们知道虚函数是通过vptr来访问的。那么vptr是怎么来的呢？vptr确定通过构造函数来初始化的。鸡生蛋，蛋生鸡，鸡生蛋……</span>

<span style="font-size: medium;">3.类的构造函数可以为static吗？根据之前说的static不能访问非静态成员变量这点可以知道构造函数是不可以为static的。两者static是对应于每个类的，而构造函数主要负责初始化对象的。这里要提一下C#中的static构造函数是用于在使用类之前进行相关的初始化工作。比如，初始化静态成员或执行特定操作。CLR在第一次创建该类对象或调用该类静态方法时自动调用静态构造函数。</span>

<span style="font-size: medium;">4.类的析构函数可以为static吗？同上。</span>

<span style="font-size: medium;">5.virtual函数可以为static吗？答案是不可以。virtual函数和static函数访问方式是不一样。</span>