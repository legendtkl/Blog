title: 'python: PyPDF2'
tags:
  - PyPDF
  - Python
id: 460
categories:
  - Python
date: 2015-05-08 13:46:43
---

今天介绍一个可以处理pdf的python包：PyPDF2。

进入正题之间先来了解一下pdf文件格式。pdf是Portable Document Format的简称，也就是便携式文档格式，由Adobe公司推出。pdf文件格式可以简单表示如下：

[![pdfcomponent](http://www.legendtkl.com/wp-content/uploads/2015/05/pdfcomponent.jpg)](http://www.legendtkl.com/wp-content/uploads/2015/05/pdfcomponent.jpg)

<!--more-->Objects：pdf文档由于data object的集合组合。File structure决定了objects在pdf中以怎样的方式存储。Document structure是pdf文档的组织结构。Content stream也就是我们看到的数据，比如文字、图像什么的，一般是压缩的，常见的压缩方式有zip等。其中document structure如下图所示。

[![structureofpdfdocument](http://www.legendtkl.com/wp-content/uploads/2015/05/structureofpdfdocument.jpg)](http://www.legendtkl.com/wp-content/uploads/2015/05/structureofpdfdocument.jpg)

有了这些信息我们就可以处理pdf文档了。PyPDF2是PyPDF的升级版，github主页在此[github](https://github.com/colemana/PyPDF2)。clone到本地，用setup就可以安装了。
[cce_python]python setup.py install[/cce_python]
项目主页里已经提供了很多示例代码，很简单，比如多个pdf文件合并。
<pre>[cce_python tab_size="4"]
from PyPDF2 import PdfFileMerger

merger = PdfFileMerger()

input1 = open("document1.pdf", "rb")
input2 = open("document2.pdf", "rb")
input3 = open("document3.pdf", "rb")

# add the first 3 pages of input1 document to output
merger.append(fileobj = input1, pages = (0,3))

# insert the first page of input2 into the output beginning after the second page
merger.merge(position = 2, fileobj = input2, pages = (0,1))

# append entire input3 document to the end of the output document
merger.append(input3)

# Write to an output PDF document
output = open("document-output.pdf", "wb")
merger.write(output)
[/cce_python]</pre>