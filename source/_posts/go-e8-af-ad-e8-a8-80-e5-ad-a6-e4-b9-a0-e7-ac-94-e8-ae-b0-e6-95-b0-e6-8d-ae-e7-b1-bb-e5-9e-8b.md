title: 'Go语言学习笔记:数据类型'
tags:
  - go
  - 数据类型
id: 409
categories:
  - go
  - 编程语言
date: 2014-10-31 21:59:14
---

go语言已经出来5年了，最近才开始学习，真是惭愧。

如果要用一个词来形容go语言的话，那么没有比高并发更合适的。levelDB就是用go语言来实现的。

<!--more-->下面主要看下go语言的数据类型

*   布尔型：bool，取值true, false
*   整型：int/uint 32位或64位
*   8位整型：int8/uint8
*   字节型：byte(unit8别名)
*   16位整型：int16/uint16
*   32位整型：int32(rune)/uint32
*   64位整型：int64/uint64
*   浮点型：float32/float64 小数点精确到7/15位
*   复数：complex64/complex128
*   足够保存指针的32位和64位整数型：uintptr
*   其它值类型：array, struct, string
*   引用类型：slice, map, chan
*   接口类型：interface
*   函数类型：func。函数是可以赋值给变量的

### 单个变量的声明与赋值有如下三种方式：

[cce_cpp tab_size="4"]
//1
var x int
x = 42

//2
var x int = 42

//3
x := 42
[/cce_cpp]
其中要注意的是第三种只能用于局部变量，而不能用于全局变量。第二种方式可以省略类型名，也就是采用类型自动推断。

### 多个变量的声明方式：

[cce_cpp tab_size="4"]
//1
var (
	x int = 1
	y int = 2
	z int = 3
)

//2
var a, b, c int = 1, 2, 3
[/cce_cpp]</pre>
其中第一种方式不能用于局部变量。第二种方式很有用，比如交换两个变量的值

[cce_cpp tab_size="4"]
var a = 1
var b = 2
a, b = b, a
[/cce_cpp]</pre>
go语言常量支持字符型，字符串型，布尔型和数字型。常量的值在编译时就已经确定，定义格式与变量基本相同。定义常量组时，如果不提供初始值，则表示将使用上行的表达式，当然两行声明的常量个数必须是相同的。值得一提的是iota，常量行数计数器，从0开始，定义常量组时，每定义一个常量自动递增。每遇到一个const关键字,iota就会重置为0.

[cce_cpp tab_size="4"]
const (
	A = 1
	B
	C = iota
	D
)
const E = iota
const F = iota
[/cce_cpp]</pre>
B没有提供初始值，则使用A的右值，也就是B=1。C=iota，行数计数器，也就是2。D使用C的右值，也就是D=iota，则D=3。每个const中的iota都是从0开始，所以E=0, F=0。

### 数组与指针。

go语言中的数组是值类型，也就是说，如果将数组作为函数的参数，那么实际传递的参数是一份数组的拷贝，而不是指向数组的指针。数组的声明以及赋值如下

[cce_cpp tab_size="4"]
var A [4]int
//now A = {0,0,0,0}
A = [4]int{1,2,3,4}
var B = [4]int{1,2,3,4}
var C = [...]int{1,2,3,4}
var D = [20]int{18:1, 19:2}
[/cce_cpp]</pre>
D实现对数组指定下标进行赋值。指针和C语言指针基本一样，但不使用"-&gt;"，而是直接采用“."选择符来操作指针。&amp;访问地址，*访问值。那么一个喜闻乐见的问题来了。区分指针数组与数组指针。指针数组是数组，数组的元素是指针；数组指针，指向数组。定义如下

[cce_cpp tab_size="4"]
//指针数组
var x, y = 1, 2
var a = [...]*int{&amp;x, &amp;y}
//数组指针
var z = [...]int{x, y}
var b *[2]int = &amp;z
[/cce_cpp]</pre>
值得注意的是go语言中不同长度的数组类型是不一样的，所以声明数组指针的时候一定要注意。

### 内置类型：slice, map, channel

slice类似于python中的切片。可以理解为一个指向数组的指针。而且这个指针还有两个域：长度，容量。长度为当前元素个数，容量为最大元素个数，类似于STL中vector的size和capacity。正常情况下slice和数组共享一段存储空间，当我们对slice操作使得slice超出数组大小时，则slice会重新分配空间。也可以用make为slice分配空间。

[cce_cpp tab_size="4"]
a := [10]int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

slice1 := a[5:10]
fmt.Println(slice1)
//[6,7,8,9,10]
a[5] = 10
fmt.Println(slice1)
//[10,7,8,9,10]
slice1 = append(slice1, 100)
fmt.Println(slice1)
//[10,7,8,9,10,100]
a[6] = 11
fmt.Println(slice1)
//[10,7,8,9,10,100]
[/cce_cpp]</pre>
map关联数组，两个域key, value。我们知道关于map的实现有基于树的实现，也有基于hash表的实现，那么go语言中是怎么来实现的呢？有空还是要去看下runtime源码。go语言中的map是用hash表来实现的，hash冲突是用开链的方式解决的。下面写下map的常见用法。

[cce_cpp tab_size="4"]
mymap := make(map[int]int)
mymap[10] = 10 * 10
mymap[5] = 5 * 5
mymap[15] = 15 * 15

for k, v := range mymap {
	fmt.Println(k, v)
}
[/cce_cpp]</pre>
channel主要用于goroutine，下次再说吧。