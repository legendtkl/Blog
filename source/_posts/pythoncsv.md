title: 'python:csv'
tags:
  - csv
  - Python
id: 492
categories:
  - Python
date: 2015-09-11 22:22:26
---

今天要处理下手机通讯录，然后发现可以导出成CSV文件。直接处理csv文件就可以了。直觉上python应该可以，一查果然，哈哈。下面介绍一下python的csv模块。

csv模块可以对csv模块进行读写操作。reader和writer分别负责读写序列，也可以用DictReader和DictWriter实现字典形式读写。

<!--more-->

csv.reader(csvfile[, dialete='excel',] [**fmtparams])返回一个迭代csvfile的行的reader object。

参数：

*   csvfile：支持迭代器的对象。所谓迭代器，需要支持__iter__(返回迭代器对象本身)和__next__(返回下一个)。csvfile的next方法需要返回string。
*   dialect：编码风格，默认为“,”分隔，另外csv模块也支持tab分隔和自定义分隔符。
*   fmtparam：格式化参数，用来覆盖之前dialect对象指定的编码风格
举个例子，很简单
[cce_js]
import csv

reader = csv.reader(file('filename.csv','rb'))
for line in reader:
    print line
[/cce_js]
这里的line对应的是一个list，通过索引我就可以对某个字段进行炒作。

csv.writer(csvfile[, dialect='excel'][, fmtparam]),类似reader，对应写文件。
[cce_js]
import csv

writer = csv.writer(file('filename.csv','wb'))
writer.writerow(['1','2','3','4'])
[/cce_js]
csv.DictReader()也是读csv文件，区别在于返回的是dict类型，而不是list。dict的key可以自己指定，如果不指定的话，默认为第一行。

csv.DictWriter()类似，看上去也挺简单的。就不写了。