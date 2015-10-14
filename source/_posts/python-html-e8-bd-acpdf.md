title: 'python: html转pdf'
tags:
  - pdfcrowd
  - Python
  - xhtml2pdf
id: 448
categories:
  - Python
date: 2015-05-07 10:22:36
---

这里介绍两个可以把html转成pdf的python包：xhtml2pdf和pdfcrowd。

xhtml2pdf的github主页在这：[xhtml2pdf](https://github.com/chrisglass/xhtml2pdf)。安装建议不要按他说的。可以通过pip安装。安装pip之前有些人可能还需要安装一下python-setuptools。
[cce_php]
$ sudo apt-get install python-setuptools
$ sudo easy_install pip
$ sudo pip install xhtml2pdf
[/cce_php]

<!--more-->
如果还有问题，大概也只能debug了。根据编译错误提示，看看是不是缺少什么文件，比如python-dev什么的。具体代码的例子，github主页中的test文件夹下面很全，比如simple.py。
[cce_python tab_size="4"]
def testSimple(
data="""Hello &lt;b&gt;World&lt;/b&gt;&lt;br/&gt;&lt;img src="img/test.jpg"/&gt;""",
dest="test.pdf"):

"""
Simple test showing how to create a PDF file from
PML Source String. Also shows errors and tries to start
the resulting PDF
"""

pdf = pisa.CreatePDF(
cStringIO.StringIO(data),
file(dest, "wb")
)

if pdf.err:
dumpErrors(pdf)
else:
pisa.startViewer(dest)

[/cce_python]
但是，xhtml2pdf包还有些缺陷，不知道是bug，还是本身就没有设计。暂且按下不表。

另外要介绍的是pdfcrowd，主页在此[pdfcrowd](https://pdfcrowd.com/html-to-pdf-api/)。安装直接pip安装。使用需要注册得到一个API Key。示例代码如下。
[cce_python]
import pdfcrowd

try:
# create an API client instance
client = pdfcrowd.Client("username", "apikey")

# convert a web page and store the generated PDF into a pdf variable
pdf = client.convertURI('http://www.google.com')

# convert an HTML string and save the result to a file
output_file = open('html.pdf', 'wb')
html="&lt;head&gt;&lt;/head&gt;&lt;body&gt;My HTML Layout&lt;/body&gt;"
client.convertHtml(html, output_file)
output_file.close()

# convert an HTML file
output_file = open('file.pdf', 'wb')
client.convertFile('/path/to/MyLayout.html', output_file)
output_file.close()

except pdfcrowd.Error, why:
print 'Failed:', why
[/cce_python]
值得一说的是converURI函数，只是将结果存入pdf变量中并没有保存，如何保存呢？查看API Reference如下。

[![pdfcrowd](http://www.legendtkl.com/wp-content/uploads/2015/05/pdfcrowd.jpg)](http://www.legendtkl.com/wp-content/uploads/2015/05/pdfcrowd.jpg)

可以看到converURI函数是通过outstream导出的，outstream可以说任何有write方法的对象，那么是不是可以使用file呢？
[cce_python]client.converURI("http://www.legendtkl.com", file("html.pdf","wb"))[/cce_python]

html.pdf文件预览如下：

[![htmlpdf](http://www.legendtkl.com/wp-content/uploads/2015/05/htmlpdf.jpg)](http://www.legendtkl.com/wp-content/uploads/2015/05/htmlpdf.jpg)

&nbsp;